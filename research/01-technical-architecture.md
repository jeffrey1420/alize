# Alizé — Technical Architecture Research
**Date:** 2026-03-30  
**Purpose:** Deep-dive into the technical architecture for Alizé as a managed AI agent platform for French PME/ETI

---

## 1. Multi-Tenant AI Agent Architecture

### Core Deployment Models

Two fundamental patterns exist for multi-tenant agent deployments (per AWS Prescriptive Guidance, July 2025):

**Silo Model (Strong Isolation)**
- Each tenant gets a dedicated agent instance in an isolated environment
- Separate compute, memory, and often separate VPC/network boundaries
- Pros: strongest isolation, no noisy-neighbor risk, simpler compliance reasoning
- Cons: higher cost per tenant, operational complexity at scale
- Use when: tenant has sensitive data (PHI, financial), requires SLA guarantees, or demands dedicated resources

**Pooled Model (Shared Infrastructure)**
- Multiple tenants share the same agent infrastructure
- Logical isolation through tenant IDs, namespacing, row-level security
- Pros: better resource utilization, lower cost, easier to scale
- Cons: cross-tenant leakage risk if isolation isn't airtight, noisy-neighbor risk
- Use when: tenants are low-sensitivity, cost optimization matters

**Hybrid Approach (Recommended for Alizé)**
- Default tier → pooled with strong logical isolation
- Premium/sensitive tenants → siloed or dedicated namespaces
- This mirrors how cloud providers handle multi-tenant PaaS

### Tenant Isolation Layers

Per AWS and Azure guidance, isolation must be enforced at multiple layers:

| Layer | Mechanism |
|-------|-----------|
| **Identity** | Short-lived tokens per tenant, no shared service accounts. Agent gets a first-class autonomous identity tied to tenant scope. |
| **Network** | Per-tenant Kubernetes namespaces + network policies blocking cross-namespace traffic. Private subnets, security groups. |
| **Data** | PostgreSQL row-level security (RLS) with mandatory `tenant_id` filters. Separate vector DB collections per tenant. Object storage with tenant-prefixed paths. |
| **Compute** | Dedicated node pools for sensitive workloads. Shared nodes for standard tier. |
| **Secrets** | Tenant-scoped secrets in vault (e.g., Vault by HashiCorp). No static API keys shared across tenants. |

### Three ML Model Strategies

Azure Architecture Center defines three approaches:

1. **Tenant-Specific Models** — Train dedicated model per tenant on their data only. Highest isolation, highest cost. Appropriate for large ETI with unique needs.

2. **Shared Pretrained Models** — All tenants infer against the same base model. Lowest cost, no tenant data used in training. Most appropriate for generic AI agent tasks (what Alizé would likely use).

3. **Tuned Shared Models** — Shared base model fine-tuned per tenant. Middle ground: leverage foundation model + tenant-specific adaptation. Good for specialized workflows.

**Recommendation for Alizé:** Start with **shared pretrained models** (strategy 2) using an external LLM API (OpenAI, Anthropic, Mistral). This keeps cost predictable, avoids training complexity, and meets most PME/ETI needs. Offer **fine-tuning** as a premium tier for ETI clients needing specialized behavior.

### Tenant Onboarding Flow

```
Tenant signup → Provision tenant record → Create tenant namespace/RLS policies
→ Generate tenant API key / OAuth credentials → Configure MCP integrations
→ Instantiate agent with tenant system prompt → First conversation
```

Onboarding must be automated (Infrastructure as Code / Terraform or Pulumi) to scale to many tenants.

---

## 2. MCP (Model Context Protocol)

### What MCP Is

MCP is an **open standard** introduced by Anthropic (November 2024) for connecting AI applications to external systems. It solves the **N×M integration problem** — without MCP, connecting N AI applications to M data sources requires N×M custom integrations. With MCP, you need only N+M implementations.

Think of MCP as the **USB-C for AI applications**: a standardized way for AI agents to connect to data sources, tools, and services.

### MCP Architecture

The protocol has four main components:

