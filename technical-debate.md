# Alizé Technical Architecture Debate
**Debater:** Alizé Technical Architecture Debater  
**Date:** 2026-03-30  
**Purpose:** Challenge core technical assumptions before Alizé commits to overengineered architecture

---

## Assumption 1: MCP-First is a Differentiator for French PME/ETI Buyers

### Current View

The BRIEF positions MCP as Alizé's primary technical and marketing differentiator. The argument: "No French competitor is positioning on MCP. Agentova certainly isn't." The positioning copy reads: "Our agents connect to your actual business tools — your CRM, email, support queue, document system — not generic AI that doesn't see your data."

### Problem

**This conflates technical differentiation with business differentiation.** French PME/ETI buyers in the 50-250 employee range are not buying AI agents on the basis of protocol standards. They are buying:

- Time saved on specific workflows they can name
- Fewer repetitive admin tasks
- Someone else to configure, maintain, and fix it when it breaks
- Reassurance that their data is safe (French hosting, GDPR compliance)
- A person or team they can call when things go wrong

The BRIEF itself says Alizé's positioning is "managed, governed, professionally deployed agent service" — **the managed service model is the differentiator, not MCP.**

Consider: If you told a French COO "we use MCP, the same standard as Anthropic and OpenAI," they'd nod and ask "but what will it actually do for my team?" If you said "we'll deploy an AI agent that handles your customer service ticket triage, connects to your existing tools, and we manage it for you," they understand the value.

**MCP as a marketing term is technical theater for this audience.** The brief even acknowledges the business audience positioning should be: "Our agents connect to your actual business tools." That's just normal integration — not a differentiator anyone asked for. Every AI agent platform claims to "connect to your tools."

**The real risk:** Building MCP-first infrastructure adds real complexity (MCP server management, per-tenant MCP credentials, MCP security hardening) for zero measurable business impact at the buyer level. This complexity will slow development, increase debugging burden, and create operational surface area — all for a feature that won't appear in a single sales call's value proposition.

**MCP does matter technically** — it's a reasonable integration pattern. But treating it as a competitive moat or marketing angle for this market is a category error.

### Alternative

**Lead with outcomes, not protocols.** The technical architecture should support integrations (including MCP where useful), but the messaging should be:

- "We connect to your existing tools: CRM, email, support, accounting"
- "We deploy and manage the agent — you don't touch the configuration"
- "Everything stays on French servers under your access rules"

MCP becomes an **implementation detail**, not a selling point. If a buyer asks "how do you connect to our systems?" you say "we use standard APIs and a protocol called MCP that lets us connect securely without sharing credentials" — which is reassuring but doesn't lead the conversation.

The competitive moat is the managed service model + French infrastructure + governance layer. That's what Agentova can't easily copy.

### Recommendation

**Demote MCP from differentiator to infrastructure.** Don't position it in sales materials. Focus messaging on: connected tools, managed service, French hosting, governance. Build MCP servers for high-value integrations (Pennylane, Qonto, Cegid), but don't architect the entire system around MCP-first principles. The architecture should be integration-agnostic — use MCP for what it's good at, but don't build a "skills system" on top of MCP when simpler approaches (direct API integrations, webhooks) may be faster to ship.

---

## Assumption 2: The 4-Layer Self-Improvement Architecture is Appropriate for a Pre-Launch Startup

### Current View

The architecture specifies four layers of agent self-improvement:

- **Layer 1 — Runtime Learning:** Bounded curated memory (MEMORY.md ~2KB, USER.md ~1KB)
- **Layer 2 — Skill System:** Autonomous skill creation after 5+ tool calls, SKILL.md format
- **Layer 3 — Session Search:** SQLite FTS5 over all past conversations
- **Layer 4 — Offline Evolution:** DSPy + GEPA on execution traces, $2-10 per run, human-gated PRs

The framing: "Inspired by NousResearch Hermes Agent. Learning at the knowledge level, not the weight level."

### Problem

**This architecture is designed for an AI lab, not a managed service startup with zero clients.**

Let's interrogate each layer's readiness:

**Layer 2 — Skill System ("autonomous skill creation after complex tasks"):** The brief describes agents that "autonomously" create skills after 5+ tool calls. This is a significant AI feature that requires:
- Robust evaluation of whether a skill is actually useful vs. noise
- Conflict resolution when two skills cover similar ground
- A skill review UX (the "progressive disclosure" UI)
- Maintenance burden as skills accumulate and some become stale

