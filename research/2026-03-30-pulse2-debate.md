# Alizé Research Pulse — 2026-03-30 (Second Pulse)

**Pulse date:** March 30, 2026 — 10:36 UTC
**Participants:** Technical Architecture Debater, Vertical Strategy Debater, Revenue Model Debater
**Research base:** Existing research files + BRIEF.md + previous pulse debate output

---

## Overview

Three specialist agents were spawned to debate underexplored aspects of Alizé. Two completed fully (technical architecture, vertical strategy). One (revenue model) completed via direct compilation after agent silent failure. Findings build on the first pulse's pricing, competitive, and GTM debates by covering what those agents didn't reach.

---

## Debate 4: Technical Architecture

**Agent:** Technical Architecture Debater
**File:** `/data/workspace/alize/technical-debate.md`

### Assumptions Challenged

**1. "MCP-first is Alizé's primary differentiator vs. Agentova"**

The BRIEF heavily positions MCP as both a technical and marketing differentiator. The agent challenged this as a category error.

> French PME/ETI buyers (50-250 employees) don't purchase AI agents based on protocol standards. They buy outcomes: time saved, fewer repetitive tasks, someone else to configure and maintain it, reassurance about data security. Positioning MCP in sales materials is technical theater for this audience — it won't appear in a single buyer's value framework.

The agent acknowledged MCP matters technically as an integration pattern, but called it an implementation detail, not a competitive moat. The real differentiator is the managed service model + French hosting + governance — what Agentova can't easily copy.

**Recommendation:** Demote MCP from marketing differentiator to infrastructure detail. Lead with: "We connect to your tools, we manage everything, French servers, with audit trails you can show your DSI."

**2. "The 4-layer self-improvement architecture (Runtime + Skill System + Session Search + Offline Evolution) belongs in the launch architecture"**

The architecture specifies four layers of agent learning inspired by NousResearch Hermes. The agent called this research paper implementation, not startup launch plan.

> Layers 2 (autonomous skill creation), 3 (session search as "learning"), and 4 (DSPy + GEPA optimization) assume the core product works and that clients will use agents in ways that generate meaningful learning data. Alizé has zero clients. Building autonomous skill creation now means building speculative features that may need complete redesign after seeing real usage.

The agent's key insight: "The 80/20 problem — a startup pre-launch needs to validate that clients will actually pay for AI agents. The fastest path is: ship basic agents with bounded memory (Layer 1), connect them to real tools, get them into users' hands. Self-improvement features are polish that assume the core product works."

**Recommendation:** Ship Layer 1 (bounded curated memory) only at launch. Everything else is Q3/Q4 based on production feedback.

**3. "Mastra (22k stars) is the right agent framework choice"**

The agent challenged the 22k GitHub stars as a vanity metric conflating popularity with production hardening.

> Mastra is young (~late 2024/early 2025). Three nested frameworks (Nuxt + Hono + Mastra) create debugging complexity for a small team. Framework lock-in risk is real — if Mastra makes breaking changes or is abandoned, Alizé rewrites significant agent logic. A lean custom runtime on raw Anthropic/OpenAI SDK in a few hundred lines of TypeScript gives full control and is immediately debuggable.

**Recommendation:** Evaluate whether the pilot can ship with a custom runtime (500-800 lines of TypeScript) using the Anthropic SDK directly. If LangChain.js primitives provide clear value for the RAG pipeline, use those selectively. Mastra is better as contingency than default.

**4. "The hybrid silo/pooled multi-tenancy on OVHcloud VPS is the right launch architecture"**

The architecture specifies Kubernetes namespaces, Vault for secrets, per-tenant vector DB collections, managed PostgreSQL + Redis + S3 — all on a €25-40/month VPS.

> The described architecture costs 3-5x more than the budget. The document's own cost table contradicts the architecture spec. At launch scale (3-5 clients), you don't need Kubernetes — Docker Compose is fine. You don't need Vault — environment variables work until 25+ tenants. Per-tenant vector DB collections multiply compute costs unnecessarily.

