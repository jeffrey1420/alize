# Multi-Tenant AI Agent SaaS Building Blocks
## Deep Research Report - March 2026

**Context:** Building a Claude Cowork-style AI assistant as a SaaS product (teams sign up, get AI agents with skills, memory, subagents, cron, document management — multi-tenant, not per-user deployments).

**Known:** OpenClaw, LangChain, LangGraph, Dify, n8n, Weaviate, BullMQ are already known. This report focuses on additional/complementary projects.

---

## Table of Contents
1. [MCP (Model Context Protocol) Ecosystem](#1-mcp-model-context-protocol-ecosystem)
2. [Multi-Tenant AI Agent Platforms](#2-multi-tenant-ai-agent-platforms)
3. [Backend-as-a-Service for AI](#3-backend-as-a-service-for-ai)
4. [Composable/Headless AI Frameworks](#4-composableheadless-ai-frameworks)
5. [Vector DB + RAG as a Service](#5-vector-db--rag-as-a-service)
6. [Scheduling/Cron for SaaS](#6-schedulingcron-for-saas)
7. [Auth & Multi-Tenancy](#7-auth--multi-tenancy)
8. [Specific Agent Components (Building Blocks)](#8-specific-agent-components-building-blocks)
9. [French/European AI SaaS Platforms](#9-frencheuropean-ai-saas-platforms)
10. [New Projects 2025-2026](#10-new-projects-2025-2026)

---

## 1. MCP (Model Context Protocol) Ecosystem

### Official MCP SDK
- **Repo:** https://github.com/modelcontextprotocol/typescript-sdk
- **Type:** Official TypeScript SDK for MCP
- **Multi-tenant:** N/A (protocol/library)
- **TypeScript/Node.js:** ✅ Yes, runs on Node.js, Bun, Deno
- **v2 in development** (Q1 2026 target), v1.x current recommended
- **Building block:** Foundation for building MCP-compatible servers and clients
- **Packages:**
  - `@modelcontextprotocol/server` - Build MCP servers
  - `@modelcontextprotocol/client` - Build MCP clients
  - `@modelcontextprotocol/node` - Node.js HTTP transport wrapper
  - `@modelcontextprotocol/express` - Express helpers
  - `@modelcontextprotocol/hono` - Hono helpers

### Official MCP Servers Repository
- **Repo:** https://github.com/modelcontextprotocol/servers
- **Reference implementations:**
  - `everything` - Reference/test server with prompts, resources, tools
  - `fetch` - Web content fetching
  - `filesystem` - Secure file operations with configurable access controls
  - `git` - Git repository tools
  - `memory` - Knowledge graph-based persistent memory system ⭐
  - `sequentialthinking` - Dynamic problem-solving through thought sequences
  - `time` - Time and timezone conversion

### Awesome MCP Servers Lists
- **punkpeye/awesome-mcp-servers:** https://github.com/punkpeye/awesome-mcp-servers
- **wong2/awesome-mcp-servers:** https://github.com/wong2/awesome-mcp-servers
- **TensorBlock/awesome-mcp-servers:** Claims 7260+ MCP servers as of May 2025
- **Registry:** https://registry.modelcontextprotocol.io/

### MCP Servers by Category

#### Knowledge & Memory
- **mcp-knowledge-graph** (shaneholloman) - Persistent memory via local knowledge graph
- **mcp-memory-service** (doobidoo) - REST API + knowledge graph, works with LangGraph, CrewAI, AutoGen
- **MemoryMesh** (CheMiguel23) - Knowledge graph MCP server based on official Memory server
- **memory-mcp-server** (okooo5km) - Knowledge graph management for LLM memory
- **knowledgegraph-mcp** (n-r-w) - Multiple storage backends, fuzzy search

#### Aggregators (Multi-server)
- **1mcp/agent** - Unified MCP server aggregating multiple MCP servers
- **Aganium/agenium** - Bridge MCP servers to agent:// network with mTLS, trust scores
- **askbudi/roundtable** - Meta-MCP unifying Codex, Claude Code, Cursor, Gemini

#### Code & Development
- **Browser automation MCP servers**
- **Code execution servers**
- **Version control (Git) servers**

### MCP Insights
- **Multi-tenant consideration:** MCP itself is a protocol; multi-tenancy depends on server implementation
- **Key benefit for Alize:** MCP provides standardized tool abstraction — agents can consume tools from any MCP server uniformly
- **Gotcha:** v2 SDK breaking changes coming Q1 2026

---

## 2. Multi-Tenant AI Agent Platforms

### Inngest
- **Site:** https://www.inngest.com/
- **GitHub:** https://github.com/inngest/inngest
- **Multi-tenant:** ✅ Yes - explicitly mentions "multi-tenant aware, multi-tier queue"
- **TypeScript/Node.js:** ✅ Yes, first-class support
- **Stars:** Not publicly disclosed (enterprise project)
- **What it does:** Workflow orchestration platform for durable functions, AI workflows, background jobs
- **Multi-tenant features:**
  - Multi-tenant aware queue with fairness and flow control
  - Concurrency, throttling, prioritization, debouncing, rate limiting
  - Batching support
- **Self-hosting:** ✅ Available (announced Jan 2026)
- **Building block:** Workflow orchestration, not a full agent platform
- **Gotcha:** More workflow/orchestration focused than agent-focused, but excellent for AI agent task scheduling

### Trigger.dev
- **Site:** https://trigger.dev/
- **GitHub:** https://github.com/triggerdotdev/trigger.dev
- **Multi-tenant:** Partial - supports concurrency keys per-tenant
- **TypeScript/Node.js:** ✅ Yes
- **What it does:** AI agent and workflow platform with long-running tasks, retries, queues
- **Features:**
  - Long-running tasks without timeouts
  - Per-tenant queuing using concurrency keys
  - Realtime streaming
  - Human-in-the-loop
  - Checkpointing/resumability
- **Building block:** Full platform (hosted), not a building block

### Network-AI (Jovancoding)
- **Site:** https://network-ai.org/
- **GitHub:** https://github.com/Jovancoding/Network-AI
- **Multi-tenant:** Architecture supports it (atomic propose→validate→commit)
- **TypeScript/Node.js:** ✅ Yes, primary language
- **What it does:** TypeScript/Node.js multi-agent orchestrator with shared state, guardrails
- **Features:**
  - 17 framework adapters: LangChain, AutoGen, CrewAI, OpenAI Assistants, LlamaIndex, Semantic Kernel, Haystack, DSPy, Agno, MCP, OpenClaw, A2A, Codex, MiniMax, NemoClaw, APS
  - Shared blackboard with locking — prevents race conditions in parallel agents
  - Guardrails and budgets (FSM governance, per-agent token ceilings)
  - HMAC/Ed25519 audit trails, permission gating
  - Persistent project memory (Layer 3)
- **Use case:** Coordination layer for multi-agent systems
- **Building block:** ✅ Excellent - can orchestrate any AI framework

### China Unicom Yuanjing Wanwu Agent Platform
- **Note from GitHub Topics:** "enterprise-grade, multi-tenant AI agent development platform"
- **Supports:** Intelligent agents, workflows, RAG, model management
- **Note:** Chinese market focus

---

## 3. Backend-as-a-Service for AI

### Mem0
- **Site:** https://mem0.ai/
- **GitHub:** https://github.com/mem0ai/mem0
- **Multi-tenant:** ✅ Yes (org_id, project_id support in API)
- **TypeScript:** ✅ Yes (npm package available)
- **Python:** ✅ Yes (primary)
- **Stars:** 37k+ on GitHub
- **What it does:** Universal memory layer for AI agents
- **Key Features:**
  - Multi-Level Memory: User, Session, Agent state
  - Adaptive personalization
  - +26% accuracy vs OpenAI Memory on LOCOMO benchmark
  - 91% faster than full-context
  - 90% fewer tokens
  - Graph memory (Neo4j integration)
- **Integrations:** OpenAI, Anthropic, Google, Ollama, Groq
- **Deployment:** Cloud API or self-hosted
- **Building block:** ✅ Excellent memory component for multi-tenant AI agents
- **Gotcha:** Requires LLM for memory inference (default: GPT-4.1-nano)

### OpenMemory MCP
- **GitHub:** https://github.com/gzhang33/openmemory
- **What it does:** Local and secure memory management MCP server
- **Multi-tenant:** Architecture-dependent
- **Building block:** Memory component

---

## 4. Composable/Headless AI Frameworks

### VoltAgent
- **Site:** https://voltagent.dev/
- **GitHub:** https://github.com/VoltAgent/voltagent
- **Multi-tenant:** Platform supports teams/organizations
- **TypeScript/Node.js:** ✅ Yes, primary language
- **What it does:** AI Agent Engineering Platform with open-source TypeScript framework
- **Components:**
  - **Core Runtime (@voltagent/core):** Agents with typed roles, tools, memory, model providers
  - **Workflow Engine:** Declarative multi-step automations
  - **Supervisors & Sub-Agents:** Supervisor runtime routing tasks
  - **Tool Registry & MCP:** Zod-typed tools with lifecycle hooks
  - **Memory:** Durable memory adapters
  - **RAG:** Retrieval and VoltAgent Knowledge Base service
  - **Voice:** TTS/STTS with OpenAI, ElevenLabs
  - **Guardrails:** Input/output validation
  - **Evals:** Agent eval suites
- **Building block:** Full framework with modular components

### Atomic Agents
- **Site:** https://brainblend-ai.github.io/atomic-agents/
- **GitHub:** https://github.com/BrainBlend-AI/atomic-agents
- **Multi-tenant:** Architecture-dependent
- **TypeScript:** Python primary (pip install)
- **What it does:** Lightweight, modular framework for building Agentic AI pipelines
- **Key concepts:**
  - Single-purpose, reusable, composable components
  - Built on Instructor and Pydantic
  - Atomic operations design
  - CLI tool (Atomic Assembler) for downloading Tools
- **Building block:** Component library (agents, tools, context providers)
- **Note:** Python-first, less relevant if TypeScript stack required

### Google Agent Development Kit (ADK)
- **Site:** https://google.github.io/adk-docs/
- **GitHub:** https://github.com/google/adk-js
- **Released:** December 17, 2025
- **Multi-tenant:** Architecture-dependent
- **TypeScript/Node.js:** ✅ Yes
- **What it does:** Code-first framework for building, evaluating, deploying AI agents
- **Features:**
  - Modular multi-agent systems
  - Code-first development (version control, testing, CI/CD)
  - Agent orchestration
  - Built-in evaluation
- **Integration:** AgentOps, LangChain, other observability tools
- **Building block:** Full framework from Google
- **Gotcha:** Optimized for Gemini/Google ecosystem but model-agnostic

### OpenAI Agents SDK
- **Docs:** https://openai.github.io/openai-agents-js/
- **GitHub:** https://github.com/openai/openai-agents-js (JS/TS)
- **GitHub:** https://github.com/openai/openai-agents-python (Python)
- **Released:** December 2024 (Swarm), evolved to Agents SDK 2025
- **Multi-tenant:** Architecture-dependent
- **TypeScript/Node.js:** ✅ Yes
- **What it does:** Lightweight multi-agent workflow framework
- **Core concepts:**
  - Agents with instructions, tools, guardrails, handoffs
  - Handoffs for agent-to-agent delegation
  - Sessions (conversation history)
  - Tracing
  - Realtime voice agents
  - Guardrails
  - Human-in-the-loop
- **Provider-agnostic:** Works with OpenAI and 100+ other LLMs
- **Building block:** Framework, not platform

### Network-AI
- (See Section 2 - Multi-Tenant AI Agent Platforms)

### ts-swarm
- **GitHub:** https://github.com/joshmu/ts-swarm
- **What it does:** Minimal agentic library mixing OpenAI Swarm simplicity with Vercel AI SDK flexibility
- **TypeScript:** ✅ Yes
- **Building block:** Lightweight alternative to full frameworks

### Agent Development Kit (ADK-TS) - Third Party Port
- **GitHub:** https://github.com/njraladdin/adk-typescript
- **What it does:** TypeScript port of Google's ADK
- **Note:** Community port, may lag official releases

---

## 5. Vector DB + RAG as a Service

### Qdrant
- **Site:** https://qdrant.tech/
- **Multi-tenant:** ✅ Yes - Tiered Multitenancy
- **TypeScript/Node.js:** ✅ Yes (official client)
- **What it does:** Open-source vector database (Rust), managed cloud offering
- **2025 Updates:**
  - Tiered Multitenancy for efficient small/large tenant support
  - Single Sign-On (SSO) and RBAC
  - Granular database API keys
  - Terraform-enabled Cloud API
  - Qdrant Cloud Inference: unified embedding + vector search
- **Multi-tenant patterns:** Namespaces within indexes, metadata filtering
- **Building block:** Vector storage with multi-tenant isolation
- **Self-hosted option:** ✅ Yes (open source)

### Pinecone
- **Site:** https://www.pinecone.io/
- **Multi-tenant:** ✅ Yes - serverless architecture with namespaces
- **TypeScript/Node.js:** ✅ Yes (client libraries)
- **What it does:** Managed vector database for AI applications
- **Multi-tenant features:**
  - Serverless: millions of namespaces per index
  - Multi-tenant compute layer
  - Metadata filtering for tenant isolation
  - Pay-as-you-go pricing
- **Building block:** Vector storage (cloud-only, no self-hosted)

### Cohere Embed
- **Site:** https://cohere.com/embed
- **What it does:** Embeddings API for semantic retrieval, RAG
- **Models:** Embed v3 (multimodal), Embed v4 (128K context)
- **Multi-tenant:** Managed service
- **Building block:** Embedding generation, not storage

### RAG-as-a-Service Pattern
- **n8n + Supabase (pgvector):** Enterprise-grade Agentic RAG pipelines
- **GitHub example:** anshwysmcbel2710/agentic-rag-n8n-ingestion-pipeline
- **Features:** Event-driven orchestration, hybrid RAG, multi-tenant architecture

---

## 6. Scheduling/Cron for SaaS

### Inngest
- (See Section 2 - excellent for multi-tenant scheduling)

### Trigger.dev
- (See Section 2 - includes durable cron schedules)

### Temporal
- **Note:** Known (durable workflows), still relevant for multi-tenant AI agents
- **Multi-tenant:** Can be configured per-tenant isolation

### BullMQ
- **Note:** Known, still relevant for job queues

---

## 7. Auth & Multi-Tenancy

### Better Auth
- **Site:** https://better-auth.com/
- **GitHub:** https://github.com/better-auth/better-auth
- **Multi-tenant:** ✅ Yes - First-class organization plugin
- **TypeScript:** ✅ Yes, framework-agnostic
- **What it does:** Comprehensive authentication framework for TypeScript
- **Key Features:**
  - Email/password authentication
  - Social providers (Google, GitHub, etc.)
  - Two-factor authentication
  - Passkeys
  - **Organization plugin:** Multi-tenant organizations, member management
  - **Access control plugin:** Role-based permissions
  - Prisma integration for database adapters
- **Multi-tenant pattern:**
  ```
  plugins: [organization(), twoFactor(), passkey()]
  ```
- **Building block:** Auth foundation for multi-tenant SaaS
- **Gotcha:** Newer project, ecosystem maturing

### ZenStack
- **Site:** https://zenstack.dev/
- **What it does:** ORM with built-in authorization, multi-tenancy
- **Integration:** Works with Better Auth for auth+authz
- **Multi-tenant:** ✅ Yes - row-level security patterns
- **Building block:** Database access layer with security

### Prisma + Row-Level Security
- **Multi-tenant pattern:** PostgreSQL row-level security policies
- **Architecture:** tenant_id on all multi-tenant tables
- **Building block:** Database layer

---

## 8. Specific Agent Components (Building Blocks)

### Memory Systems
- **Mem0** - (See Section 3)
- **MCP Memory Server** - Official knowledge graph memory
- **openmemory** - Local MCP memory server

### Tool Registries
- **VoltAgent Tool Registry** - Zod-typed tools with lifecycle hooks
- **Atomic Agents CLI** - Atomic Assembler for downloading tools
- **MCP servers** - Standardized tool servers

### Observability
- **AgentOps**
  - **Site:** https://www.agentops.ai/
  - **GitHub:** https://github.com/AgentOps-AI/agentops
  - **What:** Python SDK for AI agent monitoring, LLM cost tracking
  - **Integrations:** CrewAI, Agno, OpenAI Agents SDK, Langchain, AutoGen, AG2, CamelAI
  - **Features:** Session replay, cost management, observability dashboards
  - **Building block:** Observability for agents

- **Langfuse**
  - **Site:** https://langfuse.com/
  - **GitHub:** https://github.com/langfuse/langfuse
  - **What:** Open source LLM engineering platform
  - **Features:** Observability, metrics, evals, prompt management, datasets
  - **Multi-tenant:** Self-hostable, cloud option
  - **Building block:** Observability platform

- **LangSmith**
  - **Note:** Known, still relevant

### Agent Orchestration Adapters
- **Network-AI** (See Section 2) - 17 framework adapters

---

## 9. French/European AI SaaS Platforms

### Mistral AI
- **Site:** https://mistral.ai/
- **Multi-tenant:** Platform service
- **Type:** European AI company (France), open-weight models
- **Valuation:** $14B+ (as of 2025)
- **What they offer:**
  - Frontier AI LLMs
  - AI platform for enterprises
  - Customizable, fine-tuneable AI assistants
  - Autonomous agents
  - Multimodal AI
- **European advantage:** Sovereign AI infrastructure, European languages optimized
- **Note:** Foundation model provider, not multi-tenant SaaS framework

### European Sovereign AI
- **Sovereign AI initiatives:** French/German governments partnered with SAP and Mistral
- **OUTSCALE:** SecNumCloud certified, integrated Mistral's Le Chat
- **Gaia-X:** European data infrastructure initiative

### French AI Startups (2025)
- **Alpic:** $6M pre-seed, AI agent infrastructure for web services
- **Wonderful:** $134M total funding, customer service AI agents

### Embedding/Vector Services with European Options
- **Qdrant:** European company (open source HQ)
- **Cohere:** Has European data residency options

---

## 10. New Projects 2025-2026

### Google ADK (December 2025)
- (See Section 4)

### OpenAI Agents SDK (December 2025)
- (See Section 4)

### Agno (formerly Phidata)
- **Site:** https://www.agno.com/
- **GitHub:** https://github.com/agno-agi/agno
- **Released:** January 2025 (as Agno)
- **Multi-tenant:** Architecture-dependent
- **Type:** Python framework
- **What it does:** Runtime for agentic software, build agents/teams/workflows
- **Features:**
  - Agent OS with teams and workflows
  - Multi-modal agents
  - AgentOps integration
  - Slack, AGUI interfaces
- **Building block:** Python agent framework
- **Note:** Python-first, less relevant for TypeScript stack

### VoltAgent
- (See Section 4)

### Network-AI
- (See Section 2)

### Qdrant Cloud Inference (2025)
- (See Section 5)

---

## Summary: Key Building Blocks for Alize Multi-Tenant SaaS

### Recommended Stack Components

| Layer | Recommended Option | Alternative |
|-------|-------------------|-------------|
| **Agent Framework** | VoltAgent, OpenAI Agents SDK, Google ADK | Network-AI (orchestration) |
| **Memory** | Mem0 | MCP Memory Server, openmemory |
| **Scheduling/Workflow** | Inngest | Trigger.dev |
| **Vector Storage** | Qdrant (self-hosted or cloud) | Pinecone (cloud-only) |
| **Auth** | Better Auth + organization plugin | Better Auth + ZenStack |
| **MCP** | modelcontextprotocol/typescript-sdk | punkpeye awesome list |
| **Observability** | Langfuse (self-hosted) | AgentOps (Python) |

### Multi-Tenancy Patterns

1. **Queue-level isolation:** Inngest's multi-tenant queue
2. **Namespace isolation:** Qdrant/Pinecone namespaces per tenant
3. **Row-level security:** PostgreSQL + ZenStack/Prisma
4. **Org-based auth:** Better Auth organizations plugin

### Gotchas & Considerations

1. **MCP v2 SDK:** Breaking changes coming Q1 2026
2. **Mem0:** Requires LLM for memory inference (cost consideration)
3. **Better Auth:** Newer project, verify ecosystem maturity
4. **VoltAgent:** Newer framework, evaluate stability
5. **Google ADK:** Optimized for Gemini, may need adaptation for other models

---

## Appendices

### A. MCP Server Discovery Resources
- https://registry.modelcontextprotocol.io/ - Official registry
- https://mcpservers.org/ - Web directory
- https://mcp-awesome.com/ - 1200+ verified servers

### B. TypeScript AI Agent Framework Comparison (2026)

| Framework | Stars | Key Differentiator |
|-----------|-------|-------------------|
| VoltAgent | Growing | Full platform + open source |
| OpenAI Agents SDK | Growing fast | Lightweight, official |
| Google ADK | New (Dec 2025) | Google ecosystem |
| Network-AI | Growing | Multi-framework orchestration |
| Atomic Agents | Stable | Python-first, atomic design |

### C. Multi-Tenant Architecture Patterns

```
┌─────────────────────────────────────────────────────┐
│                   SaaS Platform                     │
├─────────────────────────────────────────────────────┤
│  ┌─────────┐  ┌─────────┐  ┌─────────┐           │
│  │ Tenant A│  │ Tenant B│  │Tenant C │           │
│  │ (Org)   │  │ (Org)   │  │ (Org)   │           │
│  └────┬────┘  └────┬────┘  └────┬────┘           │
│       │             │             │                 │
│  ┌────▼────┐  ┌────▼────┐  ┌────▼────┐           │
│  │ Agent   │  │ Agent   │  │ Agent   │           │
│  │ Memory  │  │ Memory  │  │ Memory  │           │
│  └────┬────┘  └────┬────┘  └────┬────┘           │
│       │             │             │                 │
│  ┌────▼─────────────▼─────────────▼────┐           │
│  │        Shared Infrastructure        │           │
│  │  • Inngest (queue, scheduling)     │           │
│  │  • Qdrant (vector storage)         │           │
│  │  • Better Auth (authentication)     │           │
│  └────────────────────────────────────┘           │
└─────────────────────────────────────────────────────┘
```

---

*Research completed: March 2026*
*Sources: GitHub, official documentation, industry reports*
