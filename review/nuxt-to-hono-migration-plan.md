# Nuxt to Hono Migration Plan

## Overview

This document analyzes the Nuxt server API routes (`apps/web/server/api/`) for migration to a Hono backend. Currently, no Hono backend exists—this is a planning document for the migration.

---

## Migration Table

| Nuxt Route File | HTTP Method(s) | Purpose | Already in Hono? | Complexity | Notes |
|-----------------|---------------|---------|-----------------|------------|-------|
| `auth/get-session.get.ts` | GET | Returns current user session | No | Medium | Uses `better-auth`; session logic must be reimplemented |
| `auth/accounts.get.ts` | GET | Lists OAuth accounts linked to user | No | Medium | Uses `better-auth` account plugin |
| `auth/[...all].ts` | * | Catch-all delegating to `better-auth` handler | No | Hard | Core auth infrastructure; complex reimplementation |
| `auth/verify.get.ts` | GET | Verifies email token, redirects | No | Medium | Email verification via `better-auth` |
| `auth/delete-account.delete.ts` | DELETE | Deletes user, chats; signs out | No | Hard | Deletes from DB + auth system + S3 cleanup |
| `chats.post.ts` | POST | Creates new chat with initial message | No | Simple | Simple INSERT, returns new chat |
| `chats.get.ts` | GET | Lists user's chats (paginated) | No | Simple | Simple SELECT with ordering |
| `chats/[id].get.ts` | GET | Gets single chat with messages | No | Simple | SELECT with relation loading |
| `chats/[id].post.ts` | POST | **Main AI streaming endpoint** | No | Hard | Complex streaming + engine proxy + AI logic |
| `chats/[id].delete.ts` | DELETE | Deletes chat + S3 files | No | Simple | DELETE + S3 cleanup |
| `chats/search.get.ts` | GET | Search chats by title | No | Simple | LIKE query with LIMIT |
| `upload/[chatId].put.ts` | PUT | Upload file to S3 | No | Medium | S3 upload + validation |
| `upload/[...pathname].delete.ts` | DELETE | Delete file from S3 | No | Simple | S3 delete with path validation |

---

## Detailed Analysis by Category

### Auth Routes

| Route | Auth Required | DB Ops | Response | Notes |
|-------|--------------|--------|----------|-------|
| `get-session.get.ts` | No | No | `{ session, user }` | Returns null if no session |
| `accounts.get.ts` | Yes (session) | Yes (listAccounts) | `Account[]` | Lists OAuth provider accounts |
| `[...all].ts` | * | * | * | Delegates ALL auth flows to better-auth |
| `verify.get.ts` | No | Yes (verifyEmail) | Redirect | GET request with redirect side-effect |
| `delete-account.delete.ts` | Yes | Yes (DELETE users, chats) | `{ success: true }` | Cascade delete; signs out user |

**Auth Implementation Details:**
- Uses `better-auth` library (v1.5.6) with `drizzle-adapter`
- Supports: email/password, Discord, GitHub, Google, Slack OAuth
- Plugins: `admin`, `organization`, `account`
- Session stored via cookies

**Migration Complexity: HARD** — Auth infrastructure is tightly coupled to `better-auth`. Reimplementing all OAuth flows, email verification, session management is significant work.

---

### Chat Routes

| Route | Auth Required | DB Ops | Response | Notes |
|-------|--------------|--------|----------|-------|
| `chats.post.ts` | Yes | INSERT chat, INSERT message | Chat object | Creates chat + initial user message |
| `chats.get.ts` | Yes | SELECT chats | `Chat[]` | Paginated, ordered by `updatedAt` DESC |
| `chats/[id].get.ts` | Yes | SELECT chat + messages | Chat with messages | Ordered by `createdAt` ASC |
| `chats/[id].post.ts` | Yes | SELECT chat, INSERT messages, UPDATE title | Stream | Complex: AI streaming, title generation, engine proxy |
| `chats/[id].delete.ts` | Yes | SELECT chat, DELETE chat, DELETE S3 files | Deleted chat | S3 cleanup for user files |
| `chats/search.get.ts` | Yes | SELECT chats (LIKE) | `Chat[]` (limited) | `LIKE %q%` query, max 5 results |

**DB Schema (for reference):**
```typescript
// Users, Chats, Messages with relations
// Chats: id, title, userId, createdAt, updatedAt
// Messages: id, chatId, role (user/assistant/system), parts (JSONB), createdAt
```

**`chats/[id].post.ts` Complexity — Key operations:**
1. Validates model (must be in `MODELS` list)
2. Auto-generates chat title if empty (calls AI)
3. Streams from external engine (`ENGINE_URL/chat/send`) with Bearer token
4. Transforms SSE events (`text-delta`, `tool-call`, `tool-result`, `done`) to VAI `UIMessage` format
5. Persists assistant messages to DB on stream completion

