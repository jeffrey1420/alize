# Alizé System Architecture Review

**Date:** 2026-03-26
**Reviewer:** Senior Software Architect
**Version reviewed:** v0.1.0

---

## 1. Architecture Overview

### High-Level Structure

```
apps/
├── api/src/
│   ├── agent/       # Mastra agent + tools
│   ├── server/     # Hono HTTP server + routes
│   ├── workers/     # BullMQ background jobs
│   ├── events/     # Redis pub/sub event bus
│   ├── documents/  # PDF/DOCX processing, embedding, pgvector
│   ├── db/         # Drizzle ORM + raw SQL for pgvector
│   ├── channels/   # Multi-channel adapter registry
│   └── config/     # Env validation, constants
└── web/
    ├── app/         # Nuxt 3 frontend (pages, composables)
    └── server/      # Nitro server API routes + DB (separate schema)
```

**Infrastructure:**
- PostgreSQL 16 with pgvector (HNSW indexes)
- Redis (shared for BullMQ connection + pub/sub)
- MinIO (S3-compatible object storage)
- Hono web server on port 4000
- Nuxt 3 frontend (dev port 3000)

### Communication Patterns

| Path | Mechanism | Purpose |
|------|-----------|---------|
| Client → API | REST (Hono) | CRUD operations |
| Client → API | SSE (`/api/events/stream`) | Real-time updates via Redis pub/sub |
| Client → Frontend | REST (Nitro) | Chat UI, uploads |
| Frontend → API | REST (Hono) | Document ops, skills, crons |
| Agent → Workers | BullMQ queues | Async document processing |
| Workers → Bus | Redis pub/sub | Emit document/cron events |
| Cron → Agent | BullMQ scheduled jobs | Nightly self-improvement |

---

## 2. Multi-Tenant SaaS Assessment

### ✅ Correct Patterns Found

- **Row-level security:** Every query filters by `org_id`. Foreign keys enforce tenant isolation at the DB level.
- **Org-scoped resources:** Skills, documents, cron jobs, memories are all org-owned.
- **JWT contains org context:** Token payload includes `orgId` and `role`, extracted in auth middleware.
- **S3 key prefixes:** `${orgId}/generated/...` — prevents cross-tenant file access.

### ⚠️ Concerns

1. **Auth is per-org, not per-resource.** The `authMiddleware` sets `orgId` from the JWT but there's no check that the `orgId` in the JWT matches the resource being accessed (e.g., when fetching a document by ID, you only verify the document belongs to the JWT's org — this is correct, but relies on every route properly filtering).

2. **No tenant-level rate limiting.** A single org could exhaust shared resources (Redis connections, BullMQ concurrency, DB pool).

3. **Turn counters are in-memory.** `prompt-builder.ts` uses a `Map<string, number>` for turn counting — these reset on restart and don't scale horizontally.

---

## 3. Critical Bottlenecks & Scaling Issues

### 🔴 High Severity

**1. SSE Stream Never Terminates (`/api/events/stream`)**

```typescript
// events.ts line ~30
await new Promise<void>(() => {
  // never resolves — stream stays open until client disconnects
})
```

This pattern is correct for SSE but there's no connection limit per org. A malicious client could open many SSE connections and hold them open indefinitely, exhausting file descriptors and memory.

**2. Redis Connection is Shared**

`queues.ts` creates a single `ioredis` connection used by both BullMQ and the EventBus pub/sub connections. However, the EventBus creates *two* additional Redis connections (pub + sub) via `getPub()` and `getSub()`. BullMQ manages its own connection pool internally, but the single shared `_connection` reference could become a bottleneck under load.

**3. Unbounded Agent Creation in Improvement Worker**

The improvement worker creates 3-4 new Mastra `Agent` instances per run (reflector, memory-updater, profile-updater, skill-improver, skill-creator). Each Mastra agent may initialize its own model connections. With many orgs, this could exhaust file descriptors or API rate limits.

**4. Fixed Concurrency, No Auto-scaling**

```typescript
const docWorker = new Worker('document-processing', ..., { concurrency: 2 })
const cronWorker = new Worker('cron-jobs', ..., { concurrency: 3 })
```

These are hardcoded. At scale, you'd need to run multiple worker processes or use auto-scaling. The current design assumes a single Node.js process.

### 🟡 Medium Severity

**5. pg Pool Size Unbounded for Concurrent Queries**

The DB pool is created with `max: 20`, but `storeChunks()` acquires a client and holds it for the entire batch insert loop. If 50 concurrent document processing jobs try to store chunks, they'll all compete for the 20-connection pool.

**6. Document Processing is Entirely Async**

