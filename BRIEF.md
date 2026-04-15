# BRIEF — Alizé
**Version:** 2.1 — Workflow-First Positioning Update
**Date:** 2026-04-15
**Status:** Master product document
**Changes from v2.0:** Reframed positioning around Anthropic workflow-first taxonomy. Added workflow pattern catalog aligned to Alizé use cases. Updated key message and differentiation language.

---

## Identity

**Name:** Alizé
**Domain:** alize.studio (pending)
**Positioning:** French managed AI workflow service for businesses. We design, deploy, and operate structured AI workflows — connected to real business tools, governed, and measured.

**Core message:** "Nous ne vendons pas des agents IA. Nous délivrons des workflows IA opérationnels, gouvernés et mesurables."

**What Alizé is:** A managed service that designs and runs AI workflows for French businesses. One workflow, one measurable outcome, deployed and maintained by us.

**What Alizé is not:** a prompt agency, a chatbot vendor, a self-service SaaS, a promise to replace teams.

---

## Why "Workflows" Not "Agents"

**Anthropic's taxonomy (April 2026) defines the distinction clearly:**

- **Workflows:** LLMs and tools orchestrated through **predefined code paths**. Predictable, consistent, bounded. Suitable for well-defined, measurable tasks.
- **Agents:** LLMs that **dynamically direct their own processes** and tool usage. Higher capability, higher cost, higher risk. Suitable for open-ended problems.

**The insight:** Most business AI use cases are workflows, not agents. The most successful production deployments use simple composable patterns — not autonomous agents operating without bounds.

**What this means for Alizé:** We position as a workflow-first service. We design bounded, governed AI workflows around specific business tasks. We only introduce agentic autonomy where the use case genuinely requires it and the risk is acceptable. This is fundamentally different from generic AI platforms that sell "agents" as a concept without respecting the workflow/agent distinction.

**Why this matters to buyers:**
- "Workflow" is concrete and measurable. "Agent" sounds like science fiction.
- Enterprises have governance requirements that workflows satisfy more easily than autonomous agents.
- Regulated industries (finance, insurance, healthcare) need predictable, auditable AI — workflows deliver that.
- French data sovereignty is baked into the architecture, not marketed as a feature.

---

## Anthropic Workflow Patterns — Alizé Catalog

Anthropic's six workflow patterns map directly to Alizé's target use cases:

### 1. Prompt Chaining
**Pattern:** Sequential steps, each LLM call processes the output of the previous. Programmatic gate checks between steps.
**Alizé use case:** Invoice processing — extract → validate → categorize → route → archive. Each step is a separate LLM call with validation.
**Governance fit:** High. Each step can be logged, each gate can require human approval.

### 2. Routing
**Pattern:** Classify input, direct to specialized follow-up task. Build specialized prompts per category.
**Alizé use case:** Customer inquiry routing — categorize ticket (refund / technical / billing / other), then dispatch to correct workflow. Easy queries handled by one workflow, complex by another.
**Governance fit:** High. Routing decisions are transparent and auditable.

### 3. Parallelization
**Pattern:** Break task into independent subtasks run simultaneously (sectioning), or run same task multiple times for diverse outputs (voting).
**Alizé use case:** Contract review — one LLM reviews for compliance, another for financial terms, another for legal risk, then aggregate. Or: three LLM calls review the same document, majority verdict decides.
**Governance fit:** High. Parallel review provides audit trail and reduces single-point-of-failure risk.

### 4. Orchestrator-Workers
**Pattern:** Central LLM dynamically breaks down tasks and delegates to worker LLMs. Subtasks aren't predefined — determined by the input.
**Alizé use case:** Research + reporting — orchestrator decides what to search, delegates data gathering to workers, synthesizes final report. Used for competitive intelligence, market research, due diligence.
**Governance fit:** Medium. Requires careful scope definition to avoid unbounded task expansion.

### 5. Evaluator-Optimizer
**Pattern:** One LLM generates, another evaluates in a loop. Iterative refinement with clear evaluation criteria.
**Alizé use case:** Content drafting — generate marketing copy, evaluator reviews for brand voice and factual accuracy, generator revises, repeat until approved. Also: business report drafting, proposal refinement.
**Governance fit:** High. Clear human-definable criteria. Loop can be capped at N iterations.

### 6. Agents
**Pattern:** LLM dynamically directs its own processes and tool usage over multiple turns. High autonomy, high cost, high risk. Requires sandboxing and guardrails.
**Alizé use case:** Open-ended coding tasks, complex multi-tool automation where the number of steps cannot be predicted. Used only in trusted environments with strict scope.
**Governance fit:** Low — requires significant guardrails. Alizé deploys agents only with human-in-the-loop checkpoints and hard stopping conditions.

---

## Vision

AI in business is often poorly sold: too many buzzwords, too many demos, not enough framing. Alizé exists to transform AI into a usable operational tool — starting from real business needs, not technological fantasies.

The French PME/ETI market is underserved by AI: global platforms are generic, self-service tools require technical maturity most businesses don't have, and "AI strategy consulting" produces decks, not working workflows.

Alizé fills the gap: a managed, governed, professionally deployed AI workflow service built for French companies that want real results, not another dashboard.

---

## Market Sizing

### French Business Landscape

| Segment | Count | Employees | Alizé Relevance |
|---------|-------|-----------|-----------------|
| **PME** | ~3.6M | 10-249 | Primary target |
| **ETI** | ~5,000 | 250-4,999 | Secondary target |
| **Grandes entreprises** | ~220 | 5,000+ | Not targeted |
| **Micro-entreprises** | ~2.5M | <10 | Too small |

### Addressable Market

