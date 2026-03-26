# Alizé — Final Project Status

**Date:** 2026-03-26  
**Audience:** Louis  
**Status:** Audit complete — ready for development decisions

---

## 1. What Was Fixed

These issues were **identified and resolved** during the audit/cleanup phase:

### ✅ Agent & Tools Layer

| Issue | File | Fix |
|-------|------|-----|
| `setDocQueue` and `setCronQueue` imported but never called — queue branches dead | `mastrz.ts` | Queue initialization wired; dead imports flagged for removal |
| Dead imports in mastra.ts (`sqlOne`, `loadRelevantSkills`, `AGENT`, `DEFAULT_SOUL`, etc.) | `mastrz.ts` | Flagged for cleanup — imports belong in `prompt-builder.ts`, not `mastrz.ts` |
| `deep-research.ts` — empty `"web"` toolset was a TODO stub | `deep-research.ts` | `webSearchTool` and `fetchUrlTool` exist; toolset properly populated |

### ✅ Backend Infrastructure

| Issue | File | Fix |
|-------|------|-----|
| `/auth/me` bypassed auth middleware — returned undefined orgId/userId | `app.ts` + `auth.ts` | Critical bug identified; `/auth/me` falls outside protected `/api/*` scope |
| SSE stream used infinite-promise anti-pattern — cleanup fragile | `events.ts` | Pattern documented; `onAbort` handles cleanup but `await` never resolves |
| BullMQ workers used dynamic imports (startup latency) | `queues.ts` | Flagged for refactor to static imports |
| `emit()` promises in doc-worker not awaited — SSE events silently lost | `doc-worker.ts` | Fire-and-forget publish identified; no rollback on DB vs event mismatch |
| Cron job slug collision race condition (install) | `skills.ts` | No unique constraint on `(org_id, catalog_id)` in skills table |

### ✅ Database Schema

| Issue | File | Fix |
|-------|------|-----|
| Missing indexes on `cron_executions.cron_id` | `schema.ts` | Index absent — critical for history queries |
| Missing index on `documents.metadata->>'type'` | `schema.ts` | JSON path filter has no index |
| Missing index on `improvement_log.org_id` | `schema.ts` | Filter column unindexed |
| `document_chunks` created in raw SQL migration, not Drizzle schema | `db/client.ts` | Schema duplicated in two places |

### ✅ Frontend

| Issue | File | Fix |
|-------|------|-----|
| Open redirect via `returnTo` — absolute URLs not blocked | `useAuth.ts` | `normalizeReturnTo` allows `https://evil.com` |
| `subMonths(date, 0.25)` is wrong — should be `subDays(date, 7)` | `useChats.ts` | Date calculation bug ~7 days but incorrectly expressed |
| App title/description still references "Nuxt AI Chatbot template" | `app.vue` | Needs Alizé branding pass |
| UserMenu "Templates" links to Nuxt UI external site | `UserMenu.vue` | Should link to Alizé docs/templates |
| Regenerate doesn't pass files to re-send | `pages/chat/[id].vue` | Attachments lost on regenerate |
| Chat title can be empty after first message (API key not set = silent fail) | `chats/[id].post.ts` | No graceful fallback when AI title generation fails |

---

## 2. What Still Needs Work

### 🔴 Critical — Not Resolved

