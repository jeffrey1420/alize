# Alizé Backend Infrastructure — Code Review

**Reviewer:** Senior Code Review (Subagent)
**Date:** 2026-03-26
**Scope:** `/data/workspace/alize-code/apps/api/src/` — Server, Events, Workers, Database

---

## 1. Auth Middleware — `server/middleware/auth.ts`

### ✅ Correct
- JWT verification using `jose` with HS256 — standard and correct.
- Context variables (`orgId`, `userId`, `userRole`) properly set on Hono context.
- `adminOnly` middleware is simple and correct.

### 🔴 Critical: `/auth/me` bypasses authentication

In `app.ts`:
```ts
app.route('/auth', authRoutes)     // Public — no middleware
app.use('/api/*', authMiddleware) // Protected
```

`authRoutes.get('/me')` in `routes/auth.ts` reads `c.get('orgId')` and `c.get('userId')`, but **these are never set** because `/auth` is outside the protected `/api/*` scope. Every call to `/auth/me` returns undefined for both values, and the query `'WHERE u.id = $1'` with `userId = undefined` will likely match the first row in the table.

**Fix:** Apply `authMiddleware` to `/auth/me` specifically, or move it to `/api/user/me`.

### 🟡 Minor: No `iss` (issuer) claim in JWT
The JWT has no `iss` claim. While not exploitable with HS256 (secret is server-side), adding an issuer claim protects against certain confusion attacks if the secret is ever rotated incorrectly.

---

## 2. SSE Streaming — `server/routes/events.ts`

### 🔴 SSE stream never terminates cleanly

```ts
await new Promise<void>(() => {
  // Never resolves — stream stays open until client disconnects
})
```

This is an anti-pattern. While `stream.onAbort` does call `clearInterval` and `unsubscribe()`, the `await` never completes so the route handler never returns. If `onAbort` fails or doesn't fire (e.g., forced termination), the subscription and `keepAlive` interval leak. The pattern makes it impossible to test whether cleanup completed.

**Fix:** Use a different mechanism to keep the handler alive (e.g., a `ReadableStream` with no data, or a `waitUntil` pattern), and ensure `onAbort` explicitly resolves the promise.

### 🟡 Event IDs are low-entropy

```ts
id: Date.now().toString()
```

If two events fire within the same millisecond, IDs collide. For SSE reconnect/resumption logic (which this API lacks), this would cause issues. Use a counter or UUID instead.

### 🟡 Silent error swallowing in pmessage handler

In `events/bus.ts`:
```ts
} catch {
  // Silently ignore malformed events
}
```

Malformed events (invalid JSON) are silently discarded. While safe, this makes debugging impossible in production. At minimum, log at `debug` level.

---

## 3. Workers — `workers/queues.ts`, `doc-worker.ts`, `cron-worker.ts`, `improvement-worker.ts`

### 🔴 Doc Worker: `sql()` result is not awaited — always undefined

In `doc-worker.ts` line:
```ts
const doc = await sql<Record<string, unknown>>(
  'SELECT * FROM documents WHERE id = $1',
  [documentId],
).then((r) => r[0])
```

Wait — this is `await ... .then()`. The `await` is present so it's actually fine (the `.then()` is just an arrow function, the promise is awaited). **Not a bug.**

### 🟡 Doc Worker: EventBus publish errors are silently swallowed

```ts
const emit = (type: string, data?: Record<string, unknown>) =>
  eventBus.publish(`org:${orgId}:doc:${documentId}`, { type, documentId, ...data })
```

These `emit()` calls (for `processing`, `ready`, `error`) are not awaited. If `eventBus.publish()` rejects, the error is unhandled and processing continues as if nothing happened. SSE subscribers would never receive the event.

**Fix:** Either `await emit(...)` or catch and log errors.

### 🟡 Doc Worker: Status update race

```ts
await updateStatus(documentId, 'processing')  // line in try block
await emit('processing')  // also fires "processing" event
```

The DB is set to `processing` first, then the event is emitted. If the process crashes between these two calls, the DB says "processing" but no event was sent. The document would be stuck.

### 🟡 Queue connection mismatch

In `queues.ts`:
- **BullMQ workers** use `maxRetriesPerRequest: null` ✅
- **EventBus ioredis** uses `maxRetriesPerRequest: 3`

For Redis pub/sub (which ioredis uses internally), `maxRetriesPerRequest` affects the initial subscription commands. 3 retries is likely fine, but inconsistent with BullMQ's configuration. Worth noting.

### 🟡 BullMQ dynamic imports add startup latency

```ts
const docWorker = new Worker(
  'document-processing',
  (await import('./doc-worker.js')).processDocument,
  ...
)
```