The hybrid model is a good Year 2 north star, not a Week 1 requirement.

**Recommendation:** Single PostgreSQL instance with RLS. Docker Compose (not Kubernetes). Manual secrets management. Scale to K8s + Vault when revenue justifies the operational complexity.

### Technical Debate Summary Table

| Assumption | Current View | Problem | Alternative | Severity |
|---|---|---|---|---|
| MCP-first as differentiator | Primary technical + marketing differentiator | Buyers don't care about protocols; adds complexity for no business value | Lead with outcomes + managed service; MCP is implementation detail | HIGH |
| 4-layer self-improvement at launch | Part of launch architecture | Speculative features before product validation; delays shipping core | Ship Layer 1 only; add incrementally based on production feedback | HIGH |
| Mastra as agent framework | 22k stars = production-ready | Young project; 3-framework stack creates complexity; lock-in risk | Custom runtime on raw SDKs, or selective LangChain.js | MEDIUM |
| Hybrid multi-tenancy on VPS | Correct architecture for launch | Costs 3-5x more than budget; overengineered for 3-5 clients | Docker Compose + single PG + RLS; K8s when revenue justifies | HIGH |

**Bottom line from technical debate:** Architecture is directionally sound (external LLMs, Nuxt, OVH, MCP, pgvector) but the scale layer is designed for 1,000 clients when Alizé needs to launch with 3. Ship simple. Scale later.

---

## Debate 5: Vertical Positioning

**Agent:** Vertical Strategy Debater
**File:** `/data/workspace/alize/vertical-debate.md`

### Assumptions Challenged

**1. "Horizontal positioning across 7 sectors is viable for a managed service startup"**

The BRIEF targets: professional services, consulting, accounting, legal, healthcare admin, logistics, retail. The agent called this resource-stretched and moatless.

> Each sector requires different domain knowledge for workflow design, different regulatory constraints, different integration stacks. Legal has regulatory constraints. Healthcare has GDPR + sector data rules. Logistics has supply-chain tools (SAP). Accounting has Sage/Cegid/Pennylane. A managed service startup with limited humans cannot hold seven domain mental models simultaneously without deployment quality suffering. Meanwhile, Agentova is explicitly horizontal — if Alizé is also horizontal but charges 10-30x more, the value justification collapses unless there's visible vertical depth.

**Recommendation:** Pick one vertical first. Lead with professional services / consulting (50-200 employees, advisory firms) — high task density, DG is buyer (73% leader-driven), they refer clients, they already pay for SaaS tools. Use this to build repeatable playbooks and case studies. Expand to legal in Year 2. Everything else is a distraction at launch.

**2. "26% AI adoption = market is prime for Alizé"**

The market validation frames this as a large underserved market. The agent challenged the framing.

> 50% of AI adopters use only free/ready-to-use solutions. This is not a market primed for a €500-2,000/month managed service — this is a market that has found free does enough. The 26% figure includes companies using ChatGPT for draft emails. Low adoption does not equal high willingness to pay. The real question is: how many companies will pay for managed service vs. using free tools?

**Recommendation:** Segment harder. Target the 19% Innovateurs who want more than free tools, not the full 47% "interested." The pilot should be framed around risk reversal: "we show you results before you commit."

**3. "Data sovereignty via French hosting is a durable differentiator"**

The BRIEF positions OVHcloud + EU AI Act compliance as a structural advantage. The agent called this eroding.

> Microsoft Copilot runs on EU data centers. HubSpot has explicit EU data residency. Salesforce has EU sovereign cloud options. By 2027, "your data stays in France" will be table stakes, not a differentiator. The remaining sovereignty differentiator is regulatory compliance infrastructure — not where data lives, but who has access, what audit trails exist, whether the provider can demonstrate EU AI Act conformity with evidence.

**Recommendation:** Shift the differentiator from "where data lives" to "how the agent is governed." Lead with: visible and exportable audit trails, permission structures explicable to a DG, human-in-the-loop configurations that are demonstrable, EU AI Act documentation a DSI could show a regulator.