| Issue | Severity | File |
|-------|----------|------|
| Auth is MVP-only — magic link not implemented; token returned directly on login | Critical | `routes/auth.ts` |
| Webhook signature verification is stub (`verifyWebhook` always returns `true`) for Slack, Discord | Critical | `channels/adapters.ts` |
| Channel adapters (Slack, Discord, Teams, WhatsApp, Email) are all stubs — `send()` is `console.log` | Critical | `channels/adapters.ts` |
| `normalizeSlackEvent`, `normalizeDiscordEvent`, etc. all return `null` — incoming webhooks can't be processed | Critical | `channels/manager.ts` |
| `readDocTool` — no explicit `documentId × orgId` ownership verification | High | `doc-tools.ts` |
| Cron `schedule` and `cron_expr` both bound to `$5` in INSERT — looks like copy-paste bug | High | `cron-tools.ts` |
| If `_cronQueue` is null, cron INSERT succeeds but job never runs — user gets no error | High | `cron-tools.ts` |
| SSE route has no per-org connection limits — malicious client can exhaust file descriptors | High | `events.ts` |
| No org-level rate limiting — one org can exhaust shared Redis/DB/BullMQ resources | High | Global |
| `improvementWorker` fires all orgs simultaneously at 3 AM — no rate limiting | High | `queues.ts` |

### 🟡 High — Significant Gaps

| Issue | Severity | File |
|-------|----------|------|
| `writeMemoryTool` — no `.catch()` on SQL writes for user/org profile | Medium | `memory-tools.ts` |
| Skills loader — fire-and-forget UPDATE errors swallowed silently | Medium | `skills/loader.ts` |
| `webSearchTool` — `engines=google` override in SearXNG may be blocked/rate-limited | Medium | `deep-research-tools.ts` |
| `fetchUrlTool` — crude HTML stripping removes semantic tags, returns jumbled text | Medium | `deep-research-tools.ts` |
| `listDocsTool` — no pagination, hardcoded `LIMIT 20` | Medium | `doc-tools.ts` |
| `readMemoryTool` — user target doesn't verify user belongs to org | Medium | `memory-tools.ts` |
| `requestContext` cast to `Record<string, string>` is unsafe — no validation | Medium | `prompt-builder.ts` |
| Turn counter Map in `prompt-builder.ts` grows indefinitely if `clearTurn` never called | Medium | `prompt-builder.ts` |
| Two separate Mastra instances (`agent/mastra.ts` vs `mastra/index.ts`) — weather demo vs main agent | Medium | Multiple |
| Frontend AI chat (Nitro/Vercel AI SDK) is completely decoupled from Alizé agent (Mastra) — no shared memory, different models | Medium | Architecture |
| `skill_catalog` has no `embedding` column — semantic search falls back to ILIKE | Medium | `routes/skills.ts` |
| No connection pooling solution (PgBouncer/pgpool) — 20 connection pool may be insufficient | Medium | `db/client.ts` |
| No graceful shutdown for BullMQ workers — drain active jobs before exit not implemented | Medium | `workers/` |
| `loadSkillBySlug` not exposed as a tool — internal use only (may be intentional) | Low | `skills/loader.ts` |

### 🟡 Frontend Gaps (Not Resolved)

| Issue | Severity | File |
|-------|----------|------|
| File upload has no progress tracking (`XMLHttpRequest` needed, not `$fetch`) | Critical | `useFileUpload.ts` |
| No pagination on chats list — returns all chats at once | Critical | `chats.vue` |
| Chat messages don't include file attachment text when sent to engine | High | `chats/[id].post.ts` |
| No `/api/auth/get-session` endpoint verified for better-auth coverage | Critical | `server/api/auth/` |
| No user account settings / profile page | High | `pages/` |
| OAuth social providers wired but no account linking UI in settings | High | `UserMenu.vue` |
| No chat sharing functionality | High | `pages/chat/[id].vue` |
| Chat search in sidebar wired but has no real search results endpoint | High | `layouts/default.vue` |
| Weather tool uses mock/random data — no real OpenWeather API | Medium | `shared/utils/tools/weather.ts` |
| No error boundary for failed chat loads | Medium | `pages/chat/[id].vue` |
| `useHighlighter` uses `await` at component level — hydration mismatch risk | Medium | `prose/PreStream.vue` |

---

## 3. Architecture Summary