For a startup that hasn't launched, with no real agent execution data, you cannot know what skills will look like in production. Building autonomous skill creation now means building something you may have to completely redesign after seeing how users actually work with agents.

**Layer 4 — Offline Evolution (DSPy + GEPA):** Running DSPy optimization on execution traces sounds sophisticated, but:
- You need substantial execution data before DSPy produces meaningful results (the paper "Adaptive RAG" by NousResearch uses this in research contexts with large datasets)
- $2-10 per optimization run sounds cheap, but if you're running it per-client, per-iteration, the costs add up
- Human-gated PRs mean someone at Alizé reviews skill updates — that's an operational cost that doesn't scale
- This is week-24-plus thinking, not week-1 thinking

**The 80/20 problem:** A startup pre-launch needs to validate that clients will actually use and pay for AI agents. The fastest path to that validation is: ship basic agents with basic memory (Layer 1 only), connect them to real tools, get them into users' hands. Self-improvement features are polish that assume the core product works. Building polish before product is a classic startup mistake.

**Session Search (Layer 3) — SQLite FTS5 over all past conversations:** This is reasonable, but it's not Layer 4 "offline evolution." Layer 3 is a search feature, not a learning feature. Calling it part of a "self-improvement architecture" overstates what it does.

### Alternative

**Ship Layer 1 (bounded curated memory) only. Everything else is Phase 2.**

Layer 1 is straightforward: give agents a fixed context window with curated memory. This is basically a RAG pipeline over session history. This is:
- Solvable with existing pgvector + LLM summarization
- Not operationally expensive
- Provides immediate user-facing value (agents that remember context)

Layers 2, 3, and 4 should be **user stories for Q3/Q4**, not architectural commitments in week 1.

Specifically:
- **Layer 2 (Skill System):** Only build this after you observe clients asking "can the agent learn to do X automatically?" in production. Otherwise you're building speculative features.
- **Layer 3 (Session Search):** Rename this to "conversation search" and treat it as a UX feature, not a learning system. Build it when clients complain about losing information in long conversations.
- **Layer 4 (Offline Evolution):** This requires meaningful execution data. Plan for it in Q3/Q4 when you have 10+ active clients producing traces.

### Recommendation

**Strip the self-improvement architecture to Layer 1 only for launch.** Add layers incrementally based on production feedback. The current 4-layer spec reads like a research paper implementation plan, not a startup launch plan. Complexity here is not a virtue — it's a distraction from validating whether anyone will pay for the core product.

---

## Assumption 3: Mastra is the Right Agent Framework Choice

### Current View

The BRIEF states: "Agent runtime: Mastra — 22k stars, TypeScript-native, RAG/memory/workflows/MCP built-in." The architecture rationale: "Natural fit with Hono; Mastra embeds in Hono." The monorepo structure shows `apps/engine/` running "Hono + Mastra."

### Problem

**Mastra is a young project (launched ~late 2024/early 2025) with 22k GitHub stars, which sounds impressive but represents limited production hardening.** The 22k stars figure is doing a lot of work in the decision rationale — stars are not the same as production deployments, enterprise customers, or battle-tested reliability.

Consider the alternatives in the research document itself:

| Framework | Language | Maturity Signal |
|-----------|----------|----------------|
| LangChain/LangGraph | Python, JS/TS | Dominant market position, extensive production use |
| AutoGen | Python | Microsoft backing, research + enterprise |
| CrewAI | Python | Strong open-source community, multi-agent focus |
| Custom Node.js | JS/TS | Full control, no framework dependency |

**Mastra's specific risks:**

1. **Framework lock-in:** Mastra has its own concepts for workflows, agents, tools, and memory. If Mastra makes a breaking change or abandons the project, Alizé rewrites a significant portion of its agent logic. A custom Node.js agent runtime on raw LLM APIs gives full control.

2. **TypeScript-only constraint:** If you later need Python-based agents (for example, for ML-specific tasks, or because a client requires a Python integration), Mastra doesn't help. LangChain/LangGraph works in both Python and JS/TS.

3. **MCP integration as a feature vs. native:** The BRIEF positions MCP as critical, but Mastra's MCP support is "built-in" — meaning it's a feature of the framework. If you want to customize MCP behavior deeply (per-tenant MCP servers, MCP security hardening), you may find yourself fighting the framework.

