# Testing Coverage Audit — Alizé Backend (`/data/workspace/alize-code/apps/api/`)

**Date:** 2026-03-26  
**Auditor:** Subagent  
**Framework found:** None  
**Test files found:** 0

---

## 1. Test Infrastructure — ❌ NONE

| Check | Result |
|---|---|
| Test directory (`__tests__/`, `tests/`) | ❌ Not found |
| Test files (`*.test.ts`, `*.spec.ts`) | ❌ Not found |
| Testing framework (Vitest, Jest, Node test) | ❌ Not configured |
| Test scripts in `package.json` | ❌ None |
| Vitest / Jest / Playwright config | ❌ None |

The `package.json` devDependencies contain only:
- `@types/node`
- `tsx`
- `typescript`

No test runner is present.

---

## 2. Coverage Matrix

| Category | Tested? | Test File | What It Covers |
|---|---|---|---|
| Auth routes (`/auth/register`, `/auth/login`, `/auth/me`) | ❌ NO | — | — |
| Chat routes (`/api/chat/send`) | ❌ NO | — | — |
| Document worker (`processDocument`) | ❌ NO | — | — |
| Cron worker (`executeCron`) | ❌ NO | — | — |
| Improvement worker (`runImprovement`) | ❌ NO | — | — |
| Tools (`searchDocsTool`, `doc-tools`, `cron-tools`, `memory-tools`, `skill-tools`, `deepResearchTool`) | ❌ NO | — | — |
| Event bus (`eventBus.publish`) | ❌ NO | — | — |
| DB client (`sql`, `sqlOne`) | ❌ NO | — | — |
| Channel adapters (`channels.send`) | ❌ NO | — | — |
| Document pipeline (chunk, embed, store, parse, download) | ❌ NO | — | — |
| Auth middleware | ❌ NO | — | — |
| Skill loader | ❌ NO | — | — |
| Prompt builders | ❌ NO | — | — |

**Overall coverage: 0%**

---

## 3. What Is NOT Tested But Should Be

### 🔴 Critical (no test coverage at all)

- **Auth routes** — `POST /auth/register`, `POST /auth/login`, `GET /auth/me`
  - JWT generation, slug uniqueness, DB writes for users/orgs
  - Error paths: duplicate email, missing fields, invalid org

- **Chat route** — `POST /api/chat/send`
  - SSE streaming, tool-call events, error handling

- **All three workers** (document, cron, improvement)
  - These run async via BullMQ — no coverage means bugs in the core business logic go undetected

- **DB layer** — `sql()`, `sqlOne()` in `src/db/client.ts`
  - Any schema change or query bug breaks everything silently

### 🟡 Important

- **Tools** — `searchDocsTool`, `doc-tools`, `cron-tools`, `memory-tools`, `skill-tools`, `deepResearchTool`
  - These are called by agents; mocked/unit tests would catch breaking changes fast

- **Channel adapters** — `channels.send()` for email/sendgrid, slack, discord
  - No coverage means broken integrations go unnoticed

- **Auth middleware** (`src/server/middleware/auth.ts`)
  - JWT verification, `c.get('userId')` / `c.get('orgId')`

- **Skill loader** — `loadSkillBySlug`, `loadRelevantSkills`
  - Critical for cron reliability

- **Document pipeline** — chunking, embedding, vector storage
  - Embedding/model changes can silently degrade quality

### 🟢 Nice to Have

- **Prompt builders** (all `src/prompts/*.ts`)
  - Hard to test semantically, but structural output tests (JSON parsing, string presence) would help

- **Event bus** — `eventBus.publish()`
  - Could be mocked and verified

---

## 4. Integration & E2E Tests

| Type | Present? |
|---|---|
| Integration tests (route + DB) | ❌ NO |
| E2E tests (Playwright, Supertest) | ❌ NO |
| Manual API testing documented | ❌ NO |

---

## 5. Recommendations

### Immediate (before next feature)

1. **Install Vitest** — it's the modern choice for Node.js/TypeScript, zero-config with Vite:
   ```bash
   npm install -D vitest @vitest/coverage-v8
   ```
   Add to `package.json`:
   ```json
   "test": "vitest",
   "test:coverage": "vitest run --coverage"
   ```

2. **Write unit tests for auth routes** — easiest win, well-defined inputs/outputs:
   - `auth.test.ts` — test slug gen, JWT signing, register/login DB rows

3. **Add a `__tests__/` directory** at `src/server/routes/` and `src/workers/`

### Short-term

4. **Unit test the 3 workers** with BullMQ job mocking:
   - Mock `sql`, `sqlOne`, `downloadFile`, `eventBus`, `channels.send`
   - Test happy path + error branches for each worker

5. **Tool tests** — mock LLM responses, verify correct tool was called

6. **DB schema migration tests** — verify queries against real schema changes

### Medium-term

7. **Integration tests** with a real (or Docker) libsql instance — test full request → DB → response cycles

8. **E2E tests** — Supertest for HTTP routes, Playwright for SSE streaming

---

## 6. Quick Wins to Add Now

```typescript
// src/server/routes/__tests__/auth.test.ts
import { describe, it, expect, vi } from 'vitest'
import { generateUniqueSlug } from '../auth'

// Example: test slug generation in isolation
describe('generateUniqueSlug', () => {
  it('removes special chars', () => {
    const slug = generateUniqueSlug('My Org!')
    expect(slug).toMatch(/^my-org-[a-f0-9]{8}$/)
  })
})
```

---

## Summary

> **The Alizé backend has zero test coverage.** No test framework, no test files, no coverage. Every route, worker, tool, and integration point is entirely untested. Given that this codebase handles authentication (JWT, user/org data), async workers (document processing, cron execution, improvement loops), and external channel integrations, adding tests — starting with auth routes and workers — is the highest-ROI improvement available.