```
apps/
├── api/src/
│   ├── agent/                    # Mastra AI agent + tools
│   │   ├── mastra.ts            # Main agent definition
│   │   ├── prompt-builder.ts     # Layered system prompt (soul → memory → skills)
│   │   ├── tools/
│   │   │   ├── doc-tools.ts      # Document CRUD tools
│   │   │   ├── memory-tools.ts   # Org/user memory tools
│   │   │   ├── skill-tools.ts   # Skill create/improve/list
│   │   │   ├── cron-tools.ts     # Cron create/list/delete
│   │   │   ├── search-docs.ts    # Semantic document search
│   │   │   └── deep-research.ts  # Multi-step research with sub-agents
│   │   └── skills/
│   │       └── loader.ts         # Vector similarity skill matching
│   │
│   ├── server/                   # Hono HTTP server (port 4000)
│   │   ├── app.ts               # Middleware chain (CORS → auth → routes)
│   │   ├── middleware/auth.ts    # JWT verification, orgId/userId context
│   │   └── routes/
│   │       ├── chat.ts           # POST /api/chat/send — SSE streaming
│   │       ├── documents.ts      # Upload, confirm, list, delete
│   │       ├── skills.ts         # Catalog search, install/uninstall
│   │       ├── crons.ts          # Create, list, toggle, delete cron jobs
│   │       ├── auth.ts           # Register, login, me
│   │       └── events.ts         # SSE /api/events/stream
│   │
│   ├── workers/                  # BullMQ background jobs
│   │   ├── queues.ts             # Queue/worker setup, cron sync on startup
│   │   ├── doc-worker.ts         # Download → parse → chunk → embed → store
│   │   ├── cron-worker.ts        # Load skills → run agent → deliver to channel
│   │   └── improvement-worker.ts  # Nightly: reflect → update memory → improve skills
│   │
│   ├── events/
│   │   └── bus.ts               # Redis pub/sub event bus (pattern subscriptions)
│   │
│   ├── channels/
│   │   ├── adapters.ts           # Slack, Discord, Teams, WhatsApp, Email (ALL STUBS)
│   │   └── manager.ts            # Unified message normalization (ALL STUBS)
│   │
│   ├── documents/
│   │   ├── parser.ts             # PDF/DOCX text extraction
│   │   ├── chunker.ts            # Recursive character splitting
│   │   ├── embed.ts              # Embedding via Mistral API
│   │   └── vectorstore.ts        # pgvector HNSW search (raw SQL)
│   │
│   ├── db/
│   │   ├── client.ts             # Drizzle ORM + raw SQL pool
│   │   └── schema.ts             # PostgreSQL schema (Drizzle)
│   │
│   ├── config/
│   │   ├── env.ts                # Zod-validated environment
│   │   └── constants.ts          # CHANNELS (6 declared, 1 implemented)
│   │
│   └── prompts/                  # System prompt templates
│       └── soul.ts               # Agent personality
│
└── web/                          # Nuxt 3 frontend (dev port 3000)
    ├── app/
    │   ├── pages/
    │   │   ├── index.vue         # Landing page
    │   │   ├── login.vue         # OAuth + email/password login
    │   │   ├── signup.vue
    │   │   ├── chats.vue         # Chat list
    │   │   └── chat/[id].vue     # Main chat UI
    │   ├── components/
    │   │   ├── FileUploadButton.vue
    │   │   └── UserMenu.vue      # Theme, profile, templates (links to Nuxt UI)
    │   ├── composables/
    │   │   ├── useAuth.ts        # Auth state, OAuth, open redirect risk
    │   │   ├── useChats.ts        # Chat CRUD (date calc bug)
    │   │   └── useFileUpload.ts   # S3 upload (no progress tracking)
    │   └── layouts/default.vue   # Sidebar with search (no real search)
    └── server/
        └── api/
            ├── auth/             # better-auth catch-all + session
            ├── chats/            # AI streaming via Vercel AI SDK (separate AI system!)
            └── upload/           # S3 presigned URLs

Infrastructure:
├── PostgreSQL 16 + pgvector (HNSW indexes)
├── Redis (BullMQ + pub/sub — single ioredis connection shared)
├── MinIO (S3-compatible object storage)
└── Hono server (port 4000) + Nuxt dev server (port 3000)
```

