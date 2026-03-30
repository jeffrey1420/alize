# Alizé Research — Pulse 12 Debate
**Date:** 2026-03-30
**Topic:** Delivery readiness, cold outbound prerequisites, and infrastructure assumptions
**Agents:** 3 specialist agents

---

## What Was Debated

Three independent specialist agents were spawned to challenge assumptions in three areas:

1. **GTM Readiness (Cold Assets)** — What must exist before Alizé starts cold outbound in Month 2?
2. **Delivery Stack (Infrastructure)** — Is n8n + OVHcloud the right delivery infrastructure for Month 1?
3. **Delivery Partner (Sourcing)** — Where to find a delivery partner, how to contract, and whether alternatives are better?

---

## Agent 1: GTM Readiness — Cold Outbound Month 2 Prerequisites

**Challenge:** The assumption "website + case study = ready for cold outbound" is dangerously incomplete.

### Challenged Assumption
D40 says "build cold-outbound-ready assets in Year 1" but never defines what "cold-outbound-ready" means concretely.

### Key Findings

**4 Hard Blockers — Cold outbound does NOT start without these:**

1. **Live website at alize.studio** — Hero clearly states what/who/for, functional diagnostic CTA, pricing visible, no 404s. Domain is not bought yet. This is the first blocker and it is not cleared.

2. **One completed pilot with documented results** — Real client, real workflow, real measurable outcome, client permission for reference (anonymized acceptable). No pilots exist yet. This is Month 1 work.

3. **ICP confirmed and locked** — Vertical defined (professional services, consulting firms), company size range confirmed, decision-maker title confirmed, 100+ target companies identified. U12 (first vertical) is still unresolved — you cannot sell to "French PME" generically.

4. **Delivery capability confirmed** — Delivery partner identified OR Louis role documented, n8n set up and tested OR manual execution path written, scope document + exit criteria drafted. U32 (delivery partner) and U33 (Louis role) are both unresolved.

**5 Required Assets — must exist, can be rough:**
- Cold email template (problem-first, no jargon, in French)
- LinkedIn company page live + Louis profile active + 3+ posts
- One-pager / leave-behind PDF
- Pricing finalized (€4,500 pilot / €1,200-1,500 / €2,000-2,500 managed)
- Technical execution path demonstrated end-to-end

**On warm-vs-cold sequence:** These run in parallel, not sequence. Warm network generates the first case studies. Cold outreach starts Month 2 only after blockers are cleared. Waiting for warm network to "exhaust" creates a pipeline gap. Waiting for perfect assets means never starting.

**Month 2 cold outreach viability:** Cold outbound is viable IF Month 1 produces: 1 pilot completed, website live, ICP locked. If Month 1 doesn't produce these, push cold outbound to Month 3.

### Challenged Assumption
That "cold outbound" and "warm outreach" are sequential phases. Wrong. Both run from Month 1 onward. Warm generates the proof. Cold scales the pipeline. They feed each other.

---

## Agent 2: Delivery Stack — n8n + OVHcloud Challenged

**Challenge:** n8n MCP is not production-stable. Self-hosted n8n creates ops burden Louis can't sustain. Month 1 doesn't need n8n at all.

### Challenged Assumptions

**Assumption 1: "n8n MCP is production-ready"**

Reality: As of March 2026, n8n has open, active MCP bugs:
- Issue #26394 (open): MCP tool calls fail in queue mode with `Tool node "" does not have supplyData method` errors and worker timeouts. Queue mode is the standard production deployment.
- Issue #27718 (open): `get_workflow_details` MCP tool consistently times out after 60 seconds.
- The n8n docs page for MCP doesn't even exist (404 when fetching `/mcp/`).

n8n's MCP integration is in beta/early-access state, not production-stable. Building Alizé's managed service delivery on a beta MCP integration is a significant risk. One failed CRM sync for a client pilot = a bad case study.

**Assumption 2: "Self-hosted n8n is free"**

