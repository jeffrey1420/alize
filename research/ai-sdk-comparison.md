# AI SDK/ADK Comparison for Multi-Tenant Agent SaaS Platform

**Research Date:** March 25, 2026  
**Researcher Context:** Louis is building a Claude Cowork-style AI agent SaaS platform  
**Framework:** TypeScript/Node.js preferred

---

## Executive Summary

Building a multi-tenant AI agent SaaS platform (teams/organizations, not per-user deployments) with document management, RAG, skills system, project-scoped agents, multi-level memory, self-improvement, cron/scheduling, and multi-channel connectors requires a framework that is **production-grade, actively maintained, and architecturally flexible**.

### 🏆 Recommended Stack

**Primary Recommendation: Mastra + Custom Multi-Tenant Layer**

Mastra is the strongest TypeScript-native framework with 22k+ GitHub stars, a mature ecosystem, built-in memory/RAG/workflows, and an active YC-backed team. However, Mastra lacks native multi-tenant isolation out of the box — this must be implemented as an application layer.

**Secondary Recommendation: LangGraph.js + LangGraph Platform**

LangGraph.js is the most mature orchestration framework, used in production by Klarna, Uber, LinkedIn, GitLab. It has native checkpointing, multi-agent patterns, and a purpose-built deployment platform. Best for complex graph-based workflows.

**Best for Rapid Multi-Channel Development: OpenAI Agents SDK**

The OpenAI Agents SDK is lightweight, well-documented, and has excellent voice/realtime support. Best if your core is OpenAI API and you want minimal abstractions.

---

## Framework Overview

| Framework | GitHub Stars | Forks | Contributors | Last Commit | Created | Maturity |
|-----------|-------------|-------|-------------|-------------|---------|----------|
| **Mastra** | 22,321 ⭐ | 1,791 | 100 | Mar 25, 2026 | Aug 2024 | 🟢 Very High |
| **VoltAgent** | 6,977 ⭐ | 696 | 61 | Mar 25, 2026 | Apr 2025 | 🟢 High |
| **LangGraph.js** | 2,702 ⭐ | 432 | 100 | Mar 24, 2026 | Jan 2024 | 🟢 Very High |
| **OpenAI Agents SDK** | 2,530 ⭐ | 657 | 81 | Mar 25, 2026 | May 2025 | 🟡 Medium |
| **Google ADK** | 929 ⭐ | 115 | 28 | Mar 23, 2026 | Aug 2025 | 🟡 Medium |
| **Network-AI** | 29 ⭐ | 10 | 3 | Mar 23, 2026 | Feb 2026 | 🔴 Very Low |

> **Note:** Network-AI (29 stars, 3 contributors) is too early-stage for production consideration. Its 17 adapters are interesting but the project is too nascent.

---

## 1. OpenAI Agents SDK (`openai/openai-agents-js`)

**GitHub:** https://github.com/openai/openai-agents-js  
**Docs:** https://openai.github.io/openai-agents-js/

### Quick Stats
- **Stars:** 2,530 | **Forks:** 657 | **Contributors:** 81
- **Last Commit:** March 25, 2026 | **Created:** May 31, 2025
- **License:** Apache 2.0
- **Type:** Provider-agnostic (supports OpenAI API and compatible providers)

### Feature Analysis

