# Agent & Tools Code Review — Alizé Backend

**Reviewer:** Senior Code Review  
**Date:** 2026-03-26  
**Files Reviewed:**  
- `agent/prompt-builder.ts`
- `agent/mastra.ts`
- `agent/tools/doc-tools.ts`
- `agent/tools/memory-tools.ts`
- `agent/tools/skill-tools.ts`
- `agent/tools/cron-tools.ts`
- `agent/tools/search-docs.ts`
- `agent/tools/deep-research.ts`
- `agent/skills/loader.ts`

---

## Executive Summary

The agent/tool architecture is reasonably well-structured with good separation of concerns. However, there are several bugs, missing error handling, performance concerns, and type safety gaps that should be addressed. Most critically, the cron-tools have a data integrity bug and the deep-research tool's "web" toolset is empty.

---

## File-by-File Analysis

### 1. `agent/prompt-builder.ts`

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 21 | **Memory leak — `turnCounters` Map never cleaned**: `turnCounters` is a module-level `Map<string, number>`. `clearTurn()` exists but is only called if explicitly done by the caller. If conversations end without calling `clearTurn()` (errors, abandoned sessions, forgotten cleanup), the Map grows indefinitely. |
| 47 | **`Promise.all` has no partial failure handling**: If any of the 4 parallel loaders (`loadSoul`, `loadOrgMemory`, `loadUserProfile`, `loadRelevantSkills`) throws, the entire `buildSystemPrompt` throws and the agent crashes with no graceful fallback. |

#### Performance Issues

| Line | Issue |
|------|-------|
| 40-43 | **No caching — every turn hits DB 4 times**: All four loaders query the database on every single turn. For a chat with many turns, this is expensive. Consider caching with TTL or conversation-scoped cache. |

#### Type Safety

| Line | Issue |
|------|-------|
| 17 | `PromptContext.conversationId` is optional (`string \| undefined`), and the code at line 35 handles this with `ctx.conversationId ? incrementTurn(...) : 0`. Correct but relies on runtime check. |

#### Design Issue

| Line | Issue |
|------|-------|
| 47 | `loadRelevantSkills` is called every turn even though its result (loaded skills) rarely changes mid-conversation. Consider caching skills at conversation start. |

---

### 2. `agent/mastra.ts`

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 49-56 | **`requestContext` cast to `Record<string, string>` is unsafe**: If `orgId` or `userId` is missing from the context, it silently becomes `undefined` and the agent continues with a malformed prompt. No validation that required context keys exist before building the prompt. |
| 58 | **No handling of empty `messages` array**: `messages?.[messages.length - 1]` returns `undefined` if messages is empty. `typeof undefined.content === 'string'` is `false`, so `userMessage` becomes `undefined`. This is passed to `buildSystemPrompt`. The code handles `userMessage?: string` but it's worth noting the agent gets no user message context. |

#### Export Chaos

| Line | Issue |
|------|-------|
| 1-14 | Tools are imported from `./tools/index.js` (barrel). But tools are **also** re-exported from `prompt-builder.ts` (lines 76-82). This creates confusing multiple export paths. Decide on a single canonical export location. |

---

### 3. `agent/tools/doc-tools.ts`

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 36 | **`orgId` silently undefined if context missing**: `const orgId = (requestContext as Record<string, string>).orgId` — if `orgId` is absent from context, this is `undefined` and the SQL query `WHERE org_id = $2` uses `undefined`, silently returning no results. No validation. |
| 49 | **Hardcoded `maxChars = 8000` — mid-sentence truncation**: `fullText.slice(0, 8000)` truncates arbitrarily in the middle of a sentence. No word-boundary awareness. |
| 71 | **No validation of `documentId` ownership**: The query checks `org_id = $2` but doesn't verify the document belongs to the requesting org (though it does implicitly via org_id). |

#### Performance Issues