**Migration Complexity: HARD** — The streaming/AI proxy logic is complex and integrates with external engine.

---

### Upload Routes

| Route | Auth Required | DB Ops | Response | Notes |
|-------|--------------|--------|----------|-------|
| `upload/[chatId].put.ts` | Yes | SELECT chat (validate ownership) | `{ pathname, url, contentType, size }` | S3 upload, 8MB max, validates chat ownership |
| `upload/[...pathname].delete.ts` | Yes | No | 204 No Content | S3 delete, validates path ownership |

**S3 Structure:** `{userId}/{chatId}/{randomSuffix}.{ext}`

---

## Essential vs Redundant Routes

### ESSENTIAL (must exist as standalone in frontend context)
These routes handle frontend-necessary operations that the Hono backend cannot proxy:

| Route | Reason |
|-------|--------|
| `chats/[id].post.ts` | **Core AI streaming** — transforms engine SSE to VAI format; cannot be proxied directly |
| `upload/[chatId].put.ts` | **S3 presigned URL generation** — requires server-side S3 client |
| `upload/[...pathname].delete.ts` | **S3 delete** — requires server-side credentials |
| `chats/[id].get.ts` | **Chat loading with messages** — data access layer |
| `chats/[id].delete.ts` | **Chat deletion with S3 cleanup** — side effects |
| `auth/get-session.get.ts` | **SSR session hydration** — needed for server-side rendering |

### REDUNDANT (can be fully replaced by Hono backend)
These routes could be handled entirely by Hono API:

| Route | Replacement |
|-------|-------------|
| `auth/[...all].ts` | Handled by Hono auth routes (reimplemented) |
| `auth/verify.get.ts` | Email verification in Hono auth flow |
| `auth/accounts.get.ts` | Account listing in Hono user routes |
| `auth/delete-account.delete.ts` | User deletion in Hono user routes |
| `chats.post.ts` | Chat creation in Hono chats routes |
| `chats.get.ts` | Chat listing in Hono chats routes |
| `chats/search.get.ts` | Chat search in Hono chats routes |

---

## Migration Phases

### Phase 1: Auth Infrastructure (Hardest)
1. Reimplement `better-auth`-style auth with Hono
2. OAuth providers: Discord, GitHub, Google, Slack
3. Session management with cookies
4. Email verification flow
5. Account linking/unlinking

**Priority: HIGH** — Everything else depends on auth.

### Phase 2: Core Chat CRUD (Simple)
1. `chats.post.ts` → Hono POST `/chats`
2. `chats.get.ts` → Hono GET `/chats`
3. `chats/search.get.ts` → Hono GET `/chats/search`
4. `chats/[id].delete.ts` → Hono DELETE `/chats/:id`
5. `chats/[id].get.ts` → Hono GET `/chats/:id`

**Priority: MEDIUM** — Can be done in parallel with Phase 1.

### Phase 3: Complex Chat (Hard)
1. `chats/[id].post.ts` — Main streaming endpoint
   - Requires engine token generation
   - Requires SSE transformation
   - Requires on-finish DB persistence

**Priority: MEDIUM** — Can be partially proxied initially.

### Phase 4: File Upload/Delete (Medium)
1. `upload/[chatId].put.ts` → Hono PUT `/upload/:chatId`
2. `upload/[...pathname].delete.ts` → Hono DELETE `/upload/*`

**Priority: LOW** — Can use direct S3 URLs initially.

---

## Key Dependencies to Reimplement

| Package | Purpose | Notes |
|---------|---------|-------|
| `better-auth` | Auth framework | Core OAuth, session, email flows |
| `better-auth/adapters/drizzle` | DB adapter | User/account/session storage |
| `drizzle-orm` | DB queries | All chat/message queries |
| `aws-sdk/client-s3` | S3 operations | Upload, delete, presign |
| `jose` | JWT handling | Engine token generation |
| `ai` | AI SDK | `generateText`, `createUIMessageStream` |

---

## Recommendations

1. **Don't migrate auth first** — Consider using Nuxt as BFF (Backend for Frontend) initially, proxying to Hono for business logic
2. **Keep `auth/[...all].ts` in Nuxt** — Auth is the most complex; keep it in Nuxt until Hono auth is battle-tested
3. **Frontend uses VAI SDK** — The streaming chat route (`chats/[id].post.ts`) uses VAI SDK which expects a specific response format; this must stay as-is or be reimplemented identically
4. **Consider hybrid approach** — Nuxt handles SSR + auth, Hono handles API + AI engine

---

*Generated: 2026-03-26*
