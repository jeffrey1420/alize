# Agent & Tools Layer Audit

**File audited:** `/data/workspace/alize-code/apps/api/src/agent/`
**Date:** 2026-03-26
**Scope:** `tools/`, `skills/`, `mastrz.ts` (mastra.ts), `prompt-builder.ts`, `tools/index.ts`

---

## 1. Tools — Individual Audit

### `search-docs.ts` — `searchDocsTool` ✅

- **Description** ✅ Accurate. Describes semantic search across org documents.
- **Input schema** ✅ Correct: `query` (string), `documentIds` (optional string[]), `limit` (optional number, defaults to 8).
- **Error handling** ✅ Returns `{ found: false, message }` when no results. DB errors would propagate naturally.

**Issue (minor):** `documentIds` filter is passed to `searchChunks` — not verified here whether that function correctly enforces orgId scoping. Worth confirming `searchChunks` includes orgId in its query.

---

### `doc-tools.ts` — `readDocTool` ⚠️

- **Description** ✅ Clear.
- **Input schema** ✅ `documentId: z.string()`.
- **Error handling** ✅ Returns structured error when not found.

**🔴 Security issue:** No orgId verification on `documentId`. The query filters by `document_id` and `org_id` (from context), but since `documentId` is user-controlled and `org_id` comes from `requestContext`, a broken/mismatched context could allow reading another org's document if the documentId happens to exist for that org. Not exploitable if `requestContext.orgId` is always correct — but there's no explicit sanity check that `documentId` belongs to `orgId`.

---

### `doc-tools.ts` — `listDocsTool` ✅

- **Description** ✅ Accurate.
- **Input schema** ✅ `type` (optional), `search` (optional).
- **Error handling** ✅ Parameterized queries prevent SQL injection.

**Note:** Hardcoded `LIMIT 20` with no pagination. OK for MVP but will degrade with large doc counts.

---

### `doc-tools.ts` — `writeDocTool` ✅

- **Description** ✅ Accurate.
- **Input schema** ✅ `title`, `content`, `filename` — all required strings.
- **Error handling** ⚠️ The `_docQueue.add()` call has no `.catch()` — if the queue is null this is handled, but if it's configured and the job fails to enqueue, it throws.

**Dormant code:** `setDocQueue` exists and is imported in `mastrz.ts` but **never called**. The `_docQueue` module variable is therefore always `null`, so the queue branch is dead code. The queue path is never exercised.

---

### `cron-tools.ts` — `createCronTool` ✅

- **Description** ✅ Accurate.
- **Input schema** ✅ All fields present: `name`, `prompt`, `cronExpr`, `skillSlug` (optional), `channel`, `channelRef`.
- **Error handling** ✅ Handles unavailable queue gracefully with a warning in the return value.

**Dormant code:** `setCronQueue` is imported in `mastrz.ts` but **never called**. `_cronQueue` is always null, so `upsertJobScheduler` never runs. The queue warning is always returned.

---

### `cron-tools.ts` — `listCronsTool` ✅

- **Description** ✅
- **Input schema** ✅ Empty `z.object({})` — no params needed.
- **Error handling** ✅

---

### `cron-tools.ts` — `deleteCronTool` ✅

- **Description** ✅
- **Input schema** ✅ `nameOrId: z.string()`.
- **Error handling** ✅ Handles missing job gracefully.

---

### `memory-tools.ts` — `readMemoryTool` ✅

- **Description** ✅
- **Input schema** ✅ `target: z.enum(['org', 'user'])`, `userId` (optional string).
- **Error handling** ✅ Returns `{ content, usage }` even when empty (no error thrown).

---

### `memory-tools.ts` — `writeMemoryTool` ⚠️

- **Description** ✅ Good — explicitly warns about durability and transient content.
- **Input schema** ✅ `target`, optional `userId`, required `content`.
- **Error handling** ⚠️ **Missing `.catch()` on the user profile write.** The `sql()` call for `user_profiles` UPDATE has no error handling. If it throws, the error propagates uncaught. The org path has the same issue.

---

### `skill-tools.ts` — `createSkillTool` ✅

- **Description** ✅ Accurate.
- **Input schema** ✅ `name`, `description`, `content`, `tags` — all required.
- **Error handling** ⚠️ No `.catch()` on the `sql()` INSERT. If it fails (e.g., slug collision), throws.

---

### `skill-tools.ts` — `improveSkillTool` ✅

- **Description** ✅ Accurate.
- **Input schema** ✅ `skillName`, `improvement`, `newContent`.
- **Error handling** ⚠️ Same as above — no `.catch()` on SQL operations.