| Line | Issue |
|------|-------|
| 38-40 | **Fetches ALL chunks with no pagination**: `SELECT content, chunk_index FROM document_chunks WHERE document_id = $1 AND org_id = $2 ORDER BY chunk_index` — for very large documents (e.g., 500+ pages), this could return thousands of chunks. Consider chunk streaming or pagination. |

#### Error Handling

| Line | Issue |
|------|-------|
| 43-44 | Returns `{ error: '...' }` on not-found, which is fine for tool design, but callers need to handle this consistently. |

---

### 4. `agent/tools/memory-tools.ts`

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 17-28 | **`readMemoryTool` — no org-scoped check for user profiles**: When `target === 'user'`, the query `SELECT ... FROM user_profiles WHERE user_id = $1` doesn't verify the user belongs to the org in context. Any user in the system could read any other user's profile if their ID is known. |
| 45-47 | **`writeMemoryTool` — same org-scoping issue**: The `ON CONFLICT (user_id) DO UPDATE` doesn't check `org_id`, so a user could overwrite another org's user profile if their IDs collide (unlikely but possible). |

#### Error Handling

| Line | Issue |
|------|-------|
| 22, 30 | If `sqlOne` throws (DB error), it propagates up with no catch. Consider wrapping in try-catch for graceful degradation. |

---

### 5. `agent/tools/skill-tools.ts`

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 23-28 | **Slug collision — no uniqueness check**: Two skills with the same name get the same slug. The INSERT uses the slug as a value but doesn't check for duplicates. This will either fail on a DB constraint or (if no constraint exists) create a duplicate. |
| 27 | **`embedQuery` call has no error handling**: If the embedding API fails (network, rate limit, invalid input), the entire tool throws with no graceful error message to the user. |
| 61 | **`improveSkillTool` uses fuzzy ILIKE match**: `WHERE org_id = $1 AND name ILIKE $2` with `%${input.skillName}%` could match multiple skills (e.g., "contract" matches both "Contract Analysis" and "Contract Renewal"). Only the first result is updated. |
| 62 | **If no skill found, returns `{ error: 'Skill not found' }`**: But this is a string property on the return object, not a thrown error. Consistent with doc-tools pattern but the caller must know to check for `error` property. |

#### Performance Issues

| Line | Issue |
|------|-------|
| 22, 48 | `embedQuery` is awaited. Two separate embedding API calls are made (one in `createSkillTool`, one in `improveSkillTool`). These could be batched if both are called together. |

---

### 6. `agent/tools/cron-tools.ts`

#### ⚠️ CRITICAL BUG — Data Integrity

| Line | Issue |
|------|-------|
| 27 | **`schedule` and `cron_expr` both bound to `$5`**: `VALUES ($1, $2, $3, $4, $5, $5, $6, $7)` — the `schedule` column gets `input.cronExpr` (correct) but so does `cron_expr` (correct), yet `schedule` is also `$5`. This means `schedule` and `cron_expr` are identical, which seems intentional but redundant. However, looking at line 28, `schedule` and `cron_expr` are both `$5`. This is likely a copy-paste error where one of them should be a different parameter (e.g., a human-readable schedule description derived from the cron expr). |

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 35-39 | **Silent failure if `_cronQueue` is null**: If `setCronQueue` hasn't been called, `_cronQueue` is `null`. The INSERT succeeds, the tool returns `{ success: true, ... }`, but the cron **will never run** because no BullMQ scheduler was registered. The user gets no indication this failed. |
| 60-62 | **Race condition in `deleteCronTool`**: `removeJobScheduler` is called before the DB DELETE. If the DB delete fails, the scheduler is gone but the record remains. Should reverse order: delete from DB first, then remove scheduler. |
| 34 | **No validation of `cronExpr`**: Malformed cron expressions (e.g., `"not a cron"`) are accepted and stored. BullMQ will fail to schedule them silently. |

#### Error Handling

| Line | Issue |
|------|-------|
| 34 | `upsertJobScheduler` could throw (BullMQ error). If it throws, the DB insert succeeded but the scheduler didn't register. No rollback. |

---

