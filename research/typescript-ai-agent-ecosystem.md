# TypeScript Open Source Ecosystem for Building a Claude Cowork-Style AI Agent Assistant

**Research Date:** March 25, 2026  
**Researcher:** Alize AI System  
**Scope:** Comprehensive analysis of TypeScript/JavaScript open source projects for building autonomous AI agents

---

## Executive Summary

The TypeScript AI agent ecosystem has matured significantly in 2024-2026. For building a Claude Cowork-style assistant (multi-step task execution, document management, tool use, memory, skills, subagents, cron/scheduling, multi-channel connectors), the landscape offers several strong options.

**Key Findings:**
- **OpenClaw** (336k stars) is the closest open-source project to Claude Cowork, with built-in multi-channel support, skills system, and agent runtime
- **LangChain.js + LangGraph.js** form the most comprehensive agent framework ecosystem with excellent TypeScript support
- **Dify** (75k+ stars) provides a production-ready workflow platform with RAG and agent capabilities
- **n8n** offers workflow automation with native LangChain integration
- **LlamaIndex.TS is DEPRECATED** — use Python version or LangChain instead
- French AI companies like **Mistral AI** and **Algolia** offer relevant infrastructure components

---

## 1. AI Agent Frameworks (TypeScript/JS)

### 1.1 LangChain.js + LangGraph.js
**GitHub:** https://github.com/langchain-ai/langchainjs | https://github.com/langchain-ai/langgraphjs  
**TypeScript Support:** ⭐⭐⭐⭐⭐ (First-class TypeScript, written in TypeScript)  
**Activity:** Highly active, commits within last week  
**Stars:** langchainjs ~5k+, langgraphjs growing rapidly

**Description:** The de facto standard for building LLM applications in JavaScript/TypeScript. LangChain provides components (models, prompts, memory, tools) while LangGraph enables complex agent workflows with cycles.

**Key Features:**
- Modular architecture with composable components
- First-class TypeScript support with full type safety
- 100+ integrations with vector stores, tools, and model providers
- LangGraph for multi-agent orchestration with cycles
- Streaming support, retries, and error handling built-in
- Supports Node.js 20+, Bun, Deno, Cloudflare Workers, Vercel

**Architecture Fit:** ✅ Excellent — LangGraph is purpose-built for agent orchestration similar to Claude Code's agent runtime. Provides state management, tool definitions, and reasoning loops.

**Install:**
```bash
npm install langchain @langchain/langgraph
```

---

### 1.2 OpenClaw
**GitHub:** https://github.com/openclaw/openclaw  
**TypeScript Support:** ⭐⭐⭐⭐⭐ (Written in TypeScript)  
**Activity:** Highly active, commits within days  
**Stars:** 336k ⭐ (massive community)  
**License:** MIT

**Description:** The closest project to Claude Cowork. OpenClaw is "your own personal AI assistant" that runs on your own devices. It provides multi-channel delivery, skills system, memory, cron/scheduling, and more.

**Key Features:**
- **Multi-channel inbox:** WhatsApp, Telegram, Slack, Discord, Signal, iMessage, IRC, Microsoft Teams, Matrix, and 15+ more
- **Multi-agent routing:** Route inbound channels to isolated agents
- **Skills platform:** Bundled, managed, and workspace skills with install gating
- **Live Canvas:** Agent-driven visual workspace
- **Voice Wake + Talk Mode:** Wake words on macOS/iOS, continuous voice on Android
- **Browser control:** OpenClaw-managed Chrome/Chromium with CDP
- **Cron + webhooks:** Time-triggered agent tasks
- **Nodes:** Camera snap/clip, screen record, location.get, notifications
- **macOS/iOS/Android companion apps**

**Architecture Fit:** ✅ **Perfect match** — This IS a Claude Cowork equivalent. Built-in channel connectors, skills registry (ClawHub), gateway control plane, session management.

---

### 1.3 CopilotKit
**GitHub:** https://github.com/CopilotKit/CopilotKit  
**TypeScript Support:** ⭐⭐⭐⭐⭐  
**Activity:** Active  
**Stars:** Growing rapidly  
**License:** MIT

**Description:** The Frontend Stack for Agents & Generative UI. React-focused SDK for building agentic applications with generative UI, shared state, and human-in-the-loop workflows.