When a document is uploaded, the API immediately returns a presigned URL and queues a BullMQ job. There's no way to retry a failed upload — if the client fails to upload to S3, the document record remains in `pending` state forever.

**7. Frontend Has Its Own DB Schema**

The web app (`apps/web/server/db/`) has its own Drizzle schema (`chats`, `messages`) which is separate from the API's schema. This means the API's Mastra agent has no access to the chat history that the frontend stores. The frontend uses Vercel AI SDK directly with its own model selection — completely decoupled from the API's Mastra agent.

**This is a significant architectural split:** The "AI chat" in the web frontend talks to the Nuxt/Nitro server, which uses `streamText` from `ai` SDK. The Alizé agent (which has memory, skills, document access) is only used when clients call `/api/chat/send` on the Hono server. These are two separate AI systems.

**8. Skill Matching is N+1**

`skill-tools.ts` and `skills/loader.ts` update `times_used` with a fire-and-forget query per skill:

```typescript
for (const skill of rows) {
  sql('UPDATE skills SET times_used = times_used + 1, ...').catch(() => {})
}
```

With 3 skills per request and high traffic, this generates many single-row UPDATE queries.

### 🟢 Lower Severity

**9. No Request Validation on Some Routes**

The document upload route validates mime type and file size, but many routes accept arbitrary JSON and pass it directly to SQL parameters. This is acceptable with parameterized queries but leaves the door open for misuse.

**10. `improvementQueue.upsertJobScheduler()` Called at Startup**

At startup, the system iterates over ALL orgs and creates a BullMQ recurring job per org. For 1000+ orgs, this could slow startup significantly and create 1000+ scheduler entries in Redis.

---

## 4. Failure Modes at Scale

### Where the System Will Break

| Component | Failure Mode | Trigger |
|-----------|-------------|---------|
| Redis | OOM with pattern subscriptions | Many clients with `org:${orgId}:*` SSE subscriptions |
| SSE Route | File descriptor exhaustion | Many long-lived connections per org |
| BullMQ doc-worker | Job backlog | Large PDFs (>10MB) with high concurrency |
| Improvement Worker | Unbounded LLM API costs | Org with thousands of conversations |
| Mastra Agent | Context overflow | Long conversations hitting token limits |
| PostgreSQL | Connection pool exhaustion | 20 connections insufficient for concurrent workers |
| MinIO | No multipart upload handling | Large files time out |

---

## 5. Separation of Concerns — Assessment

### ✅ Good Separations

- **Workers are isolated:** Document processing, cron execution, and improvement are in separate worker files.
- **Events are decoupled:** Redis pub/sub allows workers to emit events without knowing who's listening.
- **Channel adapters:** `channels/adapters.ts` provides an abstraction over Slack, Discord, Teams, WhatsApp, Email, Web.
- **Tool abstraction:** Agent tools are in `agent/tools/` and imported into Mastra.
- **DB schema is well-normalized:** Organizations → Users → (Documents, Skills, CronJobs) with proper foreign keys.

### ⚠️ Problematic Separations

**1. The API is NOT the system's brain — it's a side channel.**

The primary AI chat (what users interact with in the web UI) goes through `apps/web/server/api/chats/[id].post.ts` which uses `streamText` from the Vercel AI SDK. The Alizé agent (with skills, memory, document access) is only reachable via `/api/chat/send` on the Hono server. This means:

- The frontend's AI chat doesn't have access to skills, document search, or cron tools.
- Two different AI models are in play: the Nitro/Vercel AI SDK model (web chat) vs. the Mastra/Mistral agent (API chat).
- There's no shared memory between them.

**2. The "channels" abstraction is incomplete.**

`channels/manager.ts` has adapters for Slack, Discord, etc., but they're all stubs with `console.log`. The `verifyWebhook()` methods are TODO comments. The multi-channel vision is architectural scaffolding without implementation.

**3. Document chunks are managed with raw SQL, not Drizzle.**

