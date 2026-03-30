# Alizé Frontend — Missing Features & Incomplete Implementations Audit

**Date:** 2026-03-26
**Scope:** `/data/workspace/alize-code/apps/web/`
**Status:** Generally well-structured; several functional gaps and stub implementations remain.

---

## Critical — Must Fix Before Launch

### 1. No `/api/auth/get-session` endpoint exists
- **Location:** Server API (`/server/api/auth/`)
- **Issue:** `useAuth.ts` calls `$fetch('/api/auth/get-session', ...)` but the auth route only has a catch-all `/[...all].ts`. The better-auth catch-all handles its own routes but `get-session` may not be covered, causing auth state to fail.
- **Fix:** Either verify better-auth's route exports include `get-session`, or create an explicit `server/api/auth/get-session.get.ts` that calls `auth.api.getSession()`.
- **Priority:** Critical

### 2. Chat title is always empty after first message
- **Location:** `server/api/chats/[id].post.ts` — title generation block
- **Issue:** Title generation uses `generateText` with a selected model, but the selected model (`getModel(model)`) uses `APP_MODELS` which wraps around `@ai-sdk/anthropic` / `@ai-sdk/mistral`. This assumes the AI SDK providers are configured with real API keys. If they aren't, title generation silently fails and `chat.title` stays `''`.
- **Fix:** Ensure `MINIMAX_API_KEY` and `MISTRAL_API_KEY` env vars are set, or wrap title generation in a try/catch that falls back to using the first user message's first 30 chars.
- **Priority:** Critical

### 3. File upload has no progress tracking
- **Location:** `app/composables/useFileUpload.ts`
- **Issue:** `uploadFile()` uploads via `FormData`/$fetch but there's no `onUploadProgress` handler. The UI only shows a spinning loader with no byte-level progress.
- **Fix:** Use `XMLHttpRequest` (or a wrapper like `axios` with `onUploadProgress`) instead of `$fetch` for uploads to report real progress.
- **Priority:** Critical (UX)

### 4. No pagination on `/chats` page
- **Location:** `app/pages/chats.vue`, `server/api/chats.get.ts`
- **Issue:** `useFetch('/api/chats')` returns all chats at once. With hundreds of chats this will be slow and the UI will be overwhelmed.
- **Fix:** Add `limit`/`offset` query params and "Load more" / infinite scroll pagination.
- **Priority:** Critical (performance)

---

## High — Significant Functionality Gaps

### 5. No user account settings / profile page
- **Location:** `app/pages/` — no settings route
- **Issue:** Users cannot change their name, email, password, or avatar. The `UserMenu.vue` has theme and appearance options but no profile management.
- **Fix:** Create `app/pages/settings.vue` (or `profile.vue`) with forms to update name, email, password. Wire up `authClient.updateUser()` or equivalent better-auth endpoint.
- **Priority:** High

### 6. OAuth social providers UI wired but no account linking UI
- **Location:** `app/components/UserMenu.vue`, `app/pages/login.vue`
- **Issue:** Login page shows Discord, GitHub, Google, Slack buttons. But there's no UI in settings to link/unlink social accounts, see which accounts are connected, or set a primary provider.
- **Fix:** Create a "Connected Accounts" section in settings. Use better-auth's `authClient.listAccounts()` / `authClient.linkAccount()` / `authClient.unlinkAccount()`.
- **Priority:** High

### 7. No chat sharing functionality
- **Location:** `app/pages/chat/[id].vue`
- **Issue:** No share button, no "Copy link" for a chat conversation. Users cannot share a chat with others.
- **Fix:** Add a share button that either (a) generates a public shareable link, or (b) copies the chat export to clipboard. Requires backend support for public chat links.
- **Priority:** High

### 8. Chat search is wired in sidebar but has no real search results
- **Location:** `app/layouts/default.vue` — `UDashboardSearch`
- **Issue:** `UDashboardSearch` is rendered with `groups` from `useChats()`, which only includes chat titles. There's no actual search endpoint that filters chats by title or message content.
- **Fix:** Implement a `/api/chats/search` endpoint and update the search component to query it, or implement client-side filtering of existing chats by title.
- **Priority:** High

### 9. Chat messages don't include file attachment text when sent to engine
- **Location:** `server/api/chats/[id].post.ts`
- **Issue:** `messageText` extraction only looks at `parts.filter(p => p.type === 'text')`. If a user sends "analyze this" with a PDF, the text sent to the engine is just "analyze this" — the file URL is not included. The backend stores the file in S3 but never tells the AI about it.
- **Fix:** Include file URLs in the `messageText` sent to the engine, or pass file parts separately to the engine API.
- **Priority:** High

### 10. App SEO meta still references "Nuxt AI Chatbot template"
- **Location:** `app/app.vue`
- **Issue:** `title = 'Nuxt AI Chatbot template'` and description references the Nuxt template, not Alizé.
- **Fix:** Update to Alizé branding. Also `ogImage` and `twitterImage` point to `ui.nuxt.com`.
- **Priority:** High

---

## Medium — Notable Gaps

### 11. Weather tool uses mock/random data
- **Location:** `shared/utils/tools/weather.ts` — `execute` function
- **Issue:** `execute` returns `Math.random()` values and a `setTimeout` delay. No real weather API (OpenWeather, etc.) is integrated.
- **Fix:** Integrate OpenWeatherMap API or similar. Add `OPENWEATHER_API_KEY` to env and call the API in the `execute` function.
- **Priority:** Medium