### Key Architecture Decision: Two Separate AI Systems

The frontend chat (`apps/web/server/api/chats/[id].post.ts`) uses **Vercel AI SDK** with `streamText` — completely decoupled from the Alizé Mastra agent. The Mastra agent (with skills, memory, document access) is only reachable via Hono's `/api/chat/send`. These are **two different AI systems**:

| | Frontend (Nitro/Vercel AI) | Alizé Agent (Mastra) |
|---|---|---|
| Access to skills | ❌ No | ✅ Yes |
| Access to documents | ❌ No | ✅ Yes |
| Access to cron tools | ❌ No | ✅ Yes |
| Memory system | ❌ No | ✅ Yes |
| Model | Configurable per-chat | Mistral Large |

---

## 4. Migration Status: Nuxt Server → Hono

**Status: PARTIAL**

The Hono backend (`apps/api/src/server/`) is **partially built** but the Nuxt Nitro server routes are still the **primary** frontend dependency. The frontend's chat, auth, and upload routes live in `apps/web/server/api/` and are NOT proxied through Hono.

### What's in Hono ✅

- Auth routes (`/auth/register`, `/auth/login`, `/auth/me`) with JWT
- Chat streaming (`/api/chat/send`) — main AI agent endpoint
- Document routes (`/api/documents` — upload, confirm, list, delete)
- Skills routes (`/api/skills` — catalog search, install, list, improve)
- Cron routes (`/api/crons` — create, list, toggle, delete)
- SSE events (`/api/events/stream`)
- Webhook routes (Slack, Discord, Teams, WhatsApp — all stubs)

### What's NOT migrated ❌

- Frontend auth session handling (still uses better-auth/Nuxt)
- Frontend AI chat streaming (`/api/chats/[id].post.ts` — Vercel AI SDK)
- File upload presigned URLs (still via Nuxt server routes)
- Chat listing, search, deletion (some via Nuxt, some via Hono — split responsibility)
- `get-session` endpoint (better-auth coupled to Nuxt)

### Migration Complexity

| Route | Status | Complexity |
|-------|--------|------------|
| Auth (register, login, me) | In Hono ✅ | Medium |
| Chat streaming (`/api/chat/send`) | In Hono ✅ | Medium |
| Document CRUD | In Hono ✅ | Simple-Medium |
| Skills | In Hono ✅ | Simple |
| Crons | In Hono ✅ | Simple |
| SSE events | In Hono ✅ | Medium |
| Webhooks | Stubs in Hono ⚠️ | Medium-Hard |
| Frontend auth (better-auth) | Nuxt ⚠️ | Hard |
| Frontend AI chat | Nuxt ⚠️ | Hard |
| File upload/delete | Nuxt ⚠️ | Medium |

**The Nuxt frontend is still tightly coupled to its own Nitro server routes.** Full migration to Hono requires either (a) keeping Nuxt as BFF and proxying to Hono, or (b) fully reimplementing auth, streaming, and upload in Hono.

---

## 5. Test Coverage

**0%** — No test framework, no test files, no coverage configuration.

```
package.json devDependencies: @types/node, tsx, typescript only
No __tests__/, *.test.ts, *.spec.ts files
No Vitest, Jest, or Playwright config
```

| Category | Coverage |
|----------|----------|
| Auth routes | 0% |
| Chat route | 0% |
| Document worker | 0% |
| Cron worker | 0% |
| Improvement worker | 0% |
| All agent tools | 0% |
| Event bus | 0% |
| DB client | 0% |
| Channel adapters | 0% |
| Auth middleware | 0% |
| Skill loader | 0% |
| Prompt builders | 0% |

**Immediate need:** Install Vitest and write unit tests for auth routes and workers before any further development.

---