| Requirement | Status | Details |
|------------|--------|---------|
| **Tools/Tool Use** | ✅ Full | Zod-powered function tools, automatic schema generation. Built-in MCP server tool calling. |
| **Cron/Scheduling** | ❌ None | No built-in scheduling. Must use external (BullMQ, cron jobs, etc.) |
| **Skills System** | 🟡 Partial | No native "installable skills" concept. Tools serve this purpose but no composable skill registry |
| **Subagents** | ✅ Full | Handoffs allow agents to delegate to other agents. `handoffs: [otherAgent]` |
| **Memory** | ✅ Sessions | Built-in `Session` class for conversation history management. `run(agent, input, { sessionId })` |
| **Self-Improvement** | ❌ None | No built-in error recovery, learning from feedback, or self-modification |
| **Multi-Tenant** | ❌ None | No org/team concepts. Sessions must be managed externally per tenant |
| **Multi-Channel** | 🟡 Partial | Realtime voice agents built-in. Webhook/websocket interface. No pre-built Slack/Teams/WhatsApp connectors |
| **RAG** | ❌ None | No built-in document ingestion or retrieval. Must be implemented as tools |
| **Deployment** | ✅ Flexible | Node.js 22+, Deno, Bun. Cloudflare Workers with compat flag |

### Key Code Patterns

```typescript
import { Agent, run, handoff } from '@openai/agents';

// Define agents with handoffs
const triageAgent = new Agent({
  name: 'Triage',
  instructions: 'Route to the right specialist',
  handoffs: [billingAgent, techSupportAgent, salesAgent],
});

const result = await run(triageAgent, userMessage, {
  sessionId: 'tenant-123-session-456',
});
```

```typescript
// Guardrails
import { Guardrail } from '@openai/agents';

const guardrail = new Guardrail({
  name: 'content-filter',
  validate: async (input) => {
    // Return { passed: true } or { passed: false, reason: '...' }
  },
});
```