**Key Features:**
- Chat UI with message streaming and tool calls
- Backend tool rendering with UI components
- **AG-UI Protocol:** Open protocol for agent-user interaction (adopted by Google, LangChain, AWS, Microsoft)
- Shared state synchronization between agents and UI
- Human-in-the-loop workflow pauses
- Integration with LangGraph, CrewAI

**Architecture Fit:** ✅ Excellent for building UI layers on top of agent frameworks

---

### 1.4 Dify
**GitHub:** https://github.com/langgenius/dify  
**TypeScript Support:** Partial (Python backend, TypeScript components)  
**Activity:** Very active, commits within days  
**Stars:** 75k+ ⭐  
**License:** Apache 2.0 with additional conditions

**Description:** Production-ready platform for agentic workflow development. Combines AI workflow, RAG pipeline, agent capabilities, model management, and observability.

**Key Features:**
- Visual workflow canvas
- Comprehensive model support (hundreds of LLMs)
- RAG pipeline with PDF, PPT support
- Agent capabilities (Function Calling, ReAct)
- 50+ built-in tools (Google Search, DALL·E, Stable Diffusion, WolframAlpha)
- LLMOps with logging and analytics
- Backend-as-a-Service with APIs
- Docker Compose deployment

**Architecture Fit:** ✅ Good for rapid prototyping, less flexible for custom agent architectures

---

### 1.5 n8n
**GitHub:** https://github.com/n8n-io/n8n  
**TypeScript Support:** ⭐⭐⭐⭐ (JavaScript with TypeScript support)  
**Activity:** Highly active  
**Stars:** 45k+ ⭐  
**License:** Sustainable Use License (fair-code)

**Description:** Fair-code workflow automation platform with native AI capabilities. Combines visual building with custom code, 400+ integrations.

**Key Features:**
- Visual workflow editor
- AI-native with LangChain integration
- JavaScript/Python code nodes
- 400+ integrations
- Self-hostable or cloud
- Enterprise features (SSO, permissions)

**Architecture Fit:** ✅ Good for workflow automation, less suited for autonomous agents

---

### 1.6 Botpress
**GitHub:** https://github.com/botpress/botpress  
**TypeScript Support:** ⭐⭐⭐⭐  
**Activity:** Active development  
**License:** MIT

**Description:** Platform for building chatbots and assistants powered by OpenAI. SDK-based development with cloud hosting option.

**Key Features:**
- TypeScript SDK (@botpress/sdk, @botpress/cli)
- Integration development platform
- Botpress Hub for community integrations
- Cloud and self-hosted options

**Architecture Fit:** ⚠️ More focused on chatbot-style interactions than autonomous agents

---

### 1.7 Weaviate (Vector Database + Agent Skills)
**GitHub:** https://github.com/weaviate/weaviate  
**TypeScript Support:** ⭐⭐⭐⭐⭐ (@weaviate/client)  
**Activity:** Active  
**Stars:** 30k+ ⭐  
**License:** BSD-3

**Description:** Open-source vector database written in Go. Combines object and vector storage with structured filtering.

**Key Features:**
- Semantic search at scale
- Hybrid search (vector + BM25 keyword)
- Integrated RAG & reranking
- Multi-tenancy, replication, RBAC
- TypeScript client with full feature parity
- Agent Skills repository for Claude Code, Cursor, Copilot

**Architecture Fit:** ✅ Excellent for memory/knowledge component

---

## 2. Skills/Multi-Agent Systems

### 2.1 LangChain Tools
**TypeScript Support:** ⭐⭐⭐⭐⭐  
**Repository:** Built into langchainjs

**Skills Architecture Pattern:**
```typescript
import { tool } from "langchain/core/tools";

const searchTool = tool(async ({ query }) => {
  // Tool implementation
  return searchResults;
}, {
  name: "search",
  description: "Search the web",
  schema: z.object({
    query: z.string().describe("The search query")
  })
});
```

**Key Tool Categories:**
- Web search (Brave, SerpAPI, DuckDuckGo)
- Shell/bash command execution
- File system operations
- API calls
- Calculator
- Code execution

---

### 2.2 OpenClaw Skills Platform (ClawHub)
**Registry:** https://clawhub.com  
**GitHub:** https://github.com/openclaw/clawhub  
**Stars:** 6.9k ⭐

**Description:** Minimal skill registry where agents can search and pull in skills automatically.