Reality: Hidden costs for a managed service:
- Incident response: When n8n goes down at 2 AM, Louis gets the call. Alizé's positioning promises reliability but the "next business day SLA" doesn't match 24/7 infrastructure reality.
- Maintenance burden: n8n updates, security patches, database maintenance, worker restarts — all Louis's problem.
- Security surface: n8n workflows handle credentials to client CRM, email, and document systems.
- Two systems to operate: The project already has Hono + Mastra backend. Adding n8n means Louis runs and monitors two complex systems.

**Assumption 3: "OVHcloud is the right host"**

OVHcloud is chosen for French data sovereignty, but Coolify is already set up (per TOOLS.md: `https://admin.lschvn.foo`). Using Coolify on any EU VPS achieves the same sovereignty positioning without the manual ops burden. OVHcloud VPS requires Docker Compose updates, server security hardening, monitoring setup — all manual. Coolify automates this.

**Assumption 4: "n8n is needed for Month 1 delivery"**

The existing stack (Hono + Mastra + pgvector + BullMQ) is already capable of:
- Document RAG (working pipeline: upload → parse → chunk → embed → search)
- Agent execution with tools
- SSE streaming to frontend

What n8n adds for Month 1: MCP integrations (but MCP is beta and unstable), visual workflow builder (useful for complex workflows but Month 1 pilots are 1 workflow, 2-4 tools), pre-built connectors (only useful at scale).

What n8n costs: Learning curve, infrastructure setup, debugging queue mode bugs, operating two systems.

**Alternative for Month 1:** Use Mastra's built-in tools + direct API calls for specific integrations the pilot client needs. HubSpot has a REST API. Gmail has the Gmail API. These are 10-line additions to the Mastra agent, not a separate orchestration platform.

### The Meta-Challenge
Infrastructure decisions (OVHcloud, n8n, Docker Compose → K8s, MCP-first) are treated in the brief with the same authority as product decisions (managed service, French positioning, pricing tiers). The pilot can run on existing infrastructure. The tooling debate should be resolved after the first 2-3 pilots, when Louis knows which integrations actually get used and which workflows fail.

---

## Agent 3: Delivery Partner — Sourcing, Contracting, Budget

**Challenge:** Before sourcing externally, check if Gabin can serve as delivery lead. Hiring into an empty pipeline is wrong. The delivery partner question is secondary to client acquisition.

### Delivery Partner Profile

Hard requirements:
- French native speaker
- PME/ETI experience (50-200 person companies)
- Process mapping capability
- Change management instincts
- Availability: 2-3 days/week for Month 1

The delivery partner's job is NOT to build agents. It's to:
1. Run the client discovery call (not the sales call — the delivery scoping call)
2. Map the client's actual operational workflow
3. Identify which workflow to automate first
4. Manage client expectations and stakeholder resistance
5. Measure and report ROI in business terms
6. Write the delivery playbook with Louis as technical input

Louis's job is: execute the technical build, test and validate the agent, handle integration issues, monitor post-deployment.

### 5-Step Sourcing Plan

**Step 1: Confirm Gabin's Role (Day 1-3)**
Ask Gabin directly: does he have client-facing business consulting or operations experience with PME/ETI companies?
- If yes: Formalize Louis + Gabin as delivery team. Skip external hiring for Month 1.
- If no or unclear: Proceed to Step 2.

**Step 2: Warm Outreach on Malt (Day 3-10)**
Search filters: "Transformation digitale PME" + "Conduite du changement" + "Amélioration des processus" + Île-de-France + disponible maintenant. Send 20 personalized messages. Expected response rate: 10-20% (2-4 replies, 1-2 viable candidates).

**Step 3: Interview + Reference Check (Day 10-17)**
30-minute video call with each viable candidate. Always call 1-2 references from past PME clients.

**Step 4: Trial Engagement Negotiation + Contract (Day 17-21)**
- Duration: 30 days (pilot period)
- Scope: Up to 5 delivery days
- Base rate: €350/day (40% below market)
- Success fee: €1,500 paid at client conversion (signs ≥3-month managed service)
- Exit clause: Either party can terminate with 5 days notice

