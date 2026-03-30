# Alizé Backend — Missing Features Audit

**Audit Date:** 2026-03-26  
**Path:** `/data/workspace/alize-code/apps/api/src/`

---

## Summary

| Category | Critical | High | Medium | Low |
|---|---|---|---|---|
| Channel Adapters | 0 | 2 | 3 | 1 |
| Webhook Routes | 0 | 1 | 2 | 0 |
| Tools | 0 | 0 | 0 | 1 |
| Auth | 0 | 0 | 1 | 0 |
| Skills Catalog | 0 | 0 | 1 | 0 |
| **Total** | **0** | **3** | **7** | **2** |

---

## 1. Channel Adapters (`channels/adapters.ts`)

### 1.1 Slack Adapter — WebClient not initialized
- **File:** `channels/adapters.ts`, lines 19–24
- **Issue:** `SlackAdapter` has a commented-out `WebClient` initialization and stubs for `send` and `update`
- **Current:** Logs to console only (`console.log('[slack] → ...')`)
- **Status:** Stub (not started)
- **Importance:** High — Slack is listed as a supported channel in `config/constants.ts` but cannot actually send messages
- **Fix needed:** Initialize `@slack/web-api` `WebClient`, implement real `chat.postMessage` and `chat.update` calls

### 1.2 Slack Adapter — Webhook verification not implemented
- **File:** `channels/adapters.ts`, line 39
- **Issue:** `verifyWebhook()` always returns `true`; Slack signing secret verification is commented out
- **Status:** Stub (not started)
- **Importance:** High — Any Slack webhook could be spoofed without signature verification
- **Fix needed:** Uncomment and implement signature verification using `x-slack-signature` header and timestamp check

### 1.3 Discord Adapter — discord.js Client not initialized
- **File:** `channels/adapters.ts`, lines 53–62
- **Issue:** `DiscordAdapter` has no client initialization; `send` and `update` are stubs logging to console
- **Status:** Stub (not started)
- **Importance:** High — Discord is listed as a supported channel but cannot deliver messages
- **Fix needed:** Initialize `discord.js` `Client`, implement real channel fetch and message send/edit

### 1.4 Teams Adapter — Bot Framework not implemented
- **File:** `channels/adapters.ts`, lines 69–75
- **Issue:** Full stub — only logs to console
- **Status:** Stub (not started)
- **Importance:** Medium — Teams is in `config/constants.ts` channels list but has no implementation
- **Fix needed:** Use Bot Framework SDK or direct REST API to send messages to Teams channels

### 1.5 WhatsApp Adapter — Business API not implemented
- **File:** `channels/adapters.ts`, lines 80–86
- **Issue:** Full stub — only logs to console
- **Status:** Stub (not started)
- **Importance:** Medium — WhatsApp is in `config/constants.ts` channels list but has no implementation
- **Fix needed:** Use WhatsApp Business Cloud API (`POST /v21.0/{phone_number_id}/messages`)

### 1.6 Email Adapter — SMTP/Postmark/Mailgun not implemented
- **File:** `channels/adapters.ts`, lines 91–98
- **Issue:** Full stub — only logs to console
- **Status:** Stub (not started)
- **Importance:** Low — Email is in `config/constants.ts` channels list but email delivery is rarely needed for chat-first product
- **Fix needed:** Implement via Postmark, Mailgun, or nodemailer SMTP

---

## 2. Channel Manager (`channels/manager.ts`)

### 2.1 All normalize*Event functions are stubs
- **File:** `channels/manager.ts`, lines 53–73
- **Issues:**
  - `normalizeSlackEvent()` — line 53: parses to `UnifiedMessage` but always returns `null`
  - `normalizeDiscordEvent()` — line 61: stub, returns `null`
  - `normalizeTeamsEvent()` — line 67: stub, returns `null`
  - `normalizeWhatsAppEvent()` — line 73: stub, returns `null`
- **Status:** Stub (not started) — all four functions
- **Importance:** High — Without normalization, webhooks cannot process incoming messages from these channels; the `processMessage()` function in webhooks/index.ts depends on these returning valid `UnifiedMessage` objects
- **Fix needed:** Parse actual webhook payloads from each platform into `UnifiedMessage` format

---

## 3. Webhook Routes (`server/webhooks/index.ts`)