### 7. `agent/tools/search-docs.ts`

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 22 | **`documentIds` filter not validated**: `documentIds` is passed directly to `searchChunks`. If a user provides document IDs from another org, they might get results (depends on `searchChunks` implementation). Should verify ownership. |

#### Performance Issues

| Line | Issue |
|------|-------|
| 21 | **No query result caching**: Same query re-embedded every time. Consider caching embeddings for repeated queries within a conversation. |

#### Error Handling

| Line | Issue |
|------|-------|
| 22, 26 | `embedQuery` or `searchChunks` could throw. No try-catch. |

---

### 8. `agent/tools/deep-research.ts`

#### ⚠️ CRITICAL — Empty "web" Toolset

| Line | Issue |
|------|-------|
| 33 | **`toolMap['web']: {}` — empty object**: The TODO confirms this: `// TODO: add webSearchTool, fetchUrlTool`. Web research tasks get an agent with **zero tools**. The agent can only respond with hardcoded text. All "web" research will be useless. |

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 47 | **JSON parse error returns generic message**: `return { error: 'Failed to generate research plan' }` — no details about why. Hard to debug in production. |
| 50 | **No validation of `plan.tasks`**: If the planner returns `{ tasks: undefined }` or an empty array, the Promise.all over `plan.tasks.map()` will fail or do nothing. No validation. |
| 75 | **Silent JSON parse failure on evaluation**: `catch { /* use default */ }` — if evaluation JSON parse fails, `sufficient: true` is assumed even if research is incomplete. This could produce misleading reports. |
| 57-65 | **Each sub-agent gets `requestContext` passed to `generate()`**: Mastra may not forward `requestContext` to sub-agents correctly. If `requestContext` isn't forwarded, sub-agents have no `orgId`, breaking all tools that depend on it. |

#### Performance Issues

| Line | Issue |
|------|-------|
| 31, 67, 78 | **4 separate Mastra Agent instances per call**: `planner`, `evaluator`, `synthesizer` + per-task researchers. Creating Mastra agents is expensive. Consider reusing agent instances or using a pool. |

---

### 9. `agent/skills/loader.ts`

#### Bugs / Logic Errors

| Line | Issue |
|------|-------|
| 41 | **Similarity calculation may produce negative values**: `1 - (embedding <=> $2::vector)` — `<=>` is the pgvector "distance" operator. For high-dimensional embeddings, L2 distance can exceed 1. So `1 - distance` can be negative, producing meaningless similarity scores. Should use `GREATEST(0, 1 - (embedding <=> $2::vector))` or use cosine similarity (`<~>`). |
| 23-27 | **Fire-and-forget UPDATE swallows errors**: `sql('UPDATE ...').catch(() => {})` silently ignores all errors. If the UPDATE fails consistently (e.g., bad SQL), you'll never know and skill usage stats are wrong. |

#### Type Safety

| Line | Issue |
|------|-------|
| 10 | `rows` typed as `{ id: string; name: string; content: string }[]` but the SQL query also selects `relevance` (line 41). TypeScript ignores extra fields but it's inconsistent typing. |

#### Performance Issues

| Line | Issue |
|------|-------|
| 23-27 | **N separate UPDATE queries per skill load**: With `MAX_LOADED_PER_TURN = 3`, up to 3 fire-and-forget UPDATE queries per turn. Could be batched into a single query: `UPDATE skills SET times_used = times_used + 1, last_used_at = now() WHERE id = ANY($1)`. |

---

## Overall Architecture Assessment

### ✅ What's Good

1. **Clean separation**: Tools are domain-organized (doc-tools, memory-tools, skill-tools, etc.)
2. **Parallel loading in prompt builder**: `Promise.all` for all 4 loaders is efficient
3. **Skill semantic matching**: Using vector similarity for skill retrieval is appropriate
4. **Chunked document storage**: Documents stored as chunks for efficient retrieval
5. **Fault isolation in deep-research**: Individual task failures don't crash the whole research

### ❌ Issues

