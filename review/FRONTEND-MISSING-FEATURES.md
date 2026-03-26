# Alizé Frontend — Missing Features

**Date:** 2026-03-26  
**Frontend stack:** Nuxt 3 + Nuxt UI 3 + Vercel AI SDK  
**Backend:** Hono + Mastra (partial integration)

---

## Critical — Frontend Not Connected to Alizé Agent

The chat UI uses **Vercel AI SDK → Nuxt server route** (`/api/chats/[id]`), NOT the Mastra agent via Hono. The two systems are completely decoupled.

| What happens | What should happen |
|---|---|
| `DefaultChatTransport` streams from `/api/chats/[id]` (Nuxt) | Should stream from `/api/chat/send` (Hono `/api/chat/send`) |
| Nuxt server proxies to external ENGINE_URL | Mastra agent handles everything in Hono |
| Different model, no shared memory | Same agent, shared memory |
| File attachments stored via Nuxt route | File attachments sent to Mastra agent tool |

**Impact:** Chat works but bypasses the Alizé agent entirely. Skills, memory, document tools, and cron tools are inaccessible from the chat UI.

---

## P0 — Doesn't Work

### 1. Chat → wrong backend
**File:** `pages/chat/[id].vue`
**Problem:** `DefaultChatTransport({ api: `/api/chats/${id}` })` — hits Nuxt server, not Hono. Mastra agent is never consulted.
**Fix:** Rewrite to call `POST /api/chat/send` with SSE streaming from the Hono backend.

### 2. File upload endpoint missing in Hono
**File:** `composables/useFileUpload.ts` → uploads to `/api/documents/upload`
**Problem:** Nuxt server route `upload/[chatId].put.ts` handles this. Not migrated to Hono.
**Fix:** Create `POST /api/documents/upload` in Hono (S3 upload with presigned URLs).

### 3. Chat title regeneration loses files
**File:** `pages/chat/[id].vue`, `onMounted`
**Problem:** `chat.value?.regenerate()` is called without passing `uploadedFiles`.
**Fix:** Store files in state and re-send with regenerate.

### 4. Profile update not wired
**File:** `components/settings/ProfileTab.vue`
**Problem:** API call is commented out (`TODO: implement`). No `PATCH /api/settings/profile` in Hono.
**Fix:** Implement `PATCH /api/settings/profile` in Hono and wire it up.

### 5. Chat search — endpoint doesn't exist
**File:** `layouts/default.vue` (sidebar search)
**Problem:** Search box in sidebar, but no `/api/chats/search` route in Hono.
**Fix:** Create `GET /api/chats/search?q=` in Hono.

### 6. OAuth social login not wired
**File:** `pages/signup.vue`, `pages/login.vue`
**Problem:** UI shows Discord, GitHub, Google, Slack buttons but they're not connected to the Hono auth flow.
**Fix:** Wire OAuth buttons to the Hono auth endpoints or implement social login in the frontend.

---

## P1 — Broken or Incomplete

### 7. Quick chats are hardcoded examples
**File:** `pages/index.vue`
**Problem:** Quick chats reference Vue, UnJS, Tailwind, VueUse — generic AI assistant examples, not relevant to Alizé's positioning.
**Fix:** Replace with French-language prompts relevant to professional services (accounting, legal, consulting).

### 8. Connected accounts tab is empty UI
**File:** `components/settings/ConnectedAccountsTab.vue`
**Problem:** Shows Discord/GitHub/Google/Slack with "Connect" buttons but nothing happens on click.
**Fix:** Wire to OAuth link/unlink endpoints (or remove if not supported).

### 9. Chat date grouping in English
**File:** `composables/useChats.ts`
**Problem:** Groups labelled "Today", "Yesterday", "Last week", "Last month" — should be French.
**Fix:** Replace with "Aujourd'hui", "Hier", "7 derniers jours", "30 derniers jours".

### 10. `useFileUploadWithStatus` — implementation hidden
**File:** Used in `pages/chat/[id].vue` and `pages/index.vue`
**Problem:** The composable is referenced but its source isn't visible in the checked files. Appears to be in Nuxt server utilities.
**Fix:** Ensure it's fully implemented and works with Hono upload endpoint.

### 11. Chat export — tool calls not exported
**File:** `components/ChatExportButton.vue`
**Problem:** Exports text and reasoning, but not tool-call/tool-result parts. If user exports a chat with document search, the tool context is lost.
**Fix:** Include tool-call/tool-result parts in JSON export.

---

## P2 — Missing but Lower Priority

### 12. No real-time chat title in sidebar
**File:** `layouts/default.vue`
**Problem:** Sidebar shows chat list but doesn't update the title in real-time when AI renames a chat.
**Fix:** Subscribe to SSE events and update the chat title reactively.