### 3.1 Teams webhook — no activity parsing
- **File:** `server/webhooks/index.ts`, lines 85–92
- **Issue:** Teams route logs received body but never calls `normalizeTeamsEvent` or `processMessage`
- **Status:** Stub (not started)
- **Importance:** Medium — Teams integration will receive webhooks but never respond
- **Fix needed:** Parse Bot Framework activity payload, call `processMessage()`

### 3.2 WhatsApp webhook — no message parsing
- **File:** `server/webhooks/index.ts`, lines 94–104
- **Issue:** WhatsApp route handles verification challenge but logs body without parsing or processing
- **Status:** Stub (not started)
- **Importance:** Medium — WhatsApp integration cannot receive incoming messages
- **Fix needed:** Parse WhatsApp Cloud API webhook payload, call `processMessage()`

---

## 4. Auth Routes (`server/routes/auth.ts`)

### 4.1 Login sends token directly — magic link not implemented
- **File:** `server/routes/auth.ts`, line 69
- **Issue:** Comment says "TODO: Send magic link email for production"; for MVP it returns token directly
- **Status:** By design (MVP) but flagged
- **Importance:** Medium — In production, returning a token directly from login is a security concern; magic link is planned
- **Note:** This is acceptable for MVP/prototype; production deployment should implement proper magic link flow

---

## 5. Skills Catalog (`server/routes/skills.ts`)

### 5.1 Semantic search for catalog falls back to ILIKE
- **File:** `server/routes/skills.ts`, lines 21–26
- **Issue:** Code embeds the search query but then immediately pops it and falls back to `ILIKE` because "Note: requires embedding column on skill_catalog — add via migration"
- **Status:** Known limitation, workaround in place
- **Importance:** Medium — Semantic search across the skill catalog will not work until a migration adds the `embedding` column to `skill_catalog` table
- **Fix needed:** Add `embedding vector(1024)` column to `skill_catalog` via migration, then remove the fallback code

---

## 6. Tools (`agent/tools/`)

### 6.1 `webSearchTool` / `fetchUrlTool` — no error detail on failure
- **File:** `agent/tools/deep-research.ts`, lines 49–70
- **Issue:** When search or fetch fails, it returns a brief error string rather than structured error info
- **Status:** Functional but could be improved
- **Importance:** Low — Callers get a string but can't programmatically distinguish error types
- **Note:** Not a blocking issue, more of a refinement

---

## 7. Constants (`config/constants.ts`)

### 7.1 All 6 channels declared but only Web is fully implemented
- **File:** `config/constants.ts`, lines 60–63
- **Issue:** `CHANNELS` array lists 6 channels (`web`, `slack`, `discord`, `teams`, `whatsapp`, `email`) but only `web` (SSE) actually works; 5 are stubs
- **Status:** Design decision documented by TODO comments in adapters
- **Importance:** High — The codebase advertises 6 channels but realistically only supports 1 (web)
- **Note:** This is a product decision, not a bug. The stubs are placeholders for future integration.

---

## 8. Mastra Instance (`mastra/index.ts`) vs Main Agent (`agent/mastra.ts`)

### 8.1 Two separate Mastra instances
- **Files:**
  - `agent/mastra.ts` — exports `mastra` (the main Alizé agent, used by chat routes)
  - `mastra/index.ts` — exports a different `mastra` instance (with weather agent and workflow)
- **Issue:** Two `new Mastra({...})` instances exist. The weather agent and workflow are in `mastra/index.ts` but the main Alizé agent is in `agent/mastra.ts`. These are two separate Mastra registries.
- **Status:** Potentially confusing but not blocking
- **Importance:** Low — Weather workflow/agent exists but is never used by the main app
- **Note:** The weather agent and workflow in `mastra/` appear to be a demo/template that was never wired into the main application

---

## 9. Improvement Worker (`workers/improvement-worker.ts`)

### 9.1 No blocking issues found
- **Status:** Fully implemented — collects conversations, reflects, updates memory/profiles, improves/creates skills, logs results
- **Note:** JSON parsing failures in reflection loop are silently skipped (expected for LLM outputs)

---

## 10. Document Worker (`workers/doc-worker.ts`)

### 10.1 No blocking issues found
- **Status:** Fully implemented — downloads, parses, chunks, embeds, stores, extracts metadata
- **Note:** Graceful error handling with status updates to DB