### Strengths
- Excellent documentation and OpenAI backing
- Built-in tracing (OpenAI's observability suite)
- Realtime voice agent support with interruption detection
- Very low learning curve — few primitives, easy to understand
- Guardrails are first-class citizens

### Weaknesses / Gotchas
- **No multi-tenant abstraction** — you must build all org/team isolation yourself
- **No RAG** — must implement document retrieval as custom tools
- **No scheduling** — completely external
- **No skills system** — no concept of installable, composable skill packages
- **No self-improvement** — no error recovery, no learning
- **Session storage is basic** — just conversation history, no semantic memory
- Provider-locked to OpenAI ecosystem (though provider-agnostic in theory)
- `RECOMMENDED_PROMPT_PREFIX` can feel fragile (community feedback)

### Production Readiness: 🟡🟡⚪ (6/10)
Good for single-tenant or when you need OpenAI-optimized tracing. Not sufficient for multi-tenant SaaS without heavy custom work.

---

## 2. Google ADK (`google/adk-js`)

**GitHub:** https://github.com/google/adk-js  
**Docs:** https://google.github.io/adk-js/

### Quick Stats
- **Stars:** 929 | **Forks:** 115 | **Contributors:** 28
- **Last Commit:** March 23, 2026 | **Created:** August 14, 2025
- **License:** Apache 2.0
- **Type:** TypeScript toolkit for building AI agents

### Feature Analysis

| Requirement | Status | Details |
|------------|--------|---------|
| **Tools/Tool Use** | ✅ Full | Rich tool ecosystem: pre-built Google tools, custom functions, OpenAPI specs |
| **Cron/Scheduling** | ❌ None | No built-in scheduling mentioned |
| **Skills System** | ❌ None | No installable skills concept |
| **Subagents** | ✅ Full | Modular multi-agent systems via hierarchical composition |
| **Memory** | 🟡 Partial | Session-based memory, integration with Google Cloud storage |
| **Self-Improvement** | ❌ None | No built-in error recovery or learning |
| **Multi-Tenant** | 🟡 Via GCP | No native multi-tenant concepts, but GCP integration provides some isolation |
| **Multi-Channel** | ❌ None | No pre-built connectors for Slack/Teams/WhatsApp |
| **RAG** | 🟡 Via Extensions | Tool-based retrieval, but no first-party RAG service |
| **Deployment** | ✅ GCP-optimized | Tight Google Cloud integration, but runs anywhere |

### Key Code Patterns

```typescript
import { LlmAgent, GOOGLE_SEARCH } from '@google/adk';

const rootAgent = new LlmAgent({
  name: 'search_assistant',
  description: 'An assistant that can search the web.',
  model: 'gemini-2.5-flash',
  instruction: 'You are a helpful assistant. Answer user questions using Google Search when needed.',
  tools: [GOOGLE_SEARCH],
});
```

### Strengths
- Google backing and Gemini integration
- A2A protocol integration for agent-to-agent communication
- Code-first TypeScript approach
- Development UI similar to Python ADK

### Weaknesses / Gotchas
- **Very new** (August 2025) — still in early development
- **Small contributor base** (28 contributors) — ecosystem is nascent
- **Gemini-only** — tight coupling to Google's models limits provider flexibility
- **No multi-tenant abstractions** — must build your own
- **No scheduling, no RAG, no skills** — all must be custom
- **Pre-GA** — explicitly marked as "Pre-GA Offerings Terms"

### Production Readiness: 🟡⚪⚪⚪ (4/10)
Too early-stage. Good if you're fully invested in Google Cloud/Gemini ecosystem. Otherwise not recommended for a multi-tenant SaaS.

---

## 3. VoltAgent (`VoltAgent/voltagent`)

**GitHub:** https://github.com/VoltAgent/voltagent  
**Docs:** https://voltagent.dev

### Quick Stats
- **Stars:** 6,977 ⭐ | **Forks:** 696 | **Contributors:** 61
- **Last Commit:** March 25, 2026 | **Created:** April 16, 2025
- **License:** MIT
- **Type:** Full AI Agent Engineering Platform (Open Source Framework + VoltOps Cloud)

### Feature Analysis

| Requirement | Status | Details |
|------------|--------|---------|
| **Tools/Tool Use** | ✅ Full | Zod-typed tools with lifecycle hooks and cancellation. MCP integration |
| **Cron/Scheduling** | ❌ External | No built-in cron. Workflow engine supports long-running tasks but not scheduling |
| **Skills System** | ❌ None | No native installable/composable skills registry |
| **Subagents** | ✅ Full | Supervisors & Sub-Agents with task routing |
| **Memory** | ✅ Strong | Durable memory adapters (LibSQL, etc.). Per-agent persistent memory |
| **Self-Improvement** | 🟡 Partial | Guardrails for safety, evals for measurement, but no automatic self-improvement |
| **Multi-Tenant** | 🟡 Partial | Supervisor runtime can route per-tenant, but no native org/workspace abstraction |
| **Multi-Channel** | 🟡 Via Cloud | VoltOps Cloud has some connectors, but connectors not documented for OSS |
| **RAG** | ✅ Built-in | Retrieval agents + managed RAG service (VoltAgent Knowledge Base) |
| **Deployment** | ✅ Strong | Hono server, can deploy anywhere. VoltOps Cloud for observability |

### Key Code Patterns

```typescript
import { VoltAgent, Agent, Memory } from "@voltagent/core";
import { LibSQLMemoryAdapter } from "@voltagent/libsql";
import { honoServer } from "@voltagent/server-hono";

// Persistent memory per agent
const memory = new Memory({
  storage: new LibSQLMemoryAdapter({ url: "file:./.voltagent/memory.db" }),
});

const agent = new Agent({
  name: "my-agent",
  instructions: "A helpful assistant",
  model: openai("gpt-4o-mini"),
  tools: [weatherTool],
  memory,  // <-- Durable memory
});

// Multi-agent with supervisor
new VoltAgent({
  agents: { agent, specializedAgent },
  workflows: { expenseApprovalWorkflow },
  server: honoServer(),
});
```

```typescript
// Workflow with human-in-the-loop suspend/resume
const expenseApprovalWorkflow = createWorkflowChain({
  id: "expense-approval",
  input: z.object({ employeeId: z.string(), amount: z.number() }),
  result: z.object({ status: z.enum(["approved", "rejected"]) }),
})
.andThen({
  id: "check-approval-needed",
  resumeSchema: z.object({ approved: z.boolean(), managerId: z.string() }),
  execute: async ({ data, suspend, resumeData }) => {
    if (data.amount > 500) {
      await suspend("Manager approval required", { employeeId: data.employeeId });
    }
    return { ...data, approved: true };
  },
});
```

### Strengths
- **Best OSS TypeScript-native RAG story** — managed RAG service included
- **Supervisor/Subagent architecture** is exactly what Louis needs for project-scoped agents
- **Memory adapters** are first-class and durable
- **Voice support** with OpenAI/ElevenLabs
- **MCP integration** without glue code
- **VoltOps Cloud** provides observability, evals, deployment — optional managed layer
- 7k stars in ~11 months — rapid community adoption

### Weaknesses / Gotchas
- **No native multi-tenant abstraction** — must implement org/team isolation yourself
- **No skills system** — no installable skill packages
- **No built-in scheduling** — must use external
- **No self-improvement** — evals exist for measurement but not automatic learning
- **VoltOps Cloud is proprietary** — some enterprise features may require paid tier
- Skills and multi-channel connectors are the biggest gaps for Louis's use case

### Production Readiness: 🟡🟡🟡🟡 (7/10)
Excellent TypeScript-native framework with strong memory, RAG, and supervisor patterns. Missing multi-tenant and skills system but these are buildable on top. Very active development.

---

## 4. LangGraph.js + LangGraph Platform (`langchain-ai/langgraphjs`)

**GitHub:** https://github.com/langchain-ai/langgraphjs  
**Docs:** https://langchain.com/langgraph

### Quick Stats
- **Stars:** 2,702 ⭐ | **Forks:** 432 | **Contributors:** 100
- **Last Commit:** March 24, 2026 | **Created:** January 9, 2024
- **License:** MIT
- **Type:** Low-level orchestration framework for building controllable agents as graphs

### Feature Analysis

| Requirement | Status | Details |
|------------|--------|---------|
| **Tools/Tool Use** | ✅ Full | `tool` decorator, Zod schemas, extensive LangChain integrations |
| **Cron/Scheduling** | ✅ Via Platform | LangGraph Platform includes cron jobs API |
| **Skills System** | 🟡 LangChain Hub | LangChain Hub for prompt/templates, but not "installable skills" |
| **Subagents** | ✅ Full | Multi-agent patterns via subgraphs, hierarchical agents |
| **Memory** | ✅ Full | Checkpointing for state persistence, thread-based isolation |
| **Self-Improvement** | ✅ Via LangSmith | Evals in LangSmith can drive improvements |
| **Multi-Tenant** | 🟡 Threads | Thread-based isolation provides logical tenant separation |
| **Multi-Channel** | 🟡 External | LangChain expression language can wire up webhooks, but no pre-built connectors |
| **RAG** | ✅ Via LangChain | Full LangChain retrieval integrations (vectorstores, reranking) |
| **Deployment** | ✅ Platform | LangGraph Platform for deployment, scaling, cron jobs |

### Key Code Patterns

```typescript
import { createReactAgent, tool } from "langchain";
import { ChatAnthropic } from "@langchain/anthropic";
import { MemorySaver } from "@langchain/langgraph/saveable";
import { z } from "zod";

const search = tool(async ({ query }) => {
  return "Weather data...";
}, {
  name: "search",
  description: "Search the web",
  schema: z.object({ query: z.string() }),
});

const model = new ChatAnthropic({ model: "claude-3-7-sonnet-latest" });

// Checkpointing for memory persistence
const memory = new MemorySaver();

const agent = createReactAgent({
  llm: model,
  tools: [search],
  checkpointableMemory: memory,
});
```

```typescript
// Multi-agent as subgraph
const superGraph = new StateGraph({ /* ... */ })
  .add_node("supervisor", supervisorAgent)
  .add_node("researcher", researcherAgent)
  .add_node("writer", writerAgent)
  .add_edge("researcher", "writer")
  .add_edge("writer", END)
  .compile();
```

### Production Users
- **Klarna** — Customer support bot for 85M active users
- **Elastic** — Security AI assistant for threat detection
- **Uber** — Automated unit test generation
- **Replit** — Code generation
- **LinkedIn, GitLab** — Various internal agents

### Strengths
- **Most mature orchestration framework** — graph-based gives you total control
- **First-class checkpointing** — state persists across runs natively
- **Human-in-the-loop** with `interrupt` and `updateState`
- **LangGraph Platform** includes cron jobs, memory threads, deployment APIs
- **LangSmith** for full-stack observability and evals
- **Multi-agent patterns** well-documented and battle-tested
- LangChain integration provides RAG, tools, and 100+ integrations

### Weaknesses / Gotchas
- **Steep learning curve** — graph-based programming model is very different from imperative code
- **No native "skills system"** — LangChain Hub exists but isn't the same as Claude Cowork-style installable skills
- **Multi-tenant via threads** — works but requires careful implementation
- **TypeScript vs Python** — LangGraph.js is less mature than the Python version
- **No pre-built multi-channel connectors** — must wire up Slack/Teams yourself
- **Complexity** — for Louis's use case, might be overkill for what he needs
- **123 open issues** — higher issue count than peers (may indicate complexity/debugging challenges)

### Production Readiness: 🟡🟡🟡🟡 (8/10)
Most production-proven framework with major enterprise users. Graph model is powerful but has a real learning curve. LangGraph Platform addresses deployment/cron/multi-tenant at a platform level.

---

## 5. Mastra (`mastra-ai/mastra`)

**GitHub:** https://github.com/mastra-ai/mastra  
**Docs:** https://mastra.ai/docs

### Quick Stats
- **Stars:** 22,321 ⭐ | **Forks:** 1,791 | **Contributors:** 100
- **Last Commit:** March 25, 2026 | **Created:** August 6, 2024
- **License:** Apache 2.0 (core) + Enterprise License (some features)
- **Type:** All-in-one TypeScript framework for AI agents and applications

### Feature Analysis

| Requirement | Status | Details |
|------------|--------|---------|
| **Tools/Tool Use** | ✅ Full | `createTool` with Zod schemas, 40+ provider integrations |
| **Cron/Scheduling** | ✅ Via Workflows | Workflows can be triggered by webhooks; scheduling via external or Mastra Cloud |
| **Skills System** | 🟡 MCP-based | Skills via MCP servers; no native "installable skills" concept |
| **Subagents** | ✅ Via Tools | Agents can call other agents as tools; no native subagent spawning |
| **Memory** | ✅ Full | Short-term + long-term memory with storage backends (libSQL, Postgres) |
| **Self-Improvement** | 🟡 Via Evals | Evals for measurement; no automatic self-improvement |
| **Multi-Tenant** | 🟡 Workspaces | Workspaces provide some isolation (changelog Feb 2026 mentions RBAC) |
| **Multi-Channel** | 🟡 Templates | Slack agent template exists; no first-party multi-channel connector library |
| **RAG** | ✅ Built-in | RAG workflows with vectorstores |
| **Deployment** | ✅ Strong | React/Next.js/Node integration, standalone server, Vercel, etc. |

### Key Code Patterns

```typescript
import { Agent } from "@mastra/core/agent";
import { openai } from "@ai-sdk/openai";
import { createTool } from "@mastra/core/tools";

// Tool definition
const githubRepoTool = createTool({
  id: "get-github-repo-info",
  description: "Fetch basic insights for a public GitHub repository",
  inputSchema: z.object({ owner: z.string(), repo: z.string() }),
  outputSchema: z.object({ stars: z.number(), forks: z.number() }),
  execute: async ({ context }) => getRepo(context.owner, context.repo),
});

// Agent definition
export const githubAgent = new Agent({
  name: "GitHub Insights Agent",
  instructions: "You analyze GitHub repos...",
  model: openai("gpt-4o-mini"),
  tools: { githubRepoTool },
});

// Memory integration
const agent = new Agent({
  // ...
  memory: new Memory({ storage: new LibSQLMemoryAdapter({ url: "./memory.db" }) }),
});
```

```typescript
// Mastra class wiring
import { Mastra } from "@mastra/core";

export const mastra = new Mastra({
  agents: { githubAgent },
});

// Cron-like via workflows
const workflow = createWorkflow({
  id: "daily-digest",
  trigger: { schedule: "0 9 * * *" }, // cron syntax
  steps: [/* steps */],
});
```

### Strengths
- **Largest TypeScript-native community** — 22k stars, 300k weekly npm downloads
- **YC W25 backed** — $13M funding, active commercial entity
- **Model routing** — 40+ providers through one interface (OpenAI, Anthropic, Gemini, etc.)
- **Mastra Studio** — built-in local IDE for testing agents
- **Observability built-in** — tracing, logging, evals
- **Human-in-the-loop** — suspend/resume workflows
- **MCP server authoring** — create and publish MCP servers
- **Fastest growing** in the TypeScript agent space

### Weaknesses / Gotchas
- **No native multi-tenant** — Workspaces exist but isolation must be designed carefully
- **No skills system** — MCP-based skills but not the installable/composable package system Louis wants
- **Self-improvement is measurement-only** — evals measure but don't auto-improve
- **Multi-channel is template-level** — Slack agent exists as template, not a managed connector library
- **Enterprise features** in separate `ee/` directory with Enterprise License
- **Younger than LangGraph** — less battle-tested at extreme scale

### Production Readiness: 🟡🟡🟡🟡 (8/10)
Most promising TypeScript-native framework with strong community momentum. Covers most requirements but multi-tenant and skills system are application-layer concerns, not framework-provided.

---

## Feature Comparison Matrix

| Feature | OpenAI Agents SDK | Google ADK | VoltAgent | LangGraph.js | Mastra |
|---------|:-----------------:|:----------:|:---------:|:------------:|:------:|
| **Multi-Agent Orchestration** | ✅ Handoffs | ✅ Hierarchical | ✅ Supervisors | ✅ Subgraphs | ✅ Tools as agents |
| **Subagents/Child Agents** | ✅ Handoffs | ✅ | ✅ Supervisor/Sub | ✅ Graph nodes | 🟡 Via tools |
| **Tools/Tool Use** | ✅ Zod | ✅ GCP + Custom | ✅ Zod + MCP | ✅ LangChain | ✅ Zod + MCP |
| **Memory** | ✅ Sessions | 🟡 Cloud Storage | ✅ Adapters | ✅ Checkpointing | ✅ Multi-level |
| **RAG** | ❌ | 🟡 Via tools | ✅ Built-in | ✅ LangChain | ✅ Built-in |
| **Skills System** | ❌ | ❌ | ❌ | 🟡 Hub | 🟡 MCP-based |
| **Self-Improvement** | ❌ | ❌ | 🟡 Evals | ✅ LangSmith | 🟡 Evals |
| **Cron/Scheduling** | ❌ | ❌ | ❌ | ✅ Platform | ✅ Workflows |
| **Multi-Tenant** | ❌ | 🟡 GCP | 🟡 Manual | 🟡 Threads | 🟡 Workspaces |
| **Multi-Channel** | 🟡 Voice only | ❌ | 🟡 VoltOps | ❌ | 🟡 Templates |
| **Guardrails** | ✅ First-class | ✅ | ✅ | ✅ | ✅ |
| **Streaming** | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Observability** | ✅ OpenAI tracing | ✅ GCP tracing | ✅ VoltOps | ✅ LangSmith | ✅ Built-in |
| **Deployment** | ✅ Anywhere | ✅ GCP | ✅ Anywhere | ✅ Platform | ✅ Anywhere |
| **MCP Support** | ✅ | ❌ | ✅ | ✅ | ✅ |
| **TypeScript-Native** | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## MCP (Model Context Protocol) Integration

| Framework | MCP Support | Details |
|-----------|:-----------:|---------|
| **OpenAI Agents SDK** | ✅ | Built-in MCP server tool calling |
| **Google ADK** | ❌ | No MCP support documented |
| **VoltAgent** | ✅ | Native MCP support, `@voltagent/mcp-docs-server` provided |
| **LangGraph.js** | ✅ | Via LangChain MCP integrations |
| **Mastra** | ✅ | MCP server authoring + client consumption |

**MCP fits into each framework as:**
- **Tool source** — agents use MCP servers as tool providers
- **Skill delivery** — MCP servers can expose installable capabilities
- **Cross-framework** — Network-AI's MCP adapter shows MCP as a universal bridge

---

## Observability & Debugging

| Framework | Built-in Tracing | External Integration | Dev UI |
|-----------|:----------------:|:-------------------:|:------:|
| **OpenAI Agents SDK** | ✅ OpenAI tracing | OpenAI evals/fine-tuning | ❌ |
| **Google ADK** | ✅ GCP tracing | Cloud Monitoring | ✅ Dev UI |
| **VoltAgent** | ✅ VoltOps Cloud | Pino logger | ✅ VoltOps Console |
| **LangGraph.js** | ✅ LangSmith | Datadog, etc. | ✅ LangGraph Studio |
| **Mastra** | ✅ Built-in + Mastra Studio | Datadog, Pino | ✅ Mastra Studio |

**Best observability story:** LangGraph.js + LangSmith (enterprise-grade)  
**Best for TypeScript-native:** Mastra (built-in, no external service needed)  
**Best for OpenAI ecosystem:** OpenAI Agents SDK

---

## Production Readiness Rating (2026)

| Framework | Rating | Rationale |
|-----------|:------:|-----------|
| **LangGraph.js** | 8.5/10 | Most production-proven, major enterprise users, complete platform |
| **Mastra** | 8/10 | Fastest growing, YC-backed, strong community, slightly younger |
| **VoltAgent** | 7/10 | Excellent features, strong TypeScript story, needs multi-tenant layer |
| **OpenAI Agents SDK** | 6/10 | Good for single-tenant, missing critical SaaS features |
| **Google ADK** | 4/10 | Too early, Gemini-only, pre-GA |
| **Network-AI** | 2/10 | Too nascent, single-digit contributors |

---

## Migration & Learning Curve Assessment

| Framework | Learning Curve | Time to First Agent | Multi-Tenant Impl. Effort |
|-----------|:--------------:|:------------------:|:------------------------:|
| **OpenAI Agents SDK** | Low | 30 min | High (build from scratch) |
| **Google ADK** | Medium | 1 hour | High (no abstractions) |
| **VoltAgent** | Low-Medium | 30 min | Medium (supervisor routing helps) |
| **LangGraph.js** | High | 2-4 hours | Medium (threads, but complex model) |
| **Mastra** | Low | 30 min | Medium (workspaces exist) |

---

## Recommendations for Louis's Specific Requirements

### Must Have (Critical)

| Requirement | Best Framework | Approach |
|-------------|:--------------:|----------|
| Multi-tenant (teams/orgs) | LangGraph.js or Mastra | App-layer: thread/workspace per tenant |
| Document management + RAG | VoltAgent or Mastra | Built-in RAG service |
| Project-scoped agents | VoltAgent | Supervisor + subagents architecture |
| Memory (user/session/agent) | LangGraph.js | Checkpointing + memory threads |
| TypeScript/Node.js | All | N/A |

### Should Have (Important)

| Requirement | Best Framework | Approach |
|-------------|:--------------:|----------|
| Skills system | None natively | Build on MCP or tool registry |
| Self-improvement | LangGraph.js | LangSmith evals + feedback loop |
| Cron/scheduling | LangGraph.js Platform | Native cron jobs API |
| Multi-channel connectors | None | Build on webhook interfaces |

### Nice to Have

| Requirement | Best Framework | Approach |
|-------------|:--------------:|----------|
| Voice support | OpenAI Agents SDK | Realtime voice API |
| Lowest learning curve | Mastra | Single command scaffold |

---

## Code Pattern Examples

### Multi-Tenant Agent (Conceptual)

```typescript
// Using Mastra with multi-tenant workspace isolation
import { Mastra } from "@mastra/core";
import { Agent, Memory } from "@mastra/core";

const createTenantAgent = (tenantId: string) => {
  const tenantMemory = new Memory({
    storage: new LibSQLMemoryAdapter({ url: `./memory-${tenantId}.db` }),
  });
  
  return new Agent({
    name: `agent-${tenantId}`,
    instructions: `You are an agent for tenant: ${tenantId}`,
    model: openai("gpt-4o"),
    tools: [/* tenant-specific tools */],
    memory: tenantMemory,
  });
};

const mastra = new Mastra({
  agents: Object.fromEntries(
    tenants.map(t => [t.id, createTenantAgent(t.id)])
  ),
});
```

### Supervisor + Subagents (VoltAgent)

```typescript
// Project-scoped agents with supervisor routing
const projectSupervisor = new Agent({
  name: "project-supervisor",
  instructions: "Route tasks to the right project specialist",
  model: openai("gpt-4o"),
  handoffs: [codeAgent, researchAgent, writerAgent],
});

const teamSupervisor = new Agent({
  name: "team-supervisor", 
  instructions: "Route to the right project supervisor",
  model: openai("gpt-4o"),
  handoffs: [projectSupervisor],
});
```

### RAG with Memory (LangGraph.js)

```typescript
// RAG + memory + checkpointing
const agent = createReactAgent({
  llm: new ChatAnthropic({ model: "claude-3-7-sonnet-latest" }),
  tools: [retrieverTool, searchTool],
  checkpointableMemory: new MemorySaver(),
});

// Thread per user for isolation
const threadConfig = { configurable: { thread_id: `tenant-123-user-456` } };
const result = await agent.invoke({ messages }, threadConfig);
```

---

## Final Recommendation

### For Louis's Claude Cowork-style platform:

**Primary: Mastra** — Best combination of TypeScript-native design, community momentum, built-in features (RAG, memory, workflows, MCP, evals), and production readiness. The 22k stars and YC backing give confidence in long-term maintenance.

**Secondary: LangGraph.js** — If your platform requires complex graph-based workflows, multi-agent hierarchies, or you're already in the LangChain ecosystem.

### What You Must Build Yourself (No Framework Has It Natively)

1. **Multi-Tenant Isolation Layer** — Workspaces/threads exist but you must enforce org boundaries, RBAC, and data isolation
2. **Skills System** — Installable, composable skill packages are not native to any framework. Build on MCP or a custom tool registry
3. **Self-Improvement** — Error recovery and learning from feedback requires application-specific implementation
4. **Multi-Channel Connectors** — Slack, Teams, WhatsApp, email are not first-party in any framework; build on webhook interfaces

### Bottom Line

All frameworks are missing at least one critical piece for a Claude Cowork-style SaaS. **Mastra** is the strongest foundation to build on because:
- TypeScript-native with the largest community
- Best feature coverage (memory, RAG, workflows, MCP, voice, evals)
- Active development with clear roadmap
- YC-backed commercial entity

You will still need to build: multi-tenant isolation, skills system, self-improvement logic, and multi-channel connectors. These are application-layer concerns that no framework provides out of the box.