- **MCP Host**: The AI application/agent runtime (e.g., Claude, custom Nuxt app running an agent)
- **MCP Client**: Runs inside the host, mediates between the LLM and MCP servers, handles transport
- **MCP Server**: External service exposing resources (data), tools (actions), and prompts to the LLM
- **Transport Layer**: JSON-RPC 2.0 over stdio (local) or SSE (remote)

**Three primitive types:**

- **Resources** — Data the LLM can read (files, DB records, API responses). Represented as URIs.
- **Tools** — Functions the LLM can invoke to perform actions (send email, query CRM, create Slack message).
- **Prompts** — Reusable templates for common interaction patterns.

### MCP vs RAG

| Aspect | MCP | RAG |
|--------|-----|-----|
| **Purpose** | Two-way interaction + action execution | Information retrieval to augment generation |
| **Mechanism** | Standardized protocol for tool invocation | Embeddings + vector search |
| **Use case** | Agents that do things (book, update, send) | Q&A, document synthesis |
| **Execution** | Can run code, update external systems | Read-only retrieval |

For Alizé, **MCP is the primary integration layer** for connecting agents to client tools. RAG can complement it for knowledge retrieval (e.g., querying a client's document corpus).

### Building an MCP Server

MCP SDKs exist for **Python** and **TypeScript/Node.js** (relevant for the Nuxt stack).

Minimal TypeScript MCP server example:
```typescript
import { MCPServer, Resource, Tool } from '@modelcontextprotocol/sdk/server';

const server = new MCPServer('alize-slack-integration');

// Resource: read-only data
server.addResource('config://slack-workspace', async () => ({
  teamName: 'Acme Corp',
  botToken: process.env.SLACK_BOT_TOKEN // never expose raw to LLM
}));

// Tool: action the LLM can invoke
server.addTool('send-message', async ({ channel, text }) => {
  await slackClient.chat.postMessage({ channel, text });
  return { success: true, channel, text };
});

server.start();
```

### MCP Server Registries

- **MCPServers.org** — Catalog of community MCP servers (npm/PyPI-style registry)
- **Glama.ai** — Enterprise marketplace with security ratings and license compliance
- **cursor.directory** — IDE-integrated MCP servers (Cursor, VS Code)
- **Official Anthropic docs** — Reference implementations and SDKs

### Existing MCP Servers Relevant to Alizé

| Category | Example Servers |
|----------|----------------|
| **CRM** | Salesforce, HubSpot MCP servers (community) |
| **Communication** | Slack, Teams, Gmail, SendGrid |
| **ERP** | SAP (custom build likely needed), Xero, Pennylane (French accounting) |
| **Database** | PostgreSQL, MongoDB, Elasticsearch |
| **Storage** | Google Drive, Notion, Confluence |
| **Development** | GitHub, GitLab, Linear |

For French PME/ETI, building MCP servers for **Pennylane, Qonto, Cegid, Sage** would be high-value.

### MCP Security for Multi-Tenant

From Prefactor's MCP security analysis:

**Critical risks in multi-tenant MCP:**
1. **Cross-tenant data leakage** — A vector DB query without tenant ID filter can return another tenant's documents
2. **Identity confusion** — Shared service accounts blur accountability
3. **Unscoped tool calls** — An agent issuing "search all documents" without tenant context
4. **Logging PII exposure** — Prompts or DB rows logged without redaction

**Security controls:**
- Short-lived tokens instead of static API keys
- Tenant ID embedded in every request metadata `{tenant_id, user_id, agent_id, session_id}`
- Backend (not LLM) handles tenant context resolution — keep secrets and tenant IDs out of prompts
- Mandatory `tenant_id` filter enforced at database layer (PostgreSQL RLS)
- Separate vector DB collections/namespaces per tenant
- mTLS for service-to-service communication
- Per-tenant encryption keys (application-level for PII)

---

## 3. AI Agent Orchestration Patterns

### Hierarchical Agent Patterns

Production AI agent systems typically use a **planner + worker** hierarchy:

**Planner Agent (Orchestrator)**
- Receives the user's high-level request
- Decomposes into sub-tasks
- Routes sub-tasks to appropriate worker agents
- Synthesizes results into final response
- Maintains overall task state and context

**Worker Agents (Sub-agents)**
- Specialized agents focused on specific domains/tasks
- Can be stateless or maintain conversation context
- Invoke tools (via MCP) to execute actions
- Report results back to planner

**Example flow for "Prepare Q4 sales report":**
```
Planner: "Prepare Q4 sales report for Acme"
  → Worker 1: "Fetch CRM deals closed in Q4" (CRM MCP tool)
  → Worker 2: "Get revenue data from accounting system" (ERP MCP tool)
  → Worker 3: "Pull team performance from Slack" (Slack MCP tool)
Planner: Synthesizes → generates formatted report
```

### Existing Frameworks

| Framework | Language | Key Features |
|-----------|----------|--------------|
| **LangChain / LangGraph** | Python, JS/TS | Most popular, extensive tool ecosystem, cloud offering |
| **AutoGen** | Python | Microsoft, multi-agent conv建模 |
| **CrewAI** | Python | Role-based agents, sequential/hierarchical task execution |
| **Semantic Kernel** | C#, Python | Microsoft, enterprise-focused |
| **agno** | Python | Lightweight, agent-focused |
| **Nuxt AI / Nitro AI** | JS/TS | Native Nuxt integration, server-side AI |

### Tool Use vs Agents

- **Tool Use (Function Calling)**: LLM decides when to call a specific function. Single-agent, synchronous. Best for: discrete, predictable actions (send email, query DB).
- **Multi-Agent Orchestration**: Multiple specialized agents collaborate. Best for: complex workflows requiring different domains.

For Alizé, the recommended pattern is **hierarchical multi-agent** with MCP as the tool integration layer.

### State Management

Agents need to maintain:
- **Session state** — Conversation history per user/tenant
- **Workflow state** — Task decomposition progress (for long-running workflows)
- **Tool results** — Cached results from MCP tool calls to avoid redundant calls

PostgreSQL is well-suited for session/workflow state. Redis can serve as a low-latency cache layer.

---

## 4. French Data Compliance (CNIL)

### Overview

CNIL (Commission Nationale de l'Informatique et des Libertés) is France's data protection authority, enforcing **RGPD** (French GDPR implementation) and the **1978 Informatique et Libertés law**.

### Key Obligations for AI Agent Platforms

**1. Lawful Basis for Processing**
- Processing personal data requires a legal basis: consent, contract, legitimate interest, or legal obligation
- For AI agents processing client data: typically contract (service delivery) or consent
- Must document which data is processed and why

**2. Transparency (Articles 13-14 RGPD)**
- Users must be informed about: identity of the controller, purpose of processing, data recipients, retention periods, rights
- For AI agents: disclose that an AI processes their queries, what context is retained, how decisions are made
- French translation required for all privacy notices

**3. Purpose Limitation & Data Minimization**
- Data collected for one purpose cannot be repurposed without new legal basis
- AI agent platforms: system prompts and context should be scoped to the specific service purpose
- Avoid using client conversation data to train models without explicit consent + transparency

**4. Data Subject Rights**
- Right to access, rectify, erase, port, object
- Must provide mechanisms for clients to exercise these rights over their data
- Erasure must be technically enforced (not just "hidden")

**5. Data Protection by Design (Article 25 RGPD)**
- Privacy must be baked into architecture, not added after
- For Alizé: tenant isolation, RLS, encryption, access controls should be default-on

**6. DPO Requirement**
- A DPO is mandatory if core activity involves systematic monitoring of individuals or processing of sensitive data at scale
- Most AI agent platforms should appoint one voluntarily

### CNIL-Specific AI Guidance

CNIL published AI-specific recommendations (2024-2025) clarifying:
- GDPR applies to AI models as data controllers/processors
- Training data must be documented and lawful
- Automated decision-making (Article 22) — individuals have right to human review
- Security requirements for AI systems: access controls, logging, incident response

### Data Residency Requirements

**Critically important for French companies:**

- **SecNumCloud** (ANSSI certification) — Required for some public sector and sensitive industry clients
- **EU data residency** — GDPR requires data to remain in EU/EEA; UK adequacy decision needed for UK transfers
- **OVH European zones** — OVH offers data residency guarantees in EU zones (RBX, SBG, GRA, etc.)

For Alizé: all client data must stay within **EU-based infrastructure** (OVH EU zones satisfy this). Clients in regulated industries (healthcare, finance) may require SecNumCloud or equivalent.

### Practical Architecture Implications

| Requirement | Implementation |
|-------------|----------------|
| Tenant data isolation | PostgreSQL RLS, dedicated schemas, encrypted storage |
| Right to erasure | Soft delete + scheduled purge; cascade to vector DB, logs |
| Audit logging | All data access logged with timestamp, actor, tenant ID |
| Data transfer control | No data leaves EU; OVH EU zones only |
| Consent management | Granular consent tracking for different data uses |
| Incident response | Breach notification to CNIL within 72h (Article 33) |
| DPA (Data Processing Agreement) | Must contractually commit to RGPD obligations |

---

## 5. OVH / Caldoche Hosting for AI Workloads

### OVHcloud AI Infrastructure Options

OVHcloud offers several AI-focused services:

**1. AI Notebooks**
- JupyterLab-based development environment
- GPU options: Tesla V100, RTX 3080/3090, H100 (on higher-tier plans)
- Pre-configured frameworks (PyTorch, TensorFlow, Hugging Face)
- Use for: development, experimentation, fine-tuning
- Pricing: roughly €1.89–€14/hour depending on GPU

**2. AI Training**
- Managed training jobs with distributed GPU support
- Handles dataset mounting, experiment tracking
- Use for: fine-tuning models, training custom models
- Pricing: hourly based on GPU count

**3. Managed Inference (AI Deployments)**
- Serve models via API with autoscaling
- Serverless option (scales to zero)
- Use for: production inference endpoints
- Pricing: per inference minute / request

**4. GPU Instances (Bare Metal Cloud)**
- Direct access to NVIDIA GPUs: H100 SXM, H100 PCIe, A100, RTX series
- Full control over the machine
- Use for: high-performance inference, custom agent runtimes
- Pricing: ~€14–€20/hr for H100 PCIe, ~€18–€25/hr for H100 SXM

**5. Standard OVH VPS / Dedicated Servers**
- For non-GPU workloads: web servers, databases, API backends
- Much cheaper than GPU instances
- PostgreSQL managed database (OVH Cloud Databases)

### OVH AI Tier Comparison

| Workload | Recommended OVH Service | Estimated Cost |
|----------|------------------------|----------------|
| Agent orchestration API (Nuxt server) | VPS / Starter dedicated | €20–€100/month |
| PostgreSQL per tenant | OVH Cloud DB (Managed PostgreSQL) | €30–€200/month per tenant (or shared with isolation) |
| Vector embeddings | AI Training + custom service | €2–€8/hour (on-demand GPU) |
| LLM inference (external API) | External provider (Anthropic, Mistral) | Per-token pricing |
| LLM inference (self-hosted, large scale) | H100 bare metal | €14–€20/hour |
| Session/state cache | OVH Cloud DB for Redis | €20–€50/month |

### OVH vs Alternatives for French Companies

| Provider | Pros | Cons |
|----------|------|------|
| **OVHcloud** | EU data residency, French company, competitive pricing, full-stack (IaaS + PaaS) | Less managed AI services than hyperscalers |
| **Scaleway** | French, GDPR-compliant, pop:// edge network | Smaller GPU fleet |
| **AWS/GCP/Azure** | Mature AI/ML services (SageMaker, Vertex AI, Foundry) | Data residency concerns for French clients, more expensive |
| **Hetzner** | Very competitive pricing, German (EU) | Limited AI-specific services |

**Recommendation for Alizé:** Start with **OVHcloud** for the core infrastructure (Nuxt hosting, PostgreSQL, Redis, object storage) to satisfy French/EU data residency. Use **external LLM APIs** (Mistral, Anthropic, OpenAI) for inference rather than self-hosting — avoids GPU complexity. Scale to self-hosted inference only if client volume justifies it.

### Cost Benchmarks (2025 Estimates)

```
Agent API Server (Nuxt, lightweight):    €50/month (VPS)
Managed PostgreSQL (shared, isolated):    €50/month
Managed Redis (session cache):           €20/month
Object Storage (S3-compatible):          €5-50/month (based on usage)
LLM API calls (Mistral/Anthropic):       €0.01-2/1k tokens (varies widely)
GPU for batch embedding jobs:            €2-8/hour (on-demand)
```

For 100 PME clients at ~1000 messages/month each:
- LLM cost: ~€500–€2000/month (depending on model and context size)
- Infrastructure: ~€200–€500/month (excluding LLM)

---

## 6. Reference Architectures

### AWS Multi-Agent Architecture (Builder's Guide)

AWS prescriptive guidance (July 2025) outlines core patterns:

**Agent Lifecycle:**
1. User request → Agent Runtime (Bedrock Agent / custom)
2. Task decomposition → Sub-agent routing
3. Tool invocation (via function calling / MCP)
4. Response synthesis
5. Logging & monitoring

**Multi-tenant considerations:**
- Tenant context passed as **session attributes** (not in prompts)
- Separate inference endpoints or endpoint groups per tenant if using Bedrock
- CloudWatch / observability scoped per tenant

### Azure AI / Microsoft Foundry

Azure's approach to multi-tenant AI:

- **Microsoft Foundry** — Unified API layer for AI models, handles multi-tenancy natively
- **Azure AI Search** — Multi-tenant vector search with namespace isolation
- **Azure OpenAI** — API-managed, tenant isolation via Azure's infrastructure
- Tenant data never used for training by default (configurable opt-in)

### CrewAI Architecture

Open-source reference for hierarchical multi-agent:

```
Crew
├── Agent (Planner/Manager)
│   ├── role: "project_manager"
│   ├── goal: "Coordinate sub-agents"
│   └── agents: [sub_agent_1, sub_agent_2]
├── Agent (Worker 1)
│   └── tools: [mcp_tool_1]
└── Agent (Worker 2)
    └── tools: [mcp_tool_2]
```

Task execution: sequential or hierarchical (parallel execution with manager synthesis).

### LangChain / LangGraph

LangGraph extends LangChain with stateful, graph-based agent workflows:

- **Nodes**: Agent, tools, conditionals
- **Edges**: Control flow between nodes
- **State**: Shared state across graph execution (critical for multi-step workflows)

```python
# Conceptual LangGraph for Alizé agent
workflow = StateGraph(AgentState)
workflow.add_node("planner", planner_agent)
workflow.add_node("crm_worker", crm_agent)
workflow.add_node("slack_worker", slack_agent)
workflow.add_node("synthesizer", synthesize_agent)
workflow.add_edge("planner", "crm_worker")  # conditional routing
workflow.add_edge("crm_worker", "slack_worker")
workflow.add_edge("slack_worker", "synthesizer")
```

### MCP-Powered Architecture

The full MCP integration stack for Alizé:

```
┌─────────────────────────────────────────────────────┐
│                   Client Browser                     │
└──────────────────────┬──────────────────────────────┘
                       │ HTTPS
┌──────────────────────▼──────────────────────────────┐
│                Nuxt 3 Application                     │
│  ┌─────────────────────────────────────────────────┐ │
│  │           Agent Runtime (Nuxt server)            │ │
│  │  ┌──────────┐  ┌───────────┐  ┌───────────────┐  │ │
│  │  │ Planner  │  │ Sub-agent │  │ State Manager │  │ │
│  │  │ (LLM)    │  │ workers   │  │ (Redis/PG)    │  │ │
│  │  └────┬─────┘  └─────┬─────┘  └───────────────┘  │ │
│  └───────┼──────────────┼───────────────────────────┘ │
│          │              │                               │
│  ┌───────▼──────────────▼───────────────────────────┐ │
│  │              MCP Client SDK                        │ │
│  └───────┬──────────────┬───────────────────────────┘ │
└──────────┼──────────────┼─────────────────────────────
           │ stdio / SSE  │
┌──────────▼──────┐ ┌─────▼─────────────────────────────┐
│  MCP Server:    │ │  MCP Server:                        │
│  Slack          │ │  Pennylane (ERP/accounting)        │
│  (per-tenant)  │ │  (per-tenant)                       │
└─────────────────┘ └───────────────────────────────────┘
```

### Platform Comparison (Managed AI Agent Platforms)

| Platform | Focus | Multi-tenant | MCP Support |
|----------|-------|--------------|-------------|
| **AWS Bedrock Agents** | Enterprise | Yes (via session attributes) | Native function calling |
| **Microsoft Copilot Studio** | Business automation | Yes | MCP-compatible |
| **CrewAI** | Multi-agent编排 | Self-hosted, configurable | Community MCP servers |
| **AutoGen** | Research/enterprise | Self-hosted | Custom implementation |
| **LiveChat AI** | Customer service agents | Yes | Integrations (Slack, CRM) |
| **Intercom Fin** | Customer support | Per-workspace | No (proprietary) |
| **Zendesk AI** | Support agents | Per-account | Limited |

No single direct competitor to Alizé exists in the French PME market. The gap is: **no managed AI agent platform built for French businesses with pre-built ERP/CRM integrations and CNIL-compliant infrastructure**.

---

## 7. Recommended Architecture for Alizé

### Stack Summary

| Component | Technology |
|-----------|------------|
| **Frontend** | Nuxt 3 (Vue 3) |
| **Backend / Agent Runtime** | Nuxt server (Nitro) + Node.js |
| **Database** | PostgreSQL (managed, OVH EU zone) |
| **Session/State Cache** | Redis (OVH managed) |
| **Object Storage** | OVH Object Storage (S3-compatible) |
| **LLM Provider** | Mistral AI (French, GDPR-compliant API) + Anthropic Claude (fallback) |
| **Agent Framework** | LangChain.js or custom Node.js agent runtime |
| **Integration Layer** | MCP (Model Context Protocol) |
| **Hosting** | OVHcloud (VPS + AI instances on-demand) |
| **Auth** | JWT + tenant-scoped API keys |

### Tenant Isolation Implementation

```
PostgreSQL (single cluster)
├── Schema: tenant_001
│   ├── conversations
│   ├── sessions
│   ├── tool_configs
│   └── audit_logs
├── Schema: tenant_002
│   └── ...
└── Schema: platform_admin (Alizé internal)
```

- Row-Level Security (RLS) policies enforce tenant isolation
- Every query passes through a middleware that injects `SET app.current_tenant = 'tenant_001'`
- MCP servers receive tenant credentials scoped to their namespace

### Key Open Questions

1. **LLM provider choice**: Mistral for French/EU compliance + competitive pricing, but how does it compare to Claude/GPT-4 for complex reasoning?
2. **MCP server coverage**: Which French ERP/CRM integrations to build first? Pennylane (accounting) and Qonto (banking) would cover many PME needs.
3. **Self-hosted vs API inference**: Self-hosting Mistral 7B locally could reduce costs at scale but adds operational complexity.
4. **Agent memory**: Long-term memory per tenant — vector DB (pgvector) or external? pgvector simplifies the stack.
5. **Pricing model**: Per-seat, per-message, or outcome-based pricing for French PME?

---

*Research compiled 2026-03-30. Key sources: AWS Prescriptive Guidance (July 2025), Azure Architecture Center, Anthropic MCP documentation, CNIL publications, OVHcloud documentation, Prefactor MCP security analysis, Medium/Glama/GCP MCP guides.*