### 12. User cannot delete their account
- **Location:** `app/components/UserMenu.vue`
- **Issue:** No "Delete account" option. The logout is present but account deletion is missing.
- **Fix:** Add account deletion option (with a confirmation flow) using better-auth's admin plugin or a custom endpoint.
- **Priority:** Medium

### 13. Signup doesn't send verification email
- **Location:** `server/utils/auth.ts`
- **Issue:** `emailAndPassword: { enabled: true }` but no `emailConfirmation` or verification flow configured. Users can sign up with any email.
- **Fix:** Configure better-auth's email verification if email confirmation is desired.
- **Priority:** Medium

### 14. UserMenu "Templates" links to external Nuxt UI templates
- **Location:** `app/components/UserMenu.vue`
- **Issue:** The Templates submenu links to `starter-template.nuxt.dev`, `landing-template.nuxt.dev`, etc. These link to Nuxt UI's own templates, not Alizé. This is confusing and leaks users to competitor landing pages.
- **Fix:** Either remove these links, or replace with links to Alizé-specific templates/documentation.
- **Priority:** Medium

### 15. No error boundary for failed chat loads
- **Location:** `app/pages/chat/[id].vue`
- **Issue:** `throw createError({ statusCode: 404 })` if chat not found, but there's no graceful handling for network errors or 500s. The chat UI just fails silently.
- **Fix:** Add error handling with a retry button and user-friendly error messages.
- **Priority:** Medium

### 16. `useHighlighter` initialized with `await` at component level
- **Location:** `app/components/prose/PreStream.vue`
- **Issue:** `const highlighter = await useHighlighter()` in `<script setup>` block. This can cause hydration mismatches and blocks rendering. Shiki highlighter should be initialized lazily or in a plugin.
- **Fix:** Initialize the highlighter in a Nuxt plugin (`app/plugins/shiki.client.ts`) and provide it via `useState` / `useNuxtApp()`.
- **Priority:** Medium

### 17. No loading skeleton for initial chat load
- **Location:** `app/pages/chat/[id].vue`
- **Issue:** `await useFetch` blocks rendering with no skeleton. For slow connections, the chat page is blank while messages load.
- **Fix:** Add `<USkeleton>` or `useAsyncData` with a loading state.
- **Priority:** Medium

---

## Low — Nice to Have

### 18. Chat export (JSON, Markdown) not implemented
- **Location:** `app/pages/chat/[id].vue`
- **Issue:** No export button to download a chat as JSON or Markdown.
- **Fix:** Add export functionality using `JSON.stringify(messages)` or a markdown formatter.
- **Priority:** Low

### 19. No keyboard shortcut for regenerating response
- **Location:** `app/pages/chat/[id].vue`
- **Issue:** Regenerate button exists but there's no keyboard shortcut (e.g., `Cmd+Shift+R`).
- **Fix:** Add `defineShortcuts` for regenerate.
- **Priority:** Low

### 20. No empty state illustrations on error pages
- **Location:** `app/error.vue`
- **Issue:** `UError` is used but no custom illustration or recovery actions beyond "Go Home".
- **Fix:** Customize the error page with more helpful actions.
- **Priority:** Low

### 21. `DashboardSearch` placeholder is static
- **Location:** `app/layouts/default.vue`
- **Issue:** The search placeholder says "Search chats..." but doesn't live-filter — it only navigates to existing chat links.
- **Fix:** Either make it a proper live search or update the placeholder text to reflect the limitation.
- **Priority:** Low

### 22. CSV file upload restriction not enforced in upload button
- **Location:** `app/components/FileUploadButton.vue`
- **Issue:** The file upload button doesn't restrict accepted types visually. The `accept` pattern is in `useFileUpload` config but the button itself shows no indication of CSV-only.
- **Fix:** The `acceptPattern` is `'image/*,application/pdf,.csv,text/csv'` — update the tooltip or button to reflect the accepted types.
- **Priority:** Low

### 23. No rate limiting feedback to user
- **Location:** `app/pages/chat/[id].vue` — `onError` handler
- **Issue:** If the engine returns a 429 or rate limit error, the user just sees the raw error message.
- **Fix:** Detect rate limit errors and show a friendly "Slow down" message with retry timing.
- **Priority:** Low

---

## Stub / Placeholder References

| Item | Location | Notes |
|------|----------|-------|
| Quick chat "Why use Nuxt UI?" | `app/pages/index.vue` | Template leftover — references Nuxt UI, not Alizé |
| Quick chat "What is the weather in Bordeaux?" | `app/pages/index.vue` | Works via mock weather tool |
| Quick chat "Show me a chart of sales data" | `app/pages/index.vue` | Works via chart tool with mock data |
| UserMenu "Templates" submenu links | `app/components/UserMenu.vue` | Links to Nuxt UI template gallery |
| `nuxt.config.ts` commented `highlight: { }` | `nuxt.config.ts` | MDC highlight is disabled — code blocks rely on Shiki in `PreStream` |

---

## Summary by Priority

| Priority | Count |
|----------|-------|
| Critical | 4 |
| High | 6 |
| Medium | 7 |
| Low | 6 |

### Quick Wins (Under 30 min each)
1. Fix app title/description in `app.vue`
2. Remove or replace UserMenu "Templates" links
3. Add rate limit detection in chat error handler
4. Update Quick Chat labels to be Alizé-specific
5. Add "Load more" pagination to chats page