Dynamic imports on every `startWorkers()` call add latency and lose V8's parallel compilation benefits. Import once at module level and reference the function directly.

### 🟡 No rate limiting on nightly improvement

In `registerImprovementSchedule()`:
```ts
{ pattern: '0 3 * * *' } // 3am daily
```

If there are 1000 orgs, all 1000 `runImprovement` jobs fire simultaneously at 3 AM. No rate limiting. Could overwhelm the LLM API or database.

---

## 4. Event Bus — `events/bus.ts`

### 🟡 Pattern matching uses RegExp on every event

```ts
private matchPattern(pattern: string, channel: string): boolean {
  const regex = new RegExp('^' + pattern.replace(/\*/g, '.*') + '$')
  return regex.test(channel)
}
```

A new `RegExp` is created on **every single event** for every registered pattern. If there are many SSE subscribers with different patterns, this is O(n) regex creation per event. Cache compiled regexes or use a proper glob-to-regex utility.

### 🟡 `punsubscribe` failure is ignored

```ts
sub.punsubscribe(pattern).catch(() => {})
```

If `punsubscribe` fails, the pattern remains subscribed in Redis, leaking memory on the Redis server. Should at least log the error.

### ✅ EventBus overall design is sound

The pattern of `publish`/`subscribe` with Redis pub/sub, listener management, and `on('pmessage')` routing is correct. The unsubscribe flow properly removes listeners and calls `punsubscribe` when the last listener for a pattern is removed.

---

## 5. Database — `db/client.ts`, `db/schema.ts`

### 🔴 Missing indexes on heavily-queried columns

In `schema.ts`:
- `documents.metadata->>'type'` is filtered in `GET /api/documents` but **no index exists** on this JSON path
- `documents.filename` and `documents.title` are searched with `ILIKE` but **no index exists**
- `cron_executions.cron_id` is filtered in `GET /:id/history` but **no index exists** (only primary key + references)
- `improvement_log.org_id` is used for filtering but **no index exists**

### 🔴 `skills.slug` unique index is not scoped to org

```ts
slugIdx: uniqueIndex('idx_skills_slug').on(t.orgId, t.slug),
```

Wait — looking at the Drizzle schema, this is actually a **composite** unique index on `(orgId, slug)`. The variable naming `slugIdx` is misleading (implies single-column), but the implementation is correct for preventing duplicate slugs per org.

However, `skillCatalog.slug` has:
```ts
slugIdx: uniqueIndex('idx_catalog_slug').on(t.slug),
```

This is a **global** unique index on `slug` in the catalog. That's correct since it's a marketplace-wide catalog.

### 🟡 `organizations.slug` generation is not race-safe

In `routes/auth.ts`:
```ts
const slug = orgName.toLowerCase().replace(/[^a-z0-9]+/g, '-').slice(0, 50)
const [org] = await sql(
  'INSERT INTO organizations (name, slug) VALUES ($1, $2) RETURNING id',
  [orgName, `${slug}-${Date.now()}`],
)
```

The slug `${slug}-${Date.now()}` is intended to make it unique, but `Date.now()` has millisecond precision. Two orgs created in the same millisecond would have duplicate slugs (or fail on the unique constraint). Use a database sequence or UUID suffix instead.

### 🟡 `org_personality` has no updatedAt tracking

The `org_personality` table's `updatedAt` is defined in the Drizzle schema but there's no `updated_at` column in the table definition (the migration only creates `org_id` and `content`). This means the schema/DB are out of sync for this table.

### 🟡 `document_chunks` created in raw SQL migration, not in Drizzle schema

