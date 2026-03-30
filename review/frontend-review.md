# Alizé Nuxt Web App - Frontend Code Review

## Overview

Reviewed the Alizé Nuxt web app at `/data/workspace/alize-code/apps/web/`. The app is a chat interface built with Nuxt 3, Nuxt UI, and the `ai` SDK for streaming. Auth is handled via Better Auth.

---

## 1. Chat Interface

### `pages/chat/[id].vue`

**Bugs/Issues:**
- **Race condition on mount**: `chat.regenerate()` is called in `onMounted` if there's only 1 message. If the initial fetch hasn't resolved or the chat stream hasn't fully initialized, this could fail silently.
- **Title update UX**: The `onData` handler for `data-chat-title` only calls `refreshNuxtData('chats')`. The chat title won't update in the sidebar until the page refreshes, which is a poor UX.
- **Unsafe JSON parse in error handler**:
  ```ts
  const { message } = typeof error.message === 'string' && error.message[0] === '{' ? JSON.parse(error.message) : error
  ```
  If the JSON parse fails, the destructuring will use the original `error` object (without a `message` property), and `message` will be `undefined`.
- **Regenerate with files**: The `regenerate()` method is called without passing files. If the original message had attachments, they won't be resent.

**Type Safety:**
- `WeatherUIToolInvocation` and `ChartUIToolInvocation` types are used but not imported - these are likely global types that could cause runtime errors if the actual data doesn't match.

**UX:**
- No loading skeleton while the chat is being fetched initially.
- The `cache: 'force-cache'` on `useFetch` means stale data could be shown.

### `composables/useChats.ts`

**Bugs/Issues:**
- **Incorrect date calculation**:
  ```ts
  const oneWeekAgo = subMonths(new Date(), 0.25) // ~7 days ago
  ```
  `subMonths(date, 0.25)` doesn't mean "0.25 months". `subMonths` expects an integer. This should be `subDays(new Date(), 7)` for ~7 days ago.

---

## 2. File Upload

### `composables/useFileUpload.ts`

**Bugs/Issues:**
- **Login check only at start**: The `if (!loggedIn.value) return` check happens before uploads start. If a user logs out mid-upload, the uploads continue and may produce auth errors.
- **No progress tracking**: Uploads have no progress events - they just show "uploading" then either succeed or fail.
- **Silent failure on delete**: The DELETE request in `removeFile` only `console.error`s on failure, leaving the user unaware if file deletion from S3 failed.
- **CSRF could be undefined**: `useCsrf()` returns `csrf` which could theoretically be undefined, sending an empty header.
- **Race condition on file removal**: When `removeFile` is called, it removes from `files.value` and revokes the object URL, but if `uploadFile` completes at the same moment, it could write to an index that no longer exists.

**Security:**
- No client-side file type/size validation before upload. The 8MB limit is only enforced on the server.

**UX:**
- Files are uploaded immediately when added. There's no "confirm and send" flow - files go to S3 as soon as they're dropped.

### `components/FileUploadButton.vue`

**Issues:**
- The button correctly disables when not logged in and shows a tooltip, but there's no loading state while uploads are in progress (it just shows the ghost button).

### `DragDropOverlay.vue`

**Issues:**
- The `v-if="show && loggedIn"` logic is correct, but if the user drags files while logged out, there's no visual feedback that they can't upload.

---

## 3. Auth Flow

### `composables/useAuth.ts`

**Security Issues:**
- **Open redirect vulnerability**: `normalizeReturnTo`:
  ```ts
  if (typeof target !== 'string' || !target.startsWith('/') || target.startsWith('//')) {
    return '/'
  }
  ```
  This doesn't block absolute URLs like `https://evil.com`. An attacker could craft a login link like `/login?returnTo=https://evil.com` and trick users.
  
- **Inconsistent returnTo handling**: In `guest.ts`, there's a check `returnTo === '/login' || returnTo === '/signup'` to redirect to `/`, but the `normalizeReturnTo` function allows paths like `/login` (which starts with `/`). The check should use `startsWith` to be safe, but more importantly, absolute URLs should be blocked.