**Step 5: Month 1 Pilot Delivery with Shadowing (Day 21-55)**
Clear role division: delivery partner runs discovery, Louis builds agent, delivery partner manages change management and conversion.

### Budget Validation

| Item | Cost |
|------|------|
| Infra | €25-40/month |
| Delivery partner (1 pilot, 5 days at €350/day) | €1,750 base + €1,500 success fee = €3,250 if converted |
| Total Month 1 burn | €1,775-3,290 |

Against €4,500 pilot revenue: margin positive in both scenarios.

### Alternatives Assessed

| Alternative | Speed | Cost | Quality | Scalability | Verdict |
|------------|-------|------|---------|-------------|---------|
| Delivery partner (freelance, success-fee) | Medium | Low-Medium | High | Medium | Primary Month 1 path |
| Louis + Gabin formal partnership | Fast | None | High if Gabin has consulting skills | Low-Medium | Try first |
| Boutique firm white-label | Slow | High margin loss | Medium | High | Month 3-6 play |
| Discounted pilot by Louis alone | Fastest | None | Low-Medium | None | Fallback only |

**Key insight:** The delivery partner question is secondary to client acquisition. Every day spent sourcing a delivery partner before a client is committed is wasted. Get the first pilot client first, then staff the delivery partner against a concrete engagement.

---

## Decisions Reached

| ID | Topic | Decision | Source |
|----|-------|----------|--------|
| D45 | Month 1 delivery stack | Skip n8n for Month 1. Run first pilot(s) on existing Mastra stack + direct API integrations. Defer n8n/OVHcloud decision to Month 3. | Delivery stack agent |
| D46 | n8n MCP stability | n8n MCP is NOT production-stable as of March 2026. Open bugs in queue mode make it unsuitable for Alizé's managed service delivery. Do not build delivery infrastructure on n8n MCP until bugs are resolved. | Delivery stack agent |
| D47 | Cold outbound blockers | 4 hard blockers defined: (1) live website, (2) completed pilot with documented results, (3) ICP confirmed, (4) delivery capability confirmed. 5 required assets also defined. Cold outbound does not start without blockers cleared. | GTM readiness agent |
| D48 | Delivery partner sequencing | Check Gabin's consulting background first (Day 1-3). If he qualifies, Louis + Gabin deliver Month 1 together. If not, begin Malt outreach for external delivery partner. | Delivery partner agent |
| D49 | Delivery partner contract | Success-fee model: €350/day base + €1,500 conversion bonus. Total exposure capped at €3,250 if pilot doesn't convert. Against €4,500 pilot revenue, pilot economics hold. | Delivery partner agent |
| D50 | Infrastructure decision timing | Most infrastructure decisions (OVHcloud vs alternatives, n8n vs Make.com, Docker Compose vs K8s) can wait until Month 3. The existing stack is sufficient for Month 1 pilots. Focus Month 1 on delivery validation, not infrastructure optimization. | Delivery stack agent |

---

## New Unresolved Items

| ID | Topic | Blocker |
|----|-------|---------|
| U41 | Gabin's consulting background | Must confirm before sourcing external delivery partner |
| U42 | Domain purchase | First cold outbound blocker — not yet bought |
| U43 | First pilot client | Must be identified before delivery partner engagement |
| U44 | Month 1 pilot scope | Which workflow, which client, which tools — must be defined before delivery starts |
| U45 | n8n vs Make.com | Deferred to Month 3 — don't decide now |
| U46 | OVHcloud vs other EU VPS | Deferred to Month 3 — Coolify already handles deployment |

---

## Cross-Cutting Tensions

| Tension | Resolution |
|---------|------------|
| Delivery partner vs Louis solo | Try Louis + Gabin first; if Gabin lacks consulting skills, hire external |
| n8n vs direct API for Month 1 | Direct API only — n8n MCP is not stable enough |
| Cold outbound Month 2 vs deferring | Blockers must clear first; Month 2 is the target but Month 3 is acceptable |
| Infrastructure decisions now vs later | Defer all infrastructure decisions to Month 3 — existing stack is sufficient |

---

*Pulse 12 — 2026-03-30*