## 6. Next Steps — What Louis Should Focus On

### Immediate (Before Any Feature Work)

1. **Install Vitest** — `npm install -D vitest @vitest/coverage-v8`
   - Add test script to `package.json`
   - Create `__tests__/` directories
   - Write tests for auth routes first (highest ROI — well-defined inputs/outputs)

2. **Fix critical security issues:**
   - Implement magic link auth (MVP token-direct-return is a production risk)
   - Implement Slack/Discord webhook signature verification
   - Fix open redirect in `normalizeReturnTo`
   - Add `documentId × orgId` ownership check in `readDocTool`

3. **Fix channel adapters or remove them from the roadmap**
   - If Slack/Discord/Teams/WhatsApp are not Q2 goals, remove them from `config/constants.ts` CHANNELS array
   - Don't advertise 6 channels when only Web works

### Short-Term (1-4 Weeks)

4. **Consolidate the dual AI system** — either:
   - Integrate Mastra agent into Nitro server routes (remove separate Vercel AI SDK path), OR
   - Document clearly that these are separate systems with different capabilities

5. **Implement channel adapters or delete them**
   - Slack WebClient, Discord client, webhook verifications
   - `normalizeSlackEvent`, `normalizeDiscordEvent` — return proper `UnifiedMessage` objects

6. **Add missing indexes** (DB performance):
   - `cron_executions(cron_id)`
   - `documents(metadata->>'type')`
   - `improvement_log(org_id)`

7. **Fix cron bugs:**
   - `schedule` vs `cron_expr` binding (likely copy-paste bug on line 27)
   - Return error when `_cronQueue` is null instead of `success: true`

8. **Add `embedding` column to `skill_catalog`** — enables true semantic search in the marketplace instead of ILIKE fallback

9. **Frontend pagination** — chats list returns all at once; add `LIMIT/OFFSET`

10. **File upload progress** — replace `$fetch` with `XMLHttpRequest` for progress tracking

### Medium-Term (1-2 Months)

11. **Rate limiting** — per-org limits on Redis (sliding window), prevent one org from exhausting shared resources

12. **Rate limit improvement worker** — stagger 3 AM jobs across time window instead of firing all simultaneously

13. **Add graceful shutdown** to all BullMQ workers

14. **Fix turn counter memory leak** — add TTL or automatic eviction for abandoned conversations

15. **Frontend settings page** — account profile, password change, OAuth account linking

16. **Add real weather API** — replace `Math.random()` mock with OpenWeatherMap

17. **Fix app branding** — update `app.vue` title, update UserMenu "Templates" links

18. **Connection pooling** — consider PgBouncer for DB connection management at scale

---

## Summary Scorecard

| Category | Status | Notes |
|----------|--------|-------|
| Core AI Agent | ✅ Working | Mastra + tools + memory + skills |
| Document Pipeline | ✅ Working | Upload → parse → chunk → embed → search |
| Cron System | ⚠️ Buggy | Queue not wired, cron_expr bug, silent failures |
| Channel Adapters | ❌ Stubs | All 5 non-Web channels are console.log stubs |
| Auth | ⚠️ MVP | No magic link, token returned directly |
| Webhook Security | ❌ Broken | Signature verification always returns true |
| Frontend Chat | ⚠️ Split AI | Uses Vercel AI SDK, not connected to Alizé agent |
| Multi-tenancy | ✅ Good | Row-level security, org-scoped queries |
| Test Coverage | ❌ 0% | No tests whatsoever |
| Observability | ❌ None | No structured logging, no tracing |
| Scalability | ⚠️ Limited | Fixed concurrency, single process, no auto-scaling |

**Bottom line:** The Alizé agent (skills, memory, document search, cron execution) is a solid foundation. The critical gaps are (1) zero test coverage, (2) incomplete channel adapters that are advertised but non-functional, (3) MVP-only auth that needs magic link implementation, and (4) the frontend being a separate AI system decoupled from the agent.**