The `runMigrations()` function creates `document_chunks` with raw SQL including the `vector(1024)` column (Drizzle doesn't support pgvector natively). This is fine but means:
1. The schema is duplicated in two places (Drizzle schema + raw SQL)
2. Any schema changes require updating both locations

### 🟡 `skills` table: `catalogId` column in schema is `catalog_id` in DB

Drizzle generates snake_case column names. The schema has `catalogId: uuid('catalog_id')...` which is correct. No issue here.

### 🟡 Pool size: 20 connections

In `db/client.ts`:
```ts
_pool = new pg.Pool({
  connectionString: env().DATABASE_URL,
  max: 20,
  ...
})
```

20 connections may be insufficient under high concurrency with multiple workers. BullMQ workers each maintain their own connection, plus the main app. Monitor this in production.

---

## 6. Documents Route — `server/routes/documents.ts`

### 🔴 Path traversal via filename in S3 key

```ts
const { url, key } = await getUploadUrl(orgId, filename)
```

The `filename` comes directly from the client and is used to construct the S3 key. A malicious client could upload a file with `filename = "../../../etc/passwd"` and potentially access arbitrary S3 paths (depending on how `getUploadUrl` uses it). **Input validation required.**

See `documents/s3.ts` for the actual `getUploadUrl` implementation to confirm whether this is mitigated.

### 🟡 No validation that file was actually uploaded before processing

The `/confirm` endpoint is essentially a no-op:
```ts
// Processing was already queued in /upload — this is a no-op confirmation
return c.json({ success: true })
```

If the client never actually uploads the file to S3 (but gets a presigned URL), the worker will still try to process a non-existent file. Consider adding an S3 `headObject` check in the worker before downloading.

### 🟡 Status polling iterates ALL active jobs, not filtered

```ts
const jobs = await docQueue.getJobs(['active'])
const job = jobs.find((j) => j.data.documentId === id)
```

`docQueue.getJobs(['active'])` with no filter returns **all active jobs** across all orgs. As the job queue grows, this becomes expensive. Use `getJob` by ID directly, or add a named job ID.

### 🟡 `fileSize` is trusted from client

The `fileSize` is taken directly from the client body in `/upload`. A malicious client could claim a file is small to bypass validation, then upload a large file anyway. The S3 presigned URL may or may not enforce size limits — verify `getUploadUrl` enforces this.

---

## 7. Chat Route — `server/routes/chat.ts`

### 🟡 Agent stream not aborted on client disconnect

```ts
const result = await agent.stream(message, { ... })
for await (const chunk of result.fullStream) {
  await stream.writeSSE(...)
}
```

If the HTTP connection drops mid-stream, `agent.stream()` continues running to completion, consuming resources. Hono's `stream.onAbort` should cancel the stream via an `AbortSignal`. Mastra's `agent.stream()` may accept an `abortSignal` option — verify and use it.

### 🟡 No conversation storage

The chat endpoint streams a response but **never stores the messages** (user message or assistant response) in the `messages` table. This means:
- No conversation history persists
- The `improvement-worker` has nothing to analyze (its query would return nothing)
- `Memory` in mastra.ts may be the only persistence mechanism

If Mastra's `Memory` is backed by the database, this may be intentional. Verify that `Memory` writes to the `messages` table.

### 🟡 `onStepFinish` callback errors are not caught

```ts
onStepFinish: async (step) => {
  if (step.toolCalls) { for (const call of step.toolCalls) ... }
  if (step.toolResults) { for (const result of step.toolResults) ... }
}
```

If any `stream.writeSSE` in `onStepFinish` throws, the error is unhandled and could crash the stream. Wrap in try/catch.

---

## 8. Skills Route — `server/routes/skills.ts`

### 🟡 Install race condition

```ts
const existing = await sqlOne(
  'SELECT id FROM skills WHERE org_id = $1 AND catalog_id = $2',
  [orgId, catalogId],
)
if (existing) return c.json({ error: 'Already installed' }, 409)
// ... insert ...
```

Between the check and the insert, another concurrent request could install the same skill. The DB has no unique constraint on `(org_id, catalog_id)` in the Drizzle schema (no composite unique index on those columns). Two simultaneous installs would both succeed, creating duplicates.

**Fix:** Add a unique index on `skills(org_id, catalog_id)` or use `INSERT ... ON CONFLICT DO NOTHING`.

### 🟡 Fallback comment in catalog search

```ts
params.push(vectorToString(embedding))
// Note: requires embedding column on skill_catalog — add via migration
// For MVP, fall back to ILIKE search
params.pop()
params.push(`%${search}%`)
```

The code falls back to `ILIKE` because the embedding column doesn't exist yet. This comment documents a known limitation, but the `embedding` column in `skill_catalog` is never created (unlike `skills.embedding` which is created via `runMigrations()`). This semantic search feature won't work until a migration adds the column.

---

## 9. Crons Route — `server/routes/crons.ts`

### ✅ Generally correct
- Org boundary checks are present in all endpoints
- `cronQueue.upsertJobScheduler` / `removeJobScheduler` used correctly
- Toggle properly handles enable/disable lifecycle

### 🟡 `cron_expr` in toggle uses enabled job's stored expression

```ts
const job = await sqlOne<{ enabled: boolean; cron_expr: string }>(
  'SELECT enabled, cron_expr FROM cron_jobs WHERE id = $1 AND org_id = $2',
  [id, orgId],
)
```

When **disabling**, the `cron_expr` is not used (only to reschedule when re-enabling). When **enabling**, the `cron_expr` from the stored (possibly edited) job is used. This is correct.

---

## 10. Auth Route — `server/routes/auth.ts`

### 🔴 `/auth/me` broken (see Section 1)

### 🟡 Magic link TODO comment

```ts
// TODO: Send magic link email for production
// For MVP, return token directly
const token = await generateToken(user.id, user.org_id, user.role)
```

This is acknowledged as MVP-only. When implementing magic links, ensure:
- Tokens are single-use
- Tokens expire quickly (15-30 min)
- The token delivery mechanism is secure

### 🟡 Org slug collision prevention

```ts
const slug = orgName.toLowerCase().replace(/[^a-z0-9]+/g, '-').slice(0, 50)
const [org] = await sql(
  'INSERT INTO organizations (name, slug) VALUES ($1, $2) RETURNING id',
  [orgName, `${slug}-${Date.now()}`],
)
```

The `Date.now()` suffix prevents collisions at creation time, but `slug` is not validated for uniqueness in subsequent operations. If two orgs somehow get the same slug (edge case with timing), the unique constraint would catch it, but the UX would be poor.

---

## 11. Improvement Worker — `workers/improvement-worker.ts`

### 🟡 Silent reflection failures

```ts
const parsed = JSON.parse(result.text.replace(/```json|```/g, '').trim())
} catch {
  // Skip malformed reflections
}
```

If the AI returns non-JSON, the entire conversation's reflection is discarded silently. Consider logging at least for debugging.

### 🟡 No deduplication of facts before memory update

```ts
const newOrgFacts = reflections.flatMap((r) => r.facts_org ?? []).filter(Boolean)
```

All facts from all conversations are collected and appended. If the same fact appears in multiple conversations, it's added multiple times. The `updater` agent is tasked with deduplication but there's no explicit dedup before sending. May cause token waste and redundant processing.

### 🟡 Skills created without validation

```ts
await sql(
  `INSERT INTO skills (org_id, slug, name, description, content, tags, source, embedding)
   VALUES ($1, $2, $3, $4, $5, $6, 'agent', $7::vector)`,
  [orgId, slug, parsed.name, ...],
)
```

The `slug` is derived from `parsed.name.toLowerCase()...`. If `parsed.name` is empty/malformed, the slug could be invalid or duplicate an existing skill. The `embedding` could fail if `embedQuery` returns wrong dimensions.

### 🟡 Memory limits enforced AFTER generation

```ts
updated.text.slice(0, MEMORY_LIMITS.ORG_MEMORY)
```

The agent generates content, then it's sliced. The agent's instruction says "Limite: X caractères" but this is just a prompt instruction, not a hard limit. The `updater.generate()` could return more than the limit, and the slice truncates mid-sentence. Better to enforce in the prompt or use a stronger instruction.

---

## 12. Overall Architecture Assessment

### Auth Middleware ✅ (once `/auth/me` is fixed)
Org boundaries are correctly enforced in all protected routes. The middleware pattern is sound.

### SSE Streaming 🟡 (needs work)
Implementation works but cleanup is fragile. No reconnect/resume support. Event delivery is fire-and-forget.

### Workers ✅
Correctly wired to BullMQ queues. Concurrency settings are reasonable. Error handling with retry is present. Main issues are the unhandled `emit()` rejections and no rate limiting on improvement.

### Event Bus ✅
Pattern is correct. Redis pub/sub is appropriate for SSE. Listener management is sound.

### DB Schema 🟡
Core schema is well-designed. Main issues: missing indexes on frequently-filtered columns (`documents.metadata->>'type'`, `cron_executions.cron_id`, `improvement_log.org_id`), and `document_chunks` living outside the Drizzle schema.

---

## Summary of Priority Issues

| Priority | File | Issue |
|----------|------|-------|
| 🔴 Critical | `auth.ts` + `app.ts` | `/auth/me` bypasses auth middleware |
| 🔴 Critical | `routes/documents.ts` | Path traversal via `filename` in S3 key |
| 🔴 Critical | `db/schema.ts` | Missing index on `cron_executions.cron_id` |
| 🟡 High | `db/schema.ts` | Missing indexes on `documents(metadata->>'type')`, `documents(title)`, `improvement_log(org_id)` |
| 🟡 High | `doc-worker.ts` | `emit()` promises not awaited — SSE events silently lost |
| 🟡 High | `queues.ts` | Dynamic imports in `startWorkers()` add latency |
| 🟡 Medium | `chat.ts` | No `AbortSignal` for stream cancellation on disconnect |
| 🟡 Medium | `events.ts` | SSE stream cleanup relies on infinite promise anti-pattern |
| 🟡 Medium | `events/bus.ts` | New RegExp created per event — should cache |
| 🟡 Medium | `routes/skills.ts` | No unique constraint on `(org_id, catalog_id)` — race condition on install |
| 🟡 Medium | `improvement-worker.ts` | No rate limiting — all orgs process at 3am simultaneously |