1. **No input validation on context**: `orgId`, `userId` extracted from `requestContext` without checking existence
2. **Inconsistent error handling**: Some tools return `{ error: '...' }`, some throw
3. **No request context validation**: If context is malformed/missing keys, failures are silent
4. **Missing tools**: Web search, URL fetch, email, calendar tools are absent but referenced (e.g., deep-research web toolset is TODO)
5. **Cron data integrity bug**: `schedule` and `cron_expr` both get the same parameter — likely a bug
6. **Silent cron registration failure**: When `_cronQueue` is null, cron appears to succeed but won't run
7. **Memory leak risk**: `turnCounters` Map grows indefinitely if `clearTurn` isn't called
8. **No caching**: Every turn re-loads soul, memory, profile, skills from DB

### 🔧 Missing Tools I'd Expect for a Business AI Assistant

1. **Web Search Tool** — already identified as TODO in deep-research.ts
2. **URL Fetch Tool** — fetch and parse web pages
3. **Email Tool** — read/send emails (crons deliver to email, but no tool to manage email)
4. **Calendar Tool** — schedule meetings, check availability
5. **HTTP Request Tool** — interact with external REST APIs (Slack, Notion, etc.)
6. **Image Analysis Tool** — if users upload images (receipts, contracts scanned)

### 📊 Prompt Builder Assessment

**Structure: Good** — Layered approach (personality → org memory → user profile → skills → memory nudge) is well-designed and follows good practices for context injection.

**Issues:**
- No caching between turns
- No differentiation between "first turn" and "subsequent turns" in prompt structure
- Memory nudge interval (every 10 turns) is hardcoded

### 🧠 Memory System vs Hermes Patterns

The current implementation is **simpler** than typical Hermes patterns:

| Aspect | Hermes Pattern | Alizé Implementation |
|--------|---------------|---------------------|
| Org/User memory | Vector-backed semantic memory | Simple text in SQL tables |
| Session memory | Mastra Memory with semantic recall | Mastra Memory (good) |
| Skill memory | Vector-stored procedures | Vector-stored (good) |
| Memory consolidation | Automated background jobs | Agent-driven nudge only |
| Memory retrieval | Hybrid (semantic + keyword) | Pure semantic for skills, text match for memory |

**Gap**: No automated memory consolidation job. The agent is told to consolidate via the memory nudge, but there's no scheduled job to actually do it.

---

## Recommendations (Priority Order)

### P0 — Fix Immediately

1. **`cron-tools.ts` line 27**: Verify `schedule` and `cron_expr` should both be `$5`. This looks like a copy-paste bug where one should be derived from the cron expression.

2. **`deep-research.ts` line 33**: Implement web search and URL fetch tools, or remove "web" from the valid `toolset` type until implemented.

3. **`prompt-builder.ts` turn counter leak**: Either add automatic cleanup (TTL-based or LRU cache) or ensure `clearTurn` is called reliably on conversation end.

### P1 — Fix Soon

4. **`cron-tools.ts` line 35**: If `_cronQueue` is null, either throw an error or at minimum return `{ success: false, error: '...' }` instead of `{ success: true }`.

5. **`skill-tools.ts` line 23-28**: Add uniqueness check for slug or add a DB unique constraint and handle the error gracefully.

6. **`memory-tools.ts` lines 22, 30**: Add org-scoped validation when reading/writing user profiles.

7. **`loader.ts` line 41**: Fix similarity calculation to avoid negative values: `GREATEST(0, 1 - (embedding <=> $2::vector))`.

### P2 — Improve

8. **Add input validation**: Validate `orgId`/`userId` exist in context before using them. Add a helper: `getRequiredContext(ctx, 'orgId')`.

9. **Consistent error handling**: Decide on `{ error: '...' }` vs throwing. Document the pattern.

10. **Add caching**: Cache loaded soul, org memory, user profile, and skills at the conversation level with invalidation on write.

11. **Implement missing tools**: Web search, URL fetch, email, calendar.

12. **Add health checks**: For each tool, validate that required dependencies (queues, DB) are available before use.

---

*Review complete.*