**Features:**
- Skill installation gating
- UI-based management
- Bundled, managed, and workspace skills
- Community-contributed skill repository

---

### 2.3 Weaviate Agent Skills
**GitHub:** https://github.com/weaviate/agent-skills

**Description:** Collection of skills for AI coding agents (Claude Code, Cursor, GitHub Copilot) to work with Weaviate.

**Install:**
```bash
npx skills add weaviate/agent-skills
```

---

## 3. Memory & Knowledge

### 3.1 Vector Database Clients in TypeScript

#### Weaviate (@weaviate/client)
- **TypeScript:** First-class support
- **Features:** Semantic search, hybrid search, RAG, reranking
- **Install:** `npm install @weaviate/client`

#### Qdrant
- **Client:** @qdrant/qdrant-js-client-rest or qdrant-client (gRPC)
- **TypeScript:** Full TypeScript support
- **Features:** Fast vector similarity search, filtering, payload storage

#### Chroma
- **TypeScript:** chromadb-js client available
- **Note:** Chroma is primarily Python-focused; JavaScript client has limited features

#### pgvector (PostgreSQL)
- **Client:** pg (node-postgres) with pgvector extension
- **TypeScript:** pg has TypeScript definitions
- **Features:** Vector similarity in SQL, native PostgreSQL

---

### 3.2 Embedding Models in TypeScript

#### @xenova/transformers
- **TypeScript:** JavaScript with TypeScript typings
- **Features:** Run transformer models in browser/Node.js
- **Note:** Useful for client-side embeddings

#### @llamaindex/openai (for embeddings)
- Integrates with OpenAI embeddings
- Compatible with LangChain.js

---

### 3.3 Memory Management Patterns

**LangGraph Checkpointing:**
```typescript
import { MemorySaver } from "@langchain/langgraph/checkpoint";

const checkpointer = new MemorySaver();
const graph = workflow.compile({ checkpointer });
```

**Session Memory:**
- Buffer memory (recent messages)
- Buffer window (sliding window)
- Summary memory (generates summaries)

---

## 4. Tool Use / Gateway Connectors

### 4.1 OpenClaw Channel Connectors (Built-in)
OpenClaw provides 20+ channel integrations out of the box:
- **Messaging:** WhatsApp, Telegram, Signal, iMessage, SMS
- **Team Chat:** Slack, Discord, Microsoft Teams, Google Chat, IRC, Matrix
- **Social:** Nostr, Twitch, LINE, Zalo
- **Productivity:** Feishu, Mattermost, Nextcloud Talk, Synology Chat
- **Web:** WebChat, macOS/iOS apps

**Architecture:** Each channel uses provider-specific libraries (Baileys for WhatsApp, grammY for Telegram, discord.js for Discord, Bolt for Slack)

---

### 4.2 Discord.js
**GitHub:** https://github.com/discordjs/discord.js  
**TypeScript:** ⭐⭐⭐⭐⭐ (First-class)  
**Stars:** 35k+ ⭐  
**License:** Apache-2.0

**Features:**
- Full Discord API coverage
- Voice support
- Rich embeds and components
- Excellent TypeScript definitions

---

### 4.3 Slack Bolt
**npm:** @slack/bolt  
**TypeScript:** ⭐⭐⭐⭐⭐ (First-class)  
**Maintainer:** Slack

**Features:**
- TypeScript-first
- Socket mode support
- OAuth flow support
- Workflows integration

---

### 4.4 Browser Control (Puppeteer/Playwright)

#### Puppeteer
- **npm:** puppeteer, @puppeteer/browsers
- **TypeScript:** ⭐⭐⭐⭐⭐
- Full Chrome/Chromium control via CDP

#### Playwright
- **npm:** playwright
- **TypeScript:** ⭐⭐⭐⭐⭐
- Multi-browser support (Chromium, Firefox, WebKit)

#### OpenClaw Browser Tool
- Dedicated Chrome/Chromium management
- CDP control with snapshots
- Profile management

---

### 4.5 Shell Command Execution

**Node.js child_process:**
- Built-in, full TypeScript support
- spawn, exec, fork modes
- Secure shell execution patterns needed

**OpenClaw exec tool:**
- Gateway-side execution
- Elevated bash access control
- Output capture and streaming

---

## 5. Cron / Scheduling