This is a known limitation (pgvector doesn't map to Drizzle), but it creates two query patterns in the codebase: ORM queries via `db()` and raw SQL via `sql()`. This split makes it easy to miss one pattern when adding features.

**4. Improvement worker and main agent share the DB but not context.**

The improvement worker runs at 3 AM and updates `org_memory` and `user_profiles` tables. But these are only read into the Mastra agent's prompt via `prompt-builder.ts`. There's no invalidation or hot-reload of this context — the agent's in-memory `Memory` instance (`new Memory({...})`) won't see changes made by the improvement worker until the agent is restarted.

---

## 6. Improvement Worker (Hermes-Style) Integration

### What It Does

The improvement worker runs nightly per organization and:
1. Collects 24h of conversation history from PostgreSQL
2. Uses a "reflector" Mastra agent to extract durable facts, skill gaps, and improvements
3. Updates `org_memory` and `user_profiles` tables
4. Improves existing skills or creates new ones from repeated gaps
5. Logs results to `improvement_log`

### ✅ Correct Integration Patterns

- Uses its own BullMQ queue (`improvement`) with concurrency 1 — won't starve other workers
- Stores results back to PostgreSQL (durable, queryable)
- Uses embeddings for skill matching and creation
- Scheduled via `upsertJobScheduler` (persisted in Redis)

### ⚠️ Concerns

**1. No timeout on reflection/generation calls.**

```typescript
const result = await reflector.generate(`Conversation...`)
```

If the Mistral API is slow or the model is overloaded, this could hang indefinitely. Should use a timeout.

**2. Creates unbounded sub-agents within a single job.**

For an org with 50 conversations, the improvement worker creates:
- 1 reflector agent per conversation (50 max, sequential)
- 1 memory-updater agent
- 1 profile-updater per affected user
- N skill-improver agents
- N skill-creator agents

Each agent initialization involves model provider SDK setup. This is memory-intensive.

**3. Does not invalidate Mastra agent memory cache.**

When `org_memory` is updated by the worker, running Mastra agents won't see it until they reload context. The `Memory` instance in `mastra.ts` is a singleton that re-loads on each agent turn, so this is partially mitigated — but there's no forcing function.

**4. Silent failure on individual steps.**

```typescript
} catch {
  // Skip malformed reflections
}
```

Parsing errors are swallowed. An org could have broken JSON output every night without anyone knowing.

---

## 7. Security Concerns

### 🔴 High Severity

**1. Magic Link Login Returns Token Directly (MVP)**

```typescript
// auth.ts — login route
const token = await generateToken(user.id, user.org_id, user.role)
return c.json({ token, userId: user.id, orgId: user.org_id })
```

No email verification, no magic link, no password. Anyone who knows an email address in the system can get a valid JWT. This is explicitly TODO for production, but it's a critical gap.

**2. Admin-Only Check is Name-Based**

```typescript
export const adminOnly = createMiddleware(async (c, next) => {
  if (c.get('userRole') !== 'admin') {
    return c.json({ error: 'Admin access required' }, 403)
  }
  await next()
})
```

The `adminOnly` middleware is only applied to specific routes, not globally. And the role comes from the JWT — which is self-asserted. If the JWT signing key is compromised, an attacker can claim any role.

**3. No Webhook Signature Verification**

All channel adapters have stub `verifyWebhook()` methods:

```typescript
verifyWebhook(headers: Record<string, string>, body: string): boolean {
  // TODO: Verify Slack signing secret
  void headers; void body;
  return true  // Always returns true!
}
```

Slack and Discord webhooks are a primary attack vector for account takeover. These MUST be implemented before any channel goes live.

### 🟡 Medium Severity

**4. JWT Secret is env Variable**

If `JWT_SECRET` is weak or leaked, all tokens can be forged. There's no token rotation mechanism, no refresh token flow, and no revocation list.

**5. S3 Credentials in Environment**

The S3 endpoint, access key, and secret key are all env variables. If the API process is compromised, these are readable. Consider using IAM roles if running on AWS/OVHcloud with instance profiles.

**6. No Input Sanitization on User-Provided Prompts**

The cron job `prompt` field is stored and passed directly to the AI model. If a malicious user creates a cron job with a prompt injection attack, it could affect cron execution outputs. The AI model itself provides some protection, but there's no sanitization layer.

**7. Document Processing Downloads from S3**

```typescript
const buffer = await downloadFile(doc.s3_key as string)
```

If an `s3_key` is modified in the database to point to a different org's files, the document processing would happily download and index it. The `orgId` in the job data should be verified against the document's `org_id` before downloading.

---

## 8. Inter-Process Communication Assessment

### Redis Pub/Sub ✅

The event bus pattern is sound. Pattern-based subscriptions (`org:${orgId}:*`) allow targeted event delivery. The implementation properly manages subscription lifecycle with reference counting.

**However:** Redis pub/sub is at-most-once. If a client disconnects during an event publish, that event is lost. For critical events (like document ready notifications), consider using Redis Streams instead.

### BullMQ ✅

The queue design is correct:
- Exponential backoff for retries
- Job removal policies prevent queue bloat
- Separate queues for different job types (doc-processing, cron, improvement)
- Scheduler persistence via `upsertJobScheduler`

**However:** BullMQ is Redis-backed. If Redis goes down, all queue operations fail. For a production SaaS, you'd want Redis Sentinel or Cluster for HA.

### SSE ✅ (with caveats)

SSE is the right choice for real-time updates (document processing progress, cron execution results). The keep-alive ping every 30 seconds is good practice.

**Issue:** The SSE route blocks on `stream.onAbort()` but the underlying `eventBus.subscribe()` callback loop continues until the next event check. There could be a brief window where events are written to a closed stream.

### REST API ✅

Hono is a solid choice — lightweight, fast, and TypeScript-native. The middleware chain (CORS → auth → routes) is clean.

---

## 9. Recommended Architectural Improvements

### Immediate (Before Production)

1. **Implement magic link email flow** for auth. The current direct-token-return is a security risk.
2. **Add webhook signature verification** for Slack and Discord adapters before any channel goes live.
3. **Add connection limits** to SSE routes (per-org max connections).
4. **Move JWT secret to a secrets manager** (Vault, AWS Secrets Manager).
5. **Add org-level rate limiting** using a Redis sliding window.
6. **Verify s3_key org ownership** before document processing downloads.

### Short-Term (Scale Preparation)

7. **Split into multiple worker processes.** Run doc-workers, cron-workers, and improvement-workers as separate Node.js processes with independent concurrency settings.
8. **Add Redis Streams** instead of pub/sub for event delivery (at-least-once semantics).
9. **Implement connection pooling improvements:** Use PgBouncer or pgpool-II in front of PostgreSQL.
10. **Add read replicas** for vector search queries (`searchChunks` is read-heavy).
11. **Move to a job scheduling system** (Oban for Elixir, or Temporal) for the improvement worker instead of BullMQ for complex workflows with many steps.
12. **Fix the frontend/API split:** Either integrate the Mastra agent into the Nitro server routes, or explicitly document that these are two separate AI systems with different capabilities.

### Medium-Term (Multi-Tenant SaaS)

13. **Add tenant quotas:** Max documents per org, max cron jobs per org, max API calls per day per plan.
14. **Implement row-level security** in PostgreSQL as a second layer of tenant isolation (via `SET LOCAL app.current_org_id` and security barrier views).
15. **Add observability:** Structured logging (pino), OpenTelemetry tracing across workers, metrics dashboard.
16. **Implement proper graceful shutdown** for all workers (drain active jobs before exit).
17. **Add a CDN** (Cloudflare or similar) in front of the Nuxt frontend for static asset caching.
18. **Consider separating the embedding pipeline** from the document processing worker — embeddings are the most expensive part and benefit from GPU acceleration.

---

## 10. Summary Scores

| Category | Score | Notes |
|----------|-------|-------|
| Multi-tenancy isolation | 7/10 | Good org scoping, but no quotas or RLS |
| Scalability | 5/10 | Fixed concurrency, single-process, in-memory state |
| Separation of concerns | 6/10 | Clean structure, but frontend/API split is problematic |
| Security | 4/10 | MVP auth, no webhook verification, self-asserted roles |
| Observability | 3/10 | Basic console logging, no distributed tracing |
| Operational resilience | 5/10 | BullMQ retry logic exists, but no graceful shutdown, Redis is SPOF |
| Code quality | 7/10 | TypeScript throughout, Zod validation, good schema design |

**Overall: 5.3/10** — Solid MVP architecture with clear separation of concerns, but significant gaps for a production multi-tenant SaaS. The two most pressing issues are the incomplete auth story and the frontend/API split where the web chat doesn't have access to the Alizé agent's capabilities.

---

## Appendix: Key File Reference

| File | Purpose |
|------|---------|
| `apps/api/src/index.ts` | Entry point, wires queues → tools, starts server |
| `apps/api/src/server/app.ts` | Hono app, middleware, route registration |
| `apps/api/src/server/routes/chat.ts` | `/api/chat/send` — SSE streaming to Mastra agent |
| `apps/api/src/agent/mastra.ts` | Mastra agent definition with tools |
| `apps/api/src/agent/prompt-builder.ts` | Layered system prompt (soul, memory, skills) |
| `apps/api/src/workers/queues.ts` | BullMQ queue/worker setup, cron sync |
| `apps/api/src/workers/improvement-worker.ts` | Hermes-style self-improvement loop |
| `apps/api/src/events/bus.ts` | Redis pub/sub event bus |
| `apps/api/src/documents/vectorstore.ts` | pgvector search (raw SQL) |
| `apps/api/src/db/client.ts` | Drizzle + raw SQL, pool management |
| `apps/web/server/api/chats/[id].post.ts` | Frontend AI chat (Vercel AI SDK) — NOT routed to Alizé agent |