**Missing Features:**
- No password validation rules displayed on signup.
- No "forgot password" flow.
- No session expiry handling or refresh mechanism visible in the frontend.

### `middleware/auth.ts` & `middleware/guest.ts`

**Issues:**
- Both middlewares call `resolveAuthSession()` which on the client side calls `session.value.refetch()`. If the session is being fetched, there's no loading state shown to the user - they just see a flash before redirect.

**Security:**
- The auth middleware correctly protects routes, but there's no "redirect to login with returnTo" protection for API routes (API routes just return 401).

### `pages/login.vue`

**UX Issues:**
- There's no "wrong password" vs "user not found" distinction - both return the same error to prevent enumeration, but the error message is generic.
- Social provider buttons don't have `loading` states on the buttons themselves (only the internal `pendingProvider` ref is set).

**Type Safety:**
- `SocialProvider` type is defined inline but could be shared with `useAuth.ts`.

### `pages/signup.vue`

**UX Issues:**
- No password strength indicator.
- No confirmation password field.
- The form immediately signs in after successful signup, but there's no indication this is happening.

---

## 4. API Proxy Setup & SSE

**How it works:**
- The app uses Nuxt's server routes as a proxy. Chat messages go to `/api/chats/[id]` (POST) which streams SSE.
- The frontend uses `@ai-sdk/vue`'s `Chat` component with `DefaultChatTransport`.

**Issues:**
- The `Chat` component from `@ai-sdk/vue` with `DefaultChatTransport` sends a POST request with the messages in the body. This is correct.
- The SSE stream is processed via `createUIMessageStream` on the backend, which should properly format messages.
- **Potential issue**: The `onData` handler only handles `data-chat-title`. If the backend sends other transient data events, they're ignored.

---

## 5. Overall App Structure & State Management

**Good patterns:**
- Clean separation between server API routes and frontend composables.
- CSRF protection is properly implemented.
- Auth state is centralized via `useAuthState`.
- File uploads are handled consistently with status tracking.

**Issues:**
- **No optimistic updates**: Creating a chat on the index page requires waiting for the server round-trip before navigating.
- **No real-time sync**: If the user opens two tabs, changes in one won't reflect in the other.
- **Chat list caching**: `refreshNuxtData('chats')` is called after title generation, but the sidebar doesn't automatically update the title.
- **No retry on SSE failure**: If the stream disconnects, there's no automatic retry.

---

## 6. Type Safety Gaps

1. `WeatherUIToolInvocation` and `ChartUIToolInvocation` types are used but never imported - likely global types that aren't validated.
2. `FileAvatar` component doesn't have fully typed props - uses `any` for status.
3. The `chat.sendMessage` accepts `{ text, files }` but the `text` field should be verified against the AI SDK types.

---

## 7. Summary of Critical Issues

| Severity | Issue | Location |
|----------|-------|----------|
| **HIGH** | Open redirect possible via `returnTo` parameter | `useAuth.ts` |
| **HIGH** | `subMonths(date, 0.25)` doesn't do what it claims | `useChats.ts` |
| **MEDIUM** | Unsafe JSON parse in error handler | `pages/chat/[id].vue` |
| **MEDIUM** | Files upload without client-side validation | `useFileUpload.ts` |
| **MEDIUM** | No loading state during auth redirect | Middlewares |
| **LOW** | Regenerate doesn't preserve files | `pages/chat/[id].vue` |
| **LOW** | No retry on SSE failure | `pages/chat/[id].vue` |

---

## 8. Recommendations

1. **Fix the open redirect**: Add absolute URL detection in `normalizeReturnTo`:
   ```ts
   if (typeof target !== 'string' || target.startsWith('//') || /^https?:\/\//.test(target)) {
     return '/'
   }
   ```

2. **Fix date calculation**: Use `subDays` instead of `subMonths(0.25)`.

3. **Add client-side file validation** in `useFileUpload.ts` before calling `uploadFile`.

4. **Add auth loading state**: Show a loading indicator while auth is being verified.

5. **Add error boundary**: Wrap the chat interface to handle stream failures gracefully.

6. **Consider**: Adding `preserveScroll` behavior when regenerating, and properly passing files to `regenerate()`.