---

### `deep-research.ts` — `deepResearchTool` ✅

- **Description** ✅ Good — explicitly excludes simple questions.
- **Input schema** ✅ `objective` (required), `context` (optional).
- **Error handling** ✅ Plan parsing failure returns structured error. Sub-agent errors are caught and returned as `{ result: "Error: ..." }`.

---

### `deep-research-tools.ts` — `webSearchTool` ⚠️

- **Description** ✅ Accurate.
- **Input schema** ✅ `query: z.string()`.
- **Error handling** ✅ Catches fetch errors.

**Issue:** Query parameter `&engines=google` — this forces Google engine in SearXNG, which may be blocked or rate-limited. Also, `AbortSignal.timeout(15000)` is good, but there's no retry logic.

---

### `deep-research-tools.ts` — `fetchUrlTool` ⚠️

- **Description** ✅ Accurate.
- **Input schema** ✅ `url: z.string().url()` — URL validation present.
- **Error handling** ✅ Catches fetch errors.

**Issue:** Raw HTML stripping (`replace(/<[^>]*>/g, ' ')`) is too aggressive — removes `<script>`, `<style>` (good) but also removes semantic tags like `<table>`, `<li>`, etc. Returns a jumbled text blob. For research purposes, this is functional but low-quality.

---

## 2. Skills Loader (`skills/loader.ts`) — Issues Found

### 🔴 Silent error swallowing in usage tracking

```ts
sql('UPDATE skills SET times_used = times_used + 1 ...')
  .catch(() => {})
```

This fires-and-forgets the usage update. **All errors are silently swallowed.** If the DB connection is down, or the query is malformed, it fails silently and the usage stats are never updated. For a non-critical feature this is acceptable — but it should at minimum log the error.

### ✅ Semantic matching logic

The skill matching logic is correct:
- No message → load most-used skills (ORDER BY times_used DESC).
- Has message → embed query and use `<=>` (cosine distance) to find nearest skills.
- `topK` of 3 (`SKILLS.MAX_LOADED_PER_TURN`) is reasonable.

### ✅ `loadSkillBySlug` is correct

Proper org-scoped lookup. However, this function is **not exposed as a tool** — it's only callable internally by other code. No issue, just an observation.

---

## 3. `mastrz.ts` (mastra.ts) — Agent Setup Audit

### Model Configuration ✅

```ts
model: MODELS.LARGE,  // → 'mistral/mistral-large-latest'
```

The model name format (`provider/model-name`) is correct for Mastra's OpenAI-compatible model config. No issue here.

**Minor note:** Sub-agents in `deep-research.ts` also use `MODELS.LARGE` and `MODELS.SMALL`. If these model names need to change, both files must be updated.

### Memory Configuration ✅

```ts
const memory = new Memory({
  options: {
    lastMessages: 10,
    workingMemory: { enabled: true },
    semanticRecall: { enabled: true, topK: 3 },
  },
})
```

Configuration looks correct. Semantic recall topK of 3 is reasonable.

### 🔴 Dead imports — `setDocQueue` and `setCronQueue`

```ts
import { createSkillTool, improveSkillTool, deepResearchTool } from './tools/index.js'
```

`setDocQueue` and `setCronQueue` are **imported** in mastra.ts but **never called**. Both functions set module-level variables in their respective tool files, but mastra.ts never invokes them. Result: the queue variables (`_docQueue` / `_cronQueue`) are always null, making the queue branches in `createCronTool` and `writeDocTool` unreachable.

**Fix:** Either call `setDocQueue` and `setCronQueue` during mastra initialization, or remove the dead imports.

### ⚠️ `deepResearchTool` import shape

`deepResearchTool` is a named export from `deep-research.ts`. The import in mastra.ts uses:
```ts
import { deepResearchTool } from './tools/index.js'  // ✅ named export
```
This is correct since `tools/index.ts` re-exports it as a named export.

---

## 4. `prompt-builder.ts` — Memory Loading Audit

### ✅ Parallel loading with `Promise.allSettled`

Good pattern — one source failing doesn't block the others. Each result gets a fallback.

### ✅ Turn counter logic

`incrementTurn` correctly manages per-conversation counters with a Map and max size eviction. Logic is sound.

### ✅ Fallback chain

Each loader falls back to `null` or `DEFAULT_SOUL` correctly. The `getResult` helper is clean.

### ✅ Memory Nudge interval

`AGENT.MEMORY_NUDGE_INTERVAL` (every 10 turns) is checked correctly with `turn % AGENT.MEMORY_NUDGE_INTERVAL === 0`.