4. **Production trace count:** "22k stars" is a GitHub vanity metric. What's Mastra's production deployment count? What's its enterprise customer base? LangChain has been in production at thousands of companies for years. Mastra hasn't had time to accumulate that track record.

5. **Why Mastra + Hono specifically?** Hono is a lightweight web framework, which is fine. But Nitro (Nuxt's server) already handles server-side logic. The research document mentions "Nuxt server (Nitro) + Node.js" as the runtime. If you're already using Nitro, why add Hono as a separate server layer? This adds a network boundary between the Nuxt frontend and the Mastra engine that may not be necessary.

### Alternative

**Build a lean agent runtime on raw LLM APIs (or use LangChain.js) instead of Mastra.**

The core agent loop is not that complex:
1. Receive user message
2. Fetch session context (Redis + PostgreSQL)
3. Call LLM with system prompt + context
4. Parse tool calls from LLM response
5. Execute tool calls via MCP or direct API
6. Stream response back to user
7. Update session state

This fits in a few hundred lines of TypeScript using the official Anthropic SDK or OpenAI SDK. It's debuggable, maintainable, and has no framework dependency.

If you need more structure, **LangChain.js** gives you workflow primitives without the full framework weight, with years of production hardening and a massive community.

**The Hono + Mastra + Nuxt stack has three frameworks** (Nuxt, Hono, Mastra) with their own concepts, documentation, and debugging models. That's a significant cognitive load for a small team.

### Recommendation

**Evaluate: Can you ship the pilot with a custom agent runtime (500-800 lines of TypeScript) using the Anthropic SDK directly, instead of adding Mastra?** If the answer is yes, defer Mastra. If LangChain.js primitives (Document loaders, retrievers, chains) provide clear value for the RAG pipeline, use those selectively.

The BRIEF already says "LangChain.js or custom Node.js agent runtime" in the research document's stack summary — this is actually the right instinct. Mastra should be the contingency, not the default.

---

## Assumption 4: The Multi-Tenant Hybrid (Silo/Pooled) Architecture is Realistic for OVHcloud VPS at Launch Scale

### Current View

The architecture recommends:
- **Default tier:** Pooled with strong logical isolation (PostgreSQL RLS, tenant-scoped secrets, per-tenant vector DB collections)
- **Premium tier:** Siloed with dedicated namespaces
- PostgreSQL RLS policies enforced via `SET app.current_tenant = 'tenant_id'` middleware
- Kubernetes namespaces for compute isolation
- OVHcloud VPS deployment at €25-40/month

### Problem

**A €25-40/month VPS cannot run the described multi-tenant architecture.** Let's break it down:

The described infrastructure:
- PostgreSQL (managed OVH Cloud DB) — the research document says €30-200/month **per tenant** or shared with isolation. At €50-100/month minimum for managed PostgreSQL, that's already more than the entire VPS budget.
- Redis for session cache — €20-50/month
- Object Storage — €5-50/month
- The VPS itself for the Nuxt + Mastra runtime

The research document's own cost table says: **"Agent API Server (Nuxt, lightweight): €50/month (VPS)"** — that's the entire compute budget. PostgreSQL managed, Redis managed, and object storage don't fit in €25-40/month.

**The hybrid silo/pooled model is enterprise architecture running on startup infrastructure.**

PostgreSQL RLS with proper tenant isolation is not trivial to implement correctly:
- Every query must pass through the RLS layer — bugs mean cross-tenant data leakage
- The `SET app.current_tenant` middleware pattern requires discipline and automated testing to enforce
- Per-tenant vector DB collections mean separate embedding pipelines per tenant, multiplying compute costs
- Kubernetes on a VPS is a stretch — you typically need at minimum a node per replica for production K8s

**OVHcloud's managed PostgreSQL at the entry level** (€30-50/month) is a shared instance, not a cluster. True multi-tenant isolation with RLS on a shared PostgreSQL instance is achievable, but the operational model needs to be simpler than what's described.

**At launch scale (3-5 pilot clients), you don't need multi-tenancy architecture.** You need:
- A single PostgreSQL schema with a `tenant_id` column and RLS policies (achievable)
- Simple credential isolation per client (API keys in environment variables, not vault)
- No Kubernetes — Docker Compose on a VPS is fine for 3-5 tenants
- No Vault — tenant secrets can live in per-tenant environment configs until you have 50+ tenants

### Alternative

**Simplify the multi-tenancy model for launch. Defer the "hybrid" model until you have revenue.**

For 3-5 pilot clients:
- **Single PostgreSQL instance** with schema-per-tenant (not pool + silo)
- **Docker Compose** (not Kubernetes) on a single VPS
- **No Vault** — use environment variables + a secrets file per tenant, managed manually
- **Per-tenant MCP servers** where needed, but keep them simple
- **pgvector for embeddings** — single instance, shared across tenants with RLS

The "hybrid silo/pooled" model is a good north star for when Alizé has 50+ tenants and needs to offer premium tiers with SLAs. At launch, it's overengineering that delays getting the first clients live.

The research document's own cost table suggests this simplification is already baked in ("Managed PostgreSQL (shared, isolated): €50/month"). The architecture section overcomplicates what the cost section simplifies.

### Recommendation

**Adopt a "scale up, not scale out" approach for the first 12 months.** Single PostgreSQL instance with RLS. Docker Compose on a single VPS (or two: one for the app, one for the database if you want better isolation). Add Kubernetes, Vault, and dedicated namespaces only when you have 25+ paying clients with revenue to support the operational complexity.

The hybrid model is a Year 2 architectural goal. Not a Week 1 requirement.

---

## Summary Table

| Assumption | Current View | Problem | Alternative | Severity |
|------------|-------------|---------|-------------|----------|
| **MCP-first as differentiator** | MCP is Alizé's primary technical/marketing differentiator vs. Agentova | French PME/ETI buyers don't care about protocols — they care about outcomes, time saved, and managed service. MCP adds real complexity for no measurable business value at this audience level. | Lead with "connected tools + managed service + French hosting." Make MCP an implementation detail, not a selling point. | HIGH |
| **4-layer self-improvement architecture** | Four layers of agent learning (Runtime Learning + Skill System + Session Search + Offline Evolution) are part of the launch architecture | Building autonomous skill creation and DSPy optimization traces before validating that anyone will pay for the core product is classic premature optimization. The complexity will delay launch and may be entirely wrong based on real usage data. | Ship Layer 1 (bounded curated memory) only at launch. Add layers incrementally based on production feedback from real clients. | HIGH |
| **Mastra as agent framework** | Mastra (22k stars) chosen because it embeds in Hono and has built-in RAG/memory/workflows/MCP | Mastra is young and unproven in production. The "22k stars" metric conflates GitHub popularity with production readiness. Three nested frameworks (Nuxt + Hono + Mastra) create debugging complexity for a small team. Framework lock-in risk is real. | Consider a lean custom runtime on raw Anthropic/OpenAI SDK, or selective use of LangChain.js primitives. Mastra is better as a contingency than a default. | MEDIUM |
| **Hybrid silo/pooled multi-tenancy on VPS** | Default pooled with RLS, premium siloed. Kubernetes namespaces, Vault for secrets, per-tenant vector DB collections. | The described architecture costs 3-5x more than the €25-40/month VPS budget. Multi-tenancy at launch scale (3-5 clients) doesn't need this complexity. The cost table in the document contradicts the architecture spec. | Single PostgreSQL instance with RLS. Docker Compose (not Kubernetes). Manual secrets management. Scale to K8s + Vault + dedicated namespaces only when revenue justifies it. | HIGH |
| **OVHcloud as AI inference platform** | External LLM APIs (Mistral, Anthropic) for inference. OVH for hosting. | This is actually the right call — no issue here. External APIs avoid GPU complexity and keep costs predictable. | No change needed. This is a strength of the architecture, not a weakness. | — |

---

## Final Verdict

The architecture has **three HIGH-severity issues** that could materially delay Alizé's launch or burn development time on undifferentiated complexity:

1. **MCP positioning** — Remove from marketing. Build it into the architecture where useful, but don't let a protocol be the story.
2. **Self-improvement layers 2-4** — Cut for launch. Ship bounded memory, validate the product, then add sophistication based on real client needs.
3. **Multi-tenancy overengineering** — Simplify to single PostgreSQL + Docker Compose. The hybrid model is Year 2 architecture, not Week 1.

The overall architecture is **directionally sound** — external LLM APIs, Nuxt frontend, OVHcloud hosting, MCP integration layer, pgvector for memory. These are the right choices. The problem is the **scale layer** being designed for 1,000 clients when Alizé needs to launch with 3.

Ship simple. Ship ugly. Ship the core workflow. Then scale the architecture when you have revenue, not before.