---

## 11. Cron Worker (`workers/cron-worker.ts`)

### 11.1 No blocking issues found
- **Status:** Fully implemented — loads skills, runs isolated agent, delivers to channel, logs execution
- **Note:** `channels.send` is called but will only work for `web` channel in practice

---

## 12. Configuration (`config/env.ts`)

### 12.1 No missing fields — all required env vars validated
- **Status:** Complete — all required fields have zod validation
- **Note:** Slack/Discord tokens are optional (they should be required if those channels are enabled)

---

## Full TODO/FIXME/STUB List

| # | Location | Line | Text |
|---|---|---|---|
| 1 | `channels/adapters.ts` | 20 | `// TODO: Initialize with @slack/web-api WebClient` |
| 2 | `channels/adapters.ts` | 24 | `// TODO: await this.client.chat.postMessage({ channel: ref, text: content })` |
| 3 | `channels/adapters.ts` | 29 | `// TODO: await this.client.chat.update({ channel: ref, ts: messageId, text: content })` |
| 4 | `channels/adapters.ts` | 39 | `// TODO: Verify Slack signing secret` |
| 5 | `channels/adapters.ts` | 53 | `// TODO: Initialize with discord.js Client` |
| 6 | `channels/adapters.ts` | 56 | `// TODO: const channel = await this.client.channels.fetch(ref)` |
| 7 | `channels/adapters.ts` | 62 | `// TODO: const message = await channel.messages.fetch(messageId)` |
| 8 | `channels/adapters.ts` | 69 | `// ─── Teams adapter (stub)` |
| 9 | `channels/adapters.ts` | 75 | `// TODO: Use Bot Framework SDK or direct REST API` |
| 10 | `channels/adapters.ts` | 80 | `// ─── WhatsApp adapter (stub)` |
| 11 | `channels/adapters.ts` | 86 | `// TODO: Use WhatsApp Business Cloud API` |
| 12 | `channels/adapters.ts` | 92 | `// ─── Email adapter (stub)` |
| 13 | `channels/adapters.ts` | 98 | `// TODO: Use Postmark, Mailgun, or raw SMTP` |
| 14 | `channels/manager.ts` | 53 | `// TODO: Parse Slack event payload into UnifiedMessage` |
| 15 | `channels/manager.ts` | 61 | `// TODO: Parse Discord interaction payload` |
| 16 | `channels/manager.ts` | 67 | `// TODO: Parse Teams Bot Framework activity` |
| 17 | `channels/manager.ts` | 73 | `// TODO: Parse WhatsApp webhook payload` |
| 18 | `server/webhooks/index.ts` | 85 | `// ─── Teams webhook (stub)` |
| 19 | `server/webhooks/index.ts` | 88 | `// TODO: Parse Bot Framework activity` |
| 20 | `server/webhooks/index.ts` | 94 | `// ─── WhatsApp webhook (stub)` |
| 21 | `server/webhooks/index.ts` | 103 | `// TODO: Parse WhatsApp Cloud API webhook` |
| 22 | `server/routes/auth.ts` | 69 | `// TODO: Send magic link email for production` |
| 23 | `server/routes/skills.ts` | 23 | `// Note: requires embedding column on skill_catalog — add via migration` |

**Total: 23 TODOs across 6 files**

---

## Recommendations (Priority Order)

### Immediate (High Priority)
1. **Implement Slack `verifyWebhook()`** — security issue; any sender can spoof Slack webhooks currently
2. **Implement `normalizeSlackEvent()` and `normalizeDiscordEvent()`** — without these, incoming webhooks from Slack/Discord cannot be processed into unified messages
3. **Initialize Slack `WebClient`** — needed for Slack to actually deliver outgoing messages

### Soon (Medium Priority)
4. **Add `embedding` column to `skill_catalog`** — enables true semantic search in the skills marketplace
5. **Implement Discord `DiscordClient`** — Discord channel cannot send/receive messages
6. **Implement Teams webhook parsing and adapter** — Teams is a declared channel
7. **Implement WhatsApp webhook parsing and adapter** — WhatsApp is a declared channel
8. **Implement magic link email** — move beyond MVP token-direct-return login

### Later (Low Priority)
9. **Implement Email adapter** — least critical channel
10. **Decide on Mastra dual-instance** — consolidate or clearly separate weather demo from main app