### 5.1 BullMQ (Redis-based)
**GitHub:** https://github.com/taskforcesh/bullmq  
**npm:** bullmq  
**TypeScript:** ⭐⭐⭐⭐⭐  
**Stars:** 14k+ ⭐  
**License:** MIT

**Description:** Premium message queue for Node.js with reliable, Redis-based background job processing.

**Features:**
- Delayed jobs
- Cron patterns
- Priority queues
- Rate limiting
- Retries with backoff
- Dashboard (BullBoard)
- Distributed locking

**Install:**
```bash
npm install bullmq ioredis
```

**Architecture Fit:** ✅ Excellent for time-triggered agent tasks with reliability

---

### 5.2 Agenda (MongoDB-based)
**GitHub:** https://github.com/agenda/agenda  
**npm:** agenda  
**TypeScript:** ⭐⭐⭐⭐ (Community types)  
**Stars:** 10k+ ⭐

**Features:**
- Cron-like scheduling
- MongoDB persistence
- Lightweight

---

### 5.3 Node-cron
**npm:** node-cron  
**TypeScript:** Basic types included

**Features:**
- Simple cron-like scheduling
- In-process (not distributed)

---

### 5.4 OpenClaw Cron (Built-in)
- Gateway-level cron jobs
- Webhook triggers
- Gmail Pub/Sub integration
- Wakeup scheduling

---

## 6. Self-Improvement / Learning

### 6.1 Error Recovery Patterns

**LangChain Error Handling:**
```typescript
const model = new ChatAnthropic({
  model: "claude-3-7-sonnet-latest",
  maxRetries: 3,
  retryDelay: 1000,
});
```

**Built-in retry policies:**
- Model call retries
- Tool execution retries
- Circuit breaker patterns

---

### 6.2 Observability & Evaluation

#### LangSmith
**Website:** https://smith.langchain.com  
**TypeScript:** @langchain/smith

**Features:**
- Agent tracing
- Evaluation frameworks
- Performance monitoring
- Production visibility

#### Langfuse
**GitHub:** https://github.com/langfuse/langfuse  
**TypeScript:** @langfuse/core, @langfuse/nextjs  
**License:** AGPL

**Features:**
- LLM observability
- Prompt management
- Cost tracking
- Self-hostable

#### OpenTelemetry + Helicone
- Helicone: OpenAI proxy with observability
- OpenTelemetry: Vendor-neutral tracing

---

### 6.3 Self-Correction Patterns

**ReAct (Reasoning + Acting):**
LangGraph supports ReAct agent pattern with built-in reasoning loops.

**Human-in-the-loop:**
- OpenClaw: Approval workflows
- CopilotKit: Pause and request input
- LangGraph: Interruption and approval checkpoints

---

## 7. Auth & Multi-Tenancy

### 7.1 Auth.js (NextAuth v5)
**GitHub:** https://github.com/nextauthjs/next-auth  
**npm:** next-auth, @auth/core  
**TypeScript:** ⭐⭐⭐⭐⭐  
**Stars:** 30k+ ⭐

**Features:**
- 40+ providers
- JWT and database sessions
- TypeScript-first
- Edge compatible
- Multi-tenancy support

**Install:**
```bash
npm install next-auth @auth/core
```

---

### 7.2 Lucia Auth
**GitHub:** https://github.com/lucia-auth/lucia  
**TypeScript:** ⭐⭐⭐⭐⭐  
**Stars:** 8k+ ⭐

**Features:**
- Lightweight
- Framework-agnostic
- Database-agnostic
- Full TypeScript

---

### 7.3 Keycloak
**GitHub:** https://github.com/keycloak/keycloak  
**TypeScript:** Via keycloak-js adapter

**Features:**
- Enterprise SSO
- SAML/OIDC
- User federation
- **Note:** Java-based, not TypeScript-native

---

### 7.4 OpenClaw Auth (Built-in)
- Gateway auth modes (password, Tailscale)
- Channel-specific authentication
- Session management
- Multi-agent workspace isolation

---

## 8. RAG & Document Processing

### 8.1 PDF Processing

#### pdf-parse (Node.js)
- **npm:** pdf-parse
- Extracts text from PDFs
- Basic TypeScript support

#### pdf.js (Mozilla)
- **npm:** pdfjs-dist
- Full PDF rendering
- TypeScript support
- Browser-focused

#### Mammoth (DOCX)
- **npm:** mammoth
- Converts DOCX to HTML/text
- TypeScript support
- Excellent for Word documents