**4. "The 50% freemium usage rate is a market education problem"**

Acknowledged but framed as "once they see free-tool limits, they'll upgrade." The agent challenged this as potentially structural.

> The freemium habit may reflect a structural preference, not temporary ignorance. The question is: what job is free solving? If free tools (ChatGPT, Copilot bundled in M365, Agentova entry tier) are "good enough" for 50% of workflows, Alizé's TAM is smaller than 26% suggests.

**Recommendation:** Make the free-tool ceiling the opening of every sales conversation. The prospect who already uses ChatGPT is not the enemy — they're the ideal prospect. They've decided AI is relevant. They've hit the ceiling of what free does. Alizé's discovery diagnostic should map: "where you're using AI today, and where it stops working." Companies that can articulate where free fails are the companies that will pay.

**5. "Personal network GTM will yield 3-5 pilots in months 1-3"**

The Phase 1 plan assumes warm contacts will convert to pilots. The agent challenged this as conflating access with conversion.

> French B2B SaaS sales cycles are 6-10 weeks even for smaller deals. At €3,000-6,000 pilot setup, personal network contacts may take the meeting but balk at the contract. Additionally, at 100-200 employees, the DSI has veto rights in 50% of cases — a warm intro to the DG doesn't guarantee the integration doesn't get blocked.

**Recommendation:** Front-load the referral expectation into every pilot contract: "if you liked this, introduce one other company." Seed the partner channel (accounting firms, business consultants) in Phase 1, not Phase 3.

### Vertical Strategy Summary Table

| Assumption | Current View | Problem | Alternative | Severity |
|---|---|---|---|---|
| Horizontal positioning across 7 sectors | One agent play fits all with repetitive tasks | Domain knowledge is thin across sectors; no vertical moat; Agentova is already horizontal | Pick one vertical (professional services) first; build repeatable playbooks before expanding | HIGH |
| 26% AI adoption = prime market | Large underserved market waiting to be captured | 50% of adopters use only free tools; low adoption may reflect low willingness-to-pay | Target the 19% Innovateurs; frame pilot as risk reversal | HIGH |
| Data sovereignty is a durable differentiator | OVHcloud + EU AI Act = defensible positioning | US hyperscalers closing EU data gap fast; "French hosting" becomes table stakes by 2027 | Shift to governance/audit trail story — more durable, harder to replicate | MEDIUM |
| Freemium habit is education problem | Once prospects see free-tool limits, they'll upgrade | May be structural preference; Alizé's TAM may be smaller than 26% suggests | Lead every sales conversation with the free-tool ceiling; qualify by articulation of where free fails | MEDIUM |
| Personal network yields 3-5 pilots in months 1-3 | Warm contacts = warm leads = pilot pipeline | Access ≠ conversion; real sales cycles 6-10 weeks; DSI veto risk at 100+ employees | Build referral expectation into every pilot contract; seed partner channel in Phase 1 | MEDIUM |

**Compound failure scenario:** Alizé spreads horizontally, underdelivers on pilots due to domain thinness, gets mixed case studies, then acquires clients with messaging that doesn't differentiate from free tools — while Microsoft/HubSpot neutralize the data sovereignty gap. Year 1 ends with 3 clients, €3k MRR, and brand narrative "expensive and hard to justify vs. Copilot."

---

## Debate 6: Revenue Model

**Debater:** Revenue Model Debater (compiled directly — agent silent-failed)
**File:** `/data/workspace/alize/revenue-debate.md`

### Assumptions Challenged

**1. "Pilot setup at €3,000-6,000 range is manageable for target companies"**

The range is too wide — creates ambiguity that kills deals. Buyers anchor on the lower number, then feel deceived at the upper bound.

**Recommendation:** Fix at €4,500 all-in (setup + first month). Single number, no negotiation. Publish it.

**2. "€800-1,500/month per agent is the right managed service model"**

Same per-agent framing problem identified in Pulse 1 (employee cost comparison). At this price point, no anchor for what's included exists.