### ⚠️ Missing `loadRelevantSkills` call in the Promise.allSettled

Looking at the code:
```ts
const [soulResult, memoryResult, userProfileResult, skillsResult] = await Promise.allSettled([
  loadSoul(ctx.orgId),
  loadOrgMemory(ctx.orgId),
  loadUserProfile(ctx.userId),
  loadRelevantSkills(ctx.orgId, ctx.userMessage),  // ✅ present
])
```
It's present. No issue.

### ⚠️ `requestContext` type casting

```ts
const ctx = requestContext as Record<string, string>
```
This cast is unsafe — if `requestContext` is `undefined` or has unexpected shape, runtime errors occur. Consider a guard or a typed interface.

### ⚠️ `userMessage` extraction is fragile

```ts
const userMessage = typeof lastMessage?.content === 'string'
  ? lastMessage.content
  : undefined
```
Only captures string content. If the model sends a multi-modal message (images, tool calls), `userMessage` will be `undefined`. For skill matching, this means it falls back to most-used skills instead of semantically matching. Acceptable for now but fragile.

---

## 5. `tools/index.ts` — Export Audit

### Exports present:

| Export | Source | Status |
|---|---|---|
| `searchDocsTool` | search-docs.ts | ✅ |
| `readDocTool` | doc-tools.ts | ✅ |
| `listDocsTool` | doc-tools.ts | ✅ |
| `writeDocTool` | doc-tools.ts | ✅ |
| `setDocQueue` | doc-tools.ts | ❌ **NOT exported** |
| `createCronTool` | cron-tools.ts | ✅ |
| `listCronsTool` | cron-tools.ts | ✅ |
| `deleteCronTool` | cron-tools.ts | ✅ |
| `setCronQueue` | cron-tools.ts | ❌ **NOT exported** |
| `readMemoryTool` | memory-tools.ts | ✅ |
| `writeMemoryTool` | memory-tools.ts | ✅ |
| `createSkillTool` | skill-tools.ts | ✅ |
| `improveSkillTool` | skill-tools.ts | ✅ |
| `deepResearchTool` | deep-research.ts | ✅ |

### Unused imports in mastra.ts:

```ts
import { sqlOne } from '#/db/client.js'          // Used in prompt-builder.ts, not here
import { loadRelevantSkills } from './skills/loader.js'  // Used in prompt-builder.ts, not here
import { AGENT, MEMORY_LIMITS } from '#/config/constants.js'  // Used in prompt-builder.ts
import { DEFAULT_SOUL } from '#/prompts/soul.js'   // Used in prompt-builder.ts
import { MEMORY_NUDGE } from '#/prompts/memory-nudge.js'  // Used in prompt-builder.ts
```

All of the above are **imported at the top of mastra.ts** but are only used inside `prompt-builder.ts`. The `mastrz.ts` file only uses them transitively through `buildSystemPrompt`. These imports are dead in mastra.ts itself — they should be removed from mastra.ts and kept only where they're used (or moved to a shared module).

---

## Summary of Findings

| Severity | Issue | Location |
|---|---|---|
| 🔴 High | `setDocQueue` and `setCronQueue` never called — queue branches unreachable | mastra.ts |
| 🔴 High | Dead imports in mastra.ts (sqlOne, loadRelevantSkills, AGENT, etc.) | mastra.ts |
| 🔴 High | `readDocTool` — no explicit orgId×documentId ownership check | doc-tools.ts |
| 🟡 Medium | `writeMemoryTool` — no `.catch()` on SQL writes | memory-tools.ts |
| 🟡 Medium | Skills loader — fire-and-forget errors swallowed silently | skills/loader.ts |
| 🟡 Medium | `setDocQueue`/`setCronQueue` not exported from tools/index.ts | tools/index.ts |
| 🟡 Medium | `fetchUrlTool` — crude HTML stripping degrades content quality | deep-research-tools.ts |
| 🟡 Medium | `listDocsTool` — no pagination (LIMIT 20 hardcoded) | doc-tools.ts |
| 🟡 Medium | `requestContext` unsafe type cast | prompt-builder.ts |
| 🟡 Medium | `webSearchTool` — `engines=google` override in SearXNG may be unreliable | deep-research-tools.ts |
| 🟢 Low | Skills: `loadSkillBySlug` not exposed as a tool (internal use only) | skills/loader.ts |
| 🟢 Low | `userMessage` extraction misses non-string content types | prompt-builder.ts |
| 🟢 Low | Model name `mistral/mistral-large-latest` contains `/` — verify Mastra compatibility | config/constants.ts |
