# Alizé Frontend — Missing Product Features

**Date:** 2026-03-29
**Frontend stack:** Nuxt 4 + Nuxt UI 4 + Nuxt Content
**Repo:** github.com/lschvn/alize-front

---

## Context

The current `alize-front` is a **fresh Nuxt starter reset** — it has the shell of a dashboard (sidebar, navbar, settings, tasks, documents, chat pages) but is ~90% stub data. This is separate from an earlier frontend iteration (reviewed 2026-03-26) that was more wired but appears to have been replaced or abandoned.

Reference: `alize/BRIEF.md` defines Alizé as a managed IA agent service for French PME/ETI. Key use cases: service client, commerce, RH, ops, direction. Key value: agents connected to real business tools, governed, monitored, with human oversight.

---

## Pages that should exist but don't

| Page | Why it matters |
|------|---------------|
| `/` — Landing page | The root shows the Nuxt UI starter template. The Alizé landing copy exists in `alize/landing-page.md` but has never been implemented. This is the first touchpoint for prospects. |
| `/agents` | No agent management page. Users cannot see which agents are deployed, their status, or add new ones. |
| `/agents/[id]` | No agent detail view — config, connected tools, recent runs, performance metrics. |
| `/monitor` | No real-time monitoring. No page for viewing live task logs, run durations, error rates, or system health. |
| `/workflows/builder` | No workflow creation UI. How do users create a new scheduled task? What agents? What parameters? What schedule? |
| `/integrations` | No page showing connected third-party tools (CRM, email, Slack, etc.). This is Alizé's core differentiator — agents connected to real business software. |
| `/billing` | No billing/subscription management. Plan overview, current usage, invoice history. |
| `/audit-log` | No audit trail. Enterprise clients need to see who did what, when, what data was accessed — for compliance and trust. |
| `/sandbox` | No isolated testing environment where users can run a workflow with dummy data before activating it for real. |
| `/onboarding` | No first-login tour. New users arrive to a stub dashboard with no guidance on what to do first. |

---

## Features that are stub or missing

### Chat (`/chat`)

- Quick chat prompts reference Vue, Tailwind, Nuxt — completely wrong for Alizé's audience (French professional services: accounting, legal, consulting)
- No real AI streaming — all conversations are hardcoded mock data
- No chat history persistence — mock list is fake and not editable
- Chat title auto-regeneration is stubbed
- File attachments are stubbed

### Tasks (`/tasks`)

- All data is hardcoded mock (fake names: "Nina Laurent", "Sarah Liu", "Benjamin Canac")
- "Create workflow" button is a no-op
- No actual cron/schedule configuration UI
- No live run logs — just static mock arrays
- `tasks/[id]` is empty

### Documents (`/documents`)

- Mock file tree with placeholder names (kanban.md, roadmap.md, specs.pdf)
- No real file upload
- No RAG / knowledge base pipeline wired
- No folder/permission management

### Settings

- Profile page pre-filled with "Benjamin Canac / ben@nuxtlabs.com" — Nuxt UI demo data
- Members page is empty UI with no functionality
- Notifications settings reference "Nuxt UI" product, not Alizé
- No API key management
- No team roles or permissions system
- Connected accounts (OAuth) are toast stubs only

### Auth

- Login/Signup OAuth buttons (Google, GitHub) are `toast.add()` stubs — no real OAuth flow
- No magic link email
- No session management
- Password reset flow exists but is untested

### Cross-cutting

- No real-time (SSE) — dashboard requires manual refresh for new data
- No in-app notification system for task failures or completions
- No i18n — everything is English; Alizé targets French companies
- No dark/light mode
- No mobile layout optimization
- No error tracking (Sentry not configured)
- No analytics

---

## Previous audit notes (2026-03-26) — still relevant

These issues were documented against an earlier frontend iteration. Some may still apply if that code is reused:

1. Chat uses Nuxt server routes instead of Hono backend — bypasses Mastra agent entirely
2. File upload endpoint not wired to Hono
3. Profile update API not implemented
4. Chat search endpoint missing in backend
5. OAuth social login not wired
6. Quick chats need French professional prompts
7. Connected accounts tab is empty
8. French date labels missing
9. Chat export missing tool-call content
10. No real-time title update in sidebar
11. No thinking indicator in chat
12. Model preference not persisted
13. No Alizé favicon

See: `alize/review/FRONTEND-MISSING-FEATURES.md`

---

## Backend notes

The frontend references a Hono + Mastra backend (`alize-code/apps/api/`). That backend has its own audit in `alize/review/missing-features.md`.

Key backend gaps relevant to the frontend:
- Chat streaming should connect to Hono `/api/chat/send`, not Nuxt server routes
- File upload needs Hono `POST /api/documents/upload`
- Skills catalog search falls back to ILIKE (no vector embeddings yet)
- Only `web` channel works; Slack, Discord, Teams, WhatsApp, Email are stubs

---

## Priority recommendation

**Step 1:** Landing page (`/`) — prospects need to understand what Alizé is before they log in.

**Step 2:** Landing page done → auth → dashboard with 1 real workflow connected to a real AI model. Strip everything else until that loop works.

**Step 3:** Then layer in agents, monitoring, integrations, billing.

Trying to build everything in parallel is how the first frontend iteration ended up abandoned.