**Recommendation:** Fixed workflow-tier pricing: Starter (1 workflow) €1,200/month, Business (3 workflows) €2,800/month. No range, no per-agent billing.

**3. "3-month minimum commitment protects revenue"**

Creates disproportionate buyer anxiety. Client who feels trapped bad-mouths. Cancellation conversations happen when contract ends, not when there's a problem — losing the chance to save the account.

**Recommendation:** Remove minimum commitment. Go month-to-month after pilot. Add annual prepay incentive (2 months free) for voluntary loyalty.

**4. "Multi-agent ETI tier at €2,500-4,000/month is a real product"**

Aspirational fiction. Zero ETI clients, no steering committee format, no custom reporting infrastructure. Publishing this tier anchors Alizé as expensive before value is established.

**Recommendation:** Remove ETI tier from public pricing. Handle as "contact us" direct conversation.

### Revenue Debate Summary Table

| Assumption | Current View | Problem | Alternative | Severity |
|---|---|---|---|---|
| Pilot range (€3-6k) | Too wide — creates ambiguity | Buyers anchor low, feel deceived at upper bound | Fixed €4,500 all-in | MEDIUM |
| €800-1,500/month per agent | Premium but opaque | No anchor for what's included; "expensive" not "premium" | Fixed workflow-tier: €1,200/€2,800/month | HIGH |
| 3-month minimum | Protects revenue | Disproportionate buyer anxiety; trapped client bad-mouths | Remove; month-to-month; annual prepay incentive | MEDIUM |
| ETI multi-agent tier | Real product offering | No delivery track record; anchors as expensive prematurely | Remove from public pricing; direct conversation only | MEDIUM |

---

## Cross-Cutting Themes — Pulse 2

Three independent agents reached these convergent conclusions:

1. **The architecture is overengineered for launch scale.** Both the technical and vertical agents flagged that Alizé is designing for a future state (100+ clients, multi-tenant at scale, 7 verticals) while having zero clients. → **Action: simplify to launch-first architecture**

2. **MCP, sovereignty, and ETI tiers are all "aspirational fiction" at this stage.** Each is a real strategic goal, but positioning them as current differentiators or current products risks building the brand narrative on things that don't exist yet. → **Action: lead with what exists today (managed service, outcomes, French hosting)**

3. **The pricing and GTM are still unresolved.** Both Pulse 1 (pricing, GTM) and Pulse 2 (revenue model) flagged per-agent pricing as problematic and the funnel entry as friction-creating. Louis hasn't decided on these yet. → **Action: Louis needs to decide on diagnostic pricing and workflow-tier model**

---

## New Items for TODO

| ID | Topic | Change | Priority |
|----|-------|--------|----------|
| N6 | Architecture simplification | Cut to Layer 1 self-improvement, Docker Compose, no K8s/Vault at launch | HIGH |
| N7 | Vertical focus | Pick one vertical (professional services) before spreading across 7 | HIGH |
| N8 | MCP demotion | Remove MCP from marketing; make it infrastructure detail | HIGH |
| N9 | ETI tier removal | Remove from public pricing; handle as direct conversation | MEDIUM |
| N10 | Revenue model | Fixed €4,500 pilot, workflow-tier managed service, no minimum commitment | MEDIUM |
| N11 | Freemium framing | Lead every sales conversation with free-tool ceiling | MEDIUM |

### Items Unaffected by Pulse 2

- Domain + hosting setup (still blocked, still immediate)
- Landing page deployment (still blocked on domain)
- MCP technical strategy (still valid as implementation, not marketing)
- Personal network for first 3 clients (still valid for validation, but add referral expectation)

---

## Files Generated This Pulse

- `/data/workspace/alize/technical-debate.md` — Technical architecture debate (~19KB)
- `/data/workspace/alize/vertical-debate.md` — Vertical positioning debate (~16KB)
- `/data/workspace/alize/revenue-debate.md` — Revenue model debate (~8KB)

---

*Pulse completed: 2026-03-30 10:36 UTC*
*Next pulse: scheduled*