### 13. No loading state for chat search
**File:** `layouts/default.vue` (search input)
**Problem:** Search input exists but there's no loading spinner while results load.
**Fix:** Add `:loading` state to the search input.

### 14. Avatar upload is stubbed
**File:** `components/settings/ProfileTab.vue`
**Problem:** `handleAvatarUpload` shows a toast "not yet implemented".
**Fix:** Implement avatar upload to S3 + `PATCH /api/settings/profile` with avatar URL.

### 15. UserMenu links are placeholder
**File:** `components/UserMenu.vue`
**Problem:** Documentation links to `https://alize-docs.example.com`, GitHub to `https://github.com/alize-ai/alize`.
**Fix:** Update to real URLs when docs/repo are live.

### 16. No keyboard shortcut for chat search
**File:** `layouts/default.vue`
**Problem:** Common pattern is `Cmd+K` to focus search. Not implemented.
**Fix:** Add `keydown` listener for `Cmd+K` / `Ctrl+K`.

### 17. No empty state for search results
**File:** `layouts/default.vue` (search results)
**Problem:** If search returns 0 results, nothing is shown (not even a "no results" message).
**Fix:** Show empty state message in French.

### 18. Chats page — `loadMore` not connected to scroll
**File:** `pages/chats.vue`, `composables/useChats.ts`
**Problem:** `loadMore()` is defined but never called — no infinite scroll on the chats list.
**Fix:** Add an intersection observer or scroll listener to trigger `loadMore`.

### 19. No notification when AI is thinking
**File:** `pages/chat/[id].vue`
**Problem:** Only `streaming`/`done` states. No intermediate "AI is thinking..." indicator.
**Fix:** Show a thinking indicator in the chat messages area when status is `'streaming'` but no message chunk received yet.

### 20. Model select — no persistence
**File:** `components/ModelSelect.vue`
**Problem:** Model preference is selected per-session but not saved to user profile.
**Fix:** Save selected model to localStorage or `PATCH /api/settings/model`.

---

## UI/UX Polish

### 21. Wrong favicon
**File:** `app.vue` links to `/favicon.ico`
**Problem:** Likely the Nuxt UI default favicon.
**Fix:** Add Alizé favicon to `public/`.

### 22. No onboarding tour
**File:** N/A
**Problem:** New users see an empty chat with quick prompts. No explanation of what Alizé can do.
**Fix:** Add a first-login tooltip tour (3-4 steps).

### 23. Chat timestamp format not localized
**File:** `components/ChatMessage` (not reviewed)
**Problem:** Timestamps show in English format ("2 hours ago") instead of French.
**Fix:** Use `useI18n()` and French date formatting with `date-fns` locale.

### 24. No confirmation before deleting last message
**File:** `pages/chat/[id].vue`
**Problem:** Delete button on a message has no confirmation modal.
**Fix:** Add a confirmation modal before deleting a message.

### 25. Responsive — chat input overlaps keyboard on mobile
**File:** `pages/chat/[id].vue` (chat prompt)
**Problem:** On iOS/Android, the chat prompt can be hidden behind the keyboard.
**Fix:** Use `viewport` meta properly, test on real mobile devices.

---

## Summary by Priority

| # | Feature | Priority | Estimated |
|---|---------|---------|-----------|
| 1 | Chat → connect to Mastra via Hono | P0 | Medium |
| 2 | File upload to Hono | P0 | Medium |
| 3 | Regenerate preserves files | P0 | Small |
| 4 | Profile update API + wire | P0 | Small |
| 5 | Chat search endpoint | P0 | Small |
| 6 | OAuth social login | P0 | Medium |
| 7 | Replace quick chats with relevant prompts | P1 | Small |
| 8 | Connected accounts tab | P1 | Medium |
| 9 | French date labels | P1 | Small |
| 10 | Chat export includes tool calls | P1 | Small |
| 11 | File upload composable | P1 | Medium |
| 12 | Real-time title update in sidebar | P2 | Small |
| 13 | Search loading state | P2 | Small |
| 14 | Avatar upload | P2 | Small |
| 15 | Fix UserMenu links | P2 | Trivial |
| 16 | Cmd+K shortcut | P2 | Small |
| 17 | Empty search state | P2 | Small |
| 18 | Infinite scroll on chats | P2 | Small |
| 19 | Thinking indicator | P2 | Small |
| 20 | Model preference persistence | P2 | Small |
| 21 | Alizé favicon | P2 | Trivial |
| 22 | Onboarding tour | P2 | Medium |
| 23 | French timestamps | P2 | Small |
| 24 | Delete message confirmation | P2 | Small |
| 25 | Mobile keyboard overlap | P2 | Small |