#### @llamaindex/document (LangChain)
- Multi-format document loading
- PDF, PPTX, DOCX, Markdown

---

### 8.2 Text Extraction & Chunking

**LangChain Text Splitting:**
```typescript
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";

const splitter = RecursiveCharacterTextSplitter({
  chunkSize: 1000,
  chunkOverlap: 200,
});
```

**Strategies:**
- Recursive character splitting
- Token-based splitting
- Semantic chunking
- Markdown-aware splitting

---

### 8.3 Dify RAG Pipeline
- Out-of-box PDF, PPT extraction
- Visual RAG workflow builder
- Multiple retrieval strategies
- Re-ranking support

---

## 9. Complete Platforms / Clones

### 9.1 OpenClaw ✅ (PRIMARY RECOMMENDATION)
**GitHub:** https://github.com/openclaw/openclaw  
**Stars:** 336k ⭐  
**TypeScript:** First-class

**Closest to Claude Cowork:**
- Multi-channel messaging (20+ channels)
- Skills system with registry
- Multi-agent routing
- Cron/scheduling
- Browser control
- Memory/session management
- Voice capabilities
- Canvas/visual workspace
- Self-hosted

---

### 9.2 Dify
**GitHub:** https://github.com/langgenius/dify  
**Stars:** 75k+ ⭐  
**Language:** Python (backend) + TypeScript (web)

**Strengths:**
- Visual workflow builder
- Production-ready
- Comprehensive RAG
- Model management
- 50+ built-in tools

**Weaknesses:**
- Less flexible for custom architectures
- Python backend limits some deployments

---

### 9.3 n8n
**GitHub:** https://github.com/n8n-io/n8n  
**Stars:** 45k+ ⭐

**Strengths:**
- Visual workflow automation
- 400+ integrations
- AI-native platform
- Self-hostable

**Weaknesses:**
- More workflow-focused than autonomous agent

---

### 9.4 Botpress
**GitHub:** https://github.com/botpress/botpress  
**TypeScript:** SDK-first

**Strengths:**
- SDK-based development
- Cloud and self-hosted
- Integration marketplace

**Weaknesses:**
- Chatbot-focused, less autonomous agent
- Cloud-centric architecture

---

### 9.5 FastGPT
**Note:** Chinese project, limited English documentation  
**Type:** Knowledge base + AI agent platform

---

### 9.6 Coze (ByteDance)
**Note:** Commercial platform, not open source  
**Strengths:** Rich bot building platform  
**Weaknesses:** Not self-hostable, Chinese-focused

---

## 10. French AI / European Sovereignty

### 10.1 Mistral AI 🇫🇷
**Website:** https://mistral.ai  
**GitHub:** https://github.com/mistralai/mistral-src  
**Type:** AI company, not TypeScript framework

**Products:**
- Mistral models (Mistral 7B, Mixtral 8x7B)
- La Plateforme (API)
- Le Chat (assistant)

**Relevance:** Foundation model provider, European sovereignty option

---

### 10.2 Algolia 🇫🇷
**Website:** https://algolia.com  
**Type:** Search-as-a-service  
**GitHub:** (Enterprise SDKs)

**Relevance:**
- Fast vector search
- TypeScript client available
- Generous free tier
- European company (Paris-based)

**Install:**
```bash
npm install algoliasearch @algolia/client-search
```

---

### 10.3 Qdrant 🇨🇿
**Website:** https://qdrant.tech  
**GitHub:** https://github.com/qdrant/qdrant  
**Type:** Vector search engine (Czechia)

**Relevance:**
- Open-source vector database
- Excellent TypeScript client
- Self-hostable
- High performance

---

### 10.4 LangGenius (Dify)
**Note:** Chinese company, not French  
**Relevance:** Produces Dify platform

---

## Architecture Recommendations

### Recommended Stack for Claude Cowork-Style Assistant

**Core Agent Runtime:**
1. **OpenClaw** (if you want 80% built-in functionality)
2. **LangGraph.js** (if you need full customization)

**Tool Integration Layer:**
- LangChain.js tools ecosystem
- MCP (Model Context Protocol) for extensibility

**Memory/Knowledge:**
- **Weaviate** or **Qdrant** for vector search
- **pgvector** if you prefer PostgreSQL-only stack
- Session memory via LangGraph checkpointing