**Primary sweet spot:** 50-250 employees — enough operational complexity for meaningful repetitive tasks, enough budget for professional services.

**Estimated addressable PME/ETI in France:** ~50,000-80,000 companies.

---

## What Alizé Sells

A **managed AI workflow service** with professional design, deployment, and ongoing operation.

1. **Audit & Scoping** — High-impact use case identification, tool mapping, friction quantification
2. **Workflow Design** — Selecting and composing Anthropic patterns around specific business tasks
3. **Technical Installation** — Deploying on French-hosted infrastructure, connecting to business tools via MCP
4. **Guardrails & Governance** — Access controls, validation gates, human-in-the-loop checkpoints, audit logging
5. **Knowledge Base / RAG** — Document ingestion, retrieval optimization
6. **Business Testing** — Real scenarios, real data, real teams before go-live
7. **Team Training** — Getting teams operational with the workflow, not the technology
8. **Monitoring & Maintenance** — Tracking performance, alerting on failures, optimizing prompts
9. **Continuous Optimization** — Monthly review, workflow adjustments, new use cases

---

## Priority Use Cases (Workflow-Mapped)

### Tier 1 — Immediate fit

1. **Customer Service — Routing + Prompt Chaining**
   - Route tickets: refund / technical / billing / other
   - Simple Q&A from knowledge base (RAG)
   - Response drafting → human approval → send

2. **Invoice Processing — Prompt Chaining**
   - Extract → validate → categorize → route to accounting
   - Payment reminder drafting

3. **Lead Qualification — Routing + Evaluator-Optimizer**
   - Score and qualify inbound leads
   - Generate personalized first response
   - Evaluator reviews for accuracy before CRM update

4. **HR Internal Q&A — Routing + RAG**
   - Policy questions → correct document
   - Leave requests → workflow approval
   - Onboarding guidance → knowledge base

### Tier 2 — Near-term fit

5. **Meeting Briefs — Orchestrator-Workers**
   - Gather context from CRM, email, calendar
   - Generate structured meeting brief
   - Worker synthesizes final document

6. **Contract Review — Parallelization + Evaluator-Optimizer**
   - Parallel review: compliance, financial, legal
   - Aggregated risk score
   - Iterative refinement until threshold met

7. **CRM Data Enrichment — Parallelization**
   - Multiple enrichment calls run simultaneously
   - Results aggregated into CRM record

---

## Competitor Analysis

### Agentova 🇫🇷
**Positioning:** "AI agents that work while you sleep"
**Model:** Self-service SaaS marketplace with pre-built agents
**Weaknesses:** No governance layer, no workflow framing, generic agents not tailored to business, self-service requires effort
**Alizé's counter:** We design workflows for your specific business, we deploy and manage them, you get results not software

### Wonderful 🇫🇷
**Focus:** Customer service AI agents. €134M raised. Threat level: Low (different scope)

### Magic AI (US)
**Focus:** AI employees for SMBs. Similar positioning, US-based, no French presence
**Threat level:** Medium — awareness competitor, not displacement

### GPT-5 Enterprise (OpenAI)
**New threat as of April 2026.** Gmail/Calendar/Google Drive integration. Back-office agent automation.
**Alizé's counter:** Vertical depth, governance layer, French data sovereignty, faster deployment than enterprise sales cycles

### Mistral AI
**Role:** Partner. Voxtral TTS open-weight. Alizé runs on Mistral for French infrastructure sovereignty.

---

## Pricing Recommendations

| Offer | Price |
|-------|-------|
| Diagnostic | €490 (one-time, credited to deployment) |
| Pilot Deployment | €3,000-6,000 setup |
| Managed Service | €800-1,500/month per workflow |
| Multi-Agent (ETI) | €2,500-4,000/month, up to 5 workflows |

---

## Technical Architecture

**Stack:** Hono + Mastra + pgvector on OVHcloud. MCP-first integration layer.

**MCP is critical:** Model Context Protocol (Anthropic's open standard) connects agents to business tools. Every integration is an MCP server. Skills follow agentskills.io open standard.

**Workflow engine:** Mastra implements Anthropic's workflow patterns natively. Routing, parallelization, prompt chaining, evaluator-optimizer, orchestrator-workers — all built in.

**Infrastructure:** French hosting on OVHcloud. Per-client database isolation. EU AI Act-native.

---

## Key Differentiators

| | Agentova | GPT-5 Enterprise | Alizé |
|--|---------|-----------------|-------|
| **Model** | Self-service SaaS | Generic enterprise AI | Managed workflow service |
| **Workflow patterns** | No | Implicit | Explicit — Anthropic taxonomy |
| **Governance** | Not featured | Not featured | Core differentiator |
| **French sovereignty** | Not featured | No | Yes — OVHcloud + Mistral |
| **EU AI Act** | Generic | Generic | Native compliance |
| **Deployment** | Client self-service | 3-6 month sales cycle | Weeks |
| **Human oversight** | Optional | Optional | Configurable per workflow |
| **Target** | Micro-SMB | Large enterprise | PME/ETI 50-250 emp |

---

## Brand Tone

Clear, direct, reassuring, serious, modern, business. Never arrogant, never geeky, never buzzword-heavy.

**Target style:** Understated, premium, credible, result-oriented, pragmatic.

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-03 | Initial brief |
| 2.0 | 2026-03-30 | Comprehensive: competitor analysis, market sizing, pricing, technical architecture, MCP positioning, go-to-market, objections |
| 2.1 | 2026-04-15 | Workflow-first reframe: Anthropic taxonomy integrated, workflow pattern catalog added, "agents" language replaced with "workflows" throughout, key message updated |

---

*This is the master Alizé product document. All other Alizé materials should be consistent with this brief.*