**Scheduling:**
- **BullMQ** for reliable job queues
- Built-in cron (OpenClaw)

**Channel Connectors:**
- OpenClaw built-ins (20+ channels)
- Or build custom with discord.js, Slack Bolt

**Document Processing:**
- **pdf-parse** + **Mammoth** for document extraction
- LangChain document loaders

**Auth:**
- **Auth.js** for application auth
- OpenClaw built-in for gateway access

**Observability:**
- **LangSmith** (if using LangChain)
- **Langfuse** (self-hosted alternative)

---

### Alternative Stacks

**Stack A: Full OpenClaw**
```
OpenClaw (everything built-in)
+ ClawHub skills
+ Weaviate (knowledge)
+ BullMQ (jobs)
```

**Stack B: Modular LangChain**
```
LangGraph.js (orchestration)
+ LangChain.js (tools)
+ Weaviate (memory)
+ BullMQ (scheduling)
+ discord.js/Slack Bolt (channels)
+ Auth.js (auth)
```

**Stack C: Dify-based**
```
Dify (workflows + agents)
+ Dify RAG pipeline
+ External vector DB (Weaviate)
+ Channel integrations
```

---

## Projects to Avoid

### ❌ LlamaIndex.TS
**Status:** **DEPRECATED** — No longer maintained

The official TypeScript port of LlamaIndex is deprecated. Use:
- Python LlamaIndex for complex RAG
- LangChain.js for TypeScript alternatives

---

### ⚠️ AutoGen (Microsoft)
**Status:** Primarily Python, TypeScript support limited

Microsoft AutoGen is Python-first with experimental TypeScript. If you need Microsoft ecosystem integration, consider:
- LangChain.js with LangGraph
- Building custom multi-agent with LangGraph

---

### ⚠️ CrewAI
**Status:** Python-focused

CrewAI is primarily Python with limited TypeScript support. Use LangGraph for multi-agent patterns instead.

---

### ⚠️ Legacy Botpress v12
**Note:** Botpress Cloud is the current focus. On-premise v12 is legacy.

---

## Key Findings Summary

| Category | Best Option | Alternative |
|----------|-----------|------------|
| **Complete Platform** | OpenClaw (336k⭐) | Dify (75k⭐), n8n (45k⭐) |
| **Agent Framework** | LangGraph.js + LangChain.js | CopilotKit (frontend) |
| **Vector DB** | Weaviate (30k⭐) | Qdrant, pgvector |
| **Multi-Channel** | OpenClaw built-in | discord.js, Slack Bolt |
| **Scheduling** | BullMQ | Agenda, node-cron |
| **Document RAG** | LangChain loaders | pdf-parse + Mammoth |
| **Auth** | Auth.js | Lucia, Keycloak |
| **Observability** | LangSmith | Langfuse |
| **Skills Platform** | ClawHub (OpenClaw) | Custom LangChain tools |

---

## Technology Stack Recommendations

### For Maximum Claude Cowork Parity
**Use OpenClaw as the foundation** — it provides:
- ✅ Multi-channel messaging (WhatsApp, Slack, Discord, etc.)
- ✅ Skills system with registry
- ✅ Multi-agent routing
- ✅ Cron/scheduling
- ✅ Browser control
- ✅ Memory/sessions
- ✅ Voice (wake words, talk mode)
- ✅ Canvas/visual workspace
- ✅ Self-hosted control plane

### For Custom Agent Architecture
**Use LangChain.js + LangGraph.js:**
- Full control over agent behavior
- Custom tool definitions
- Complex multi-agent workflows
- State management with checkpointing
- Combine with Weaviate, BullMQ, discord.js

---

## Conclusion

The TypeScript AI agent ecosystem is mature and comprehensive. **OpenClaw** is the standout project for building Claude Cowork-style assistants, with 336k GitHub stars and a massive feature set. For more custom architectures, **LangChain.js + LangGraph.js** provides the most flexible and well-supported framework.

The ecosystem has strong TypeScript support throughout, with excellent clients for vector databases (Weaviate, Qdrant), scheduling (BullMQ), and channel integrations (discord.js, Slack Bolt). Avoid **LlamaIndex.TS** (deprecated) and be aware that **AutoGen** and **CrewAI** are primarily Python projects.

---

*Report generated by Alize Research System*  
*Last Updated: March 25, 2026*
