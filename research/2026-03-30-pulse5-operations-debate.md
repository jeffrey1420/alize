# Operations Debate: Can 2 Developers + 1 Designer Actually Deliver This?

**Date:** 2026-03-30  
**Role:** Operations Strategist (Pulse 5)  
**Team:** Louis + Gabin (dev), Maëli (designer)  
**Context:** Alizé managed AI agent service, French PME/ETI target

---

## The Central Question

The BRIEF presents Alizé as a managed AI agent service that requires:

**Service delivery:**
- Client discovery & sales
- Workflow design per client
- Agent deployment & configuration
- Training & onboarding
- Monthly monitoring
- Prompt tuning & workflow adjustments
- Monthly performance reviews
- Ongoing support

**Product building:**
- Nuxt 3 frontend
- Hono + Mastra backend
- pgvector RAG pipeline
- MCP server integrations
- Docker Compose → K8s deployment
- 4-layer self-improvement architecture
- Real-time SSE, Redis pub/sub, BullMQ jobs
- Multi-tenant auth (Better Auth)
- Monitoring & observability

**Team capacity:**
- 2 developers (Louis + Gabin)
- 1 graphic designer (Maëli)

This document challenges three core assumptions embedded in the research.

---

## Assumption 1: The Technical Stack Is Deliverable by 2 Developers

### What's Actually Being Built

The architecture described is a **distributed agent platform**, not a simple SaaS app:

| Layer | Complexity Driver |
|-------|------------------|
| Nuxt 3 frontend | Request/response, but requires /agents, /monitor, /integrations, /workflows pages |
| Hono + Mastra runtime | Agent orchestration, workflow definitions, tool routing |
| pgvector RAG | Document ingestion, chunking, embedding, retrieval, reranking |
| MCP integration | Per-client MCP server setup, auth, connection management |
| Multi-tenancy | Org-level isolation, Better Auth org plugin, per-client DB or schema |
| Real-time | SSE streaming, Redis pub/sub, BullMQ job queues |
| Self-improvement | 4 layers: runtime memory, skill system, session FTS, offline DSPy optimization |
| Monitoring | Agent performance tracking, failure alerts, usage dashboards |
| Infrastructure | Docker Compose → K8s migration, OVHcloud VPS management |

### The 2-Developer Reality Check

**Ludicrous premise:** 2 developers building a distributed AI platform while also:
- Managing 5-15 client relationships
- Deploying and configuring per-client agents
- Responding to support requests
- Tuning prompts weekly
- Running monthly reviews
- Building case studies for GTM

**What's missing from the org chart:**
- DevOps / Platform engineer
- Client success / Account manager
- Sales (even part-time)
- Technical support

**The infrastructure math:**
- Each new client = new MCP server configs, new RAG corpus, new permissions
- At 5 clients with 2 agents each = 10 agent instances to monitor
- Each agent connects to 2-4 business tools = 10-20 integrations to maintain
- RAG knowledge bases go stale, need re-indexing
- MCP servers break when third-party APIs change

### What Will Break

1. **Deployments will block on client onboarding.** When a new client signs, someone has to configure their agent, test the integrations, and deploy. That's a full-day task minimum. If two clients sign in the same week, something else stops.

2. **Technical debt will accumulate silently.** Mastra updates, Nuxt security patches, pgvector performance issues, Redis connection pool exhaustion — these don't announce themselves until something fails catastrophically at 2 AM.

3. **MCP integrations will be the bottleneck.** Every client's business tools are different. Building a custom MCP server for a client's specific CRM, email system, or ERP is engineering work — not support work. Doing it 5 times in parallel with product development is not sustainable.

4. **The 4-layer self-improvement system will never be completed.** Layer 1 (bounded memory) is table stakes. Layer 2 (skill system) is useful. Layer 3 (session FTS) is nice-to-have. Layer 4 (DSPy offline evolution) is speculative. Shipping all 4 at launch means shipping none of them well.

5. **Maëli is a designer, not a product manager.** The designer's value is maximized on brand, UI, and visual identity — not on triaging client issues or managing the product backlog.

### Verdict on Assumption 1

**The stack is buildable. The team cannot build it and run it simultaneously.**

**The choice is binary:**
- Option A: Build the product first, launch with 1-2 pilot clients, do zero active sales during build
- Option B: Take clients now and accept a half-built product that will hemorrhaging time

There is no Option C where both happen at the quality level described in the BRIEF.

---

## Assumption 2: €1,200-3,000/Month Managed Service Is Sustainable

### The MRR Math at 5 Clients

| Item | Calculation | Monthly |
|------|-------------|---------|
| Revenue (5 clients × €1,500 avg) | 5 × €1,500 | €7,500 |
| Infrastructure (OVHcloud VPS, Postgres, Redis, S3) | 25-50 orgs scale | €200-300 |
| **Gross margin** | | **€7,200** |

€7,200 to cover:
- 2 developers' salaries
- Maëli's time (partial or full)
- Client success activities
- Support coverage
- Accounting, legal, admin
- Profit / reinvestment

**This assumes infrastructure scales linearly.** It does not. At 5 clients with active RAG and MCP integrations, a single VPS is likely insufficient. Multi-tenant isolation requires either separate DBs (complex) or shared DB with schema isolation (risky). Realistically: €300-500/month infra by month 3.

**Revised gross margin at realistic infra:** €7,000

### The Labor Cost Reality

The €7,000 gross margin must cover all human labor:

| Role | Monthly Cost at French Market Rate | Hours Available |
|------|----------------------------------|-----------------|
| Senior developer (Louis or Gabin) | €6,000-8,000/month | Full-time |
| Mid developer | €4,000-5,500/month | Full-time |
| Designer (Maëli) | €3,500-5,000/month | Part-time likely |

**At 5 clients, MRR covers 1.0-1.2 developer equivalents.** Not 2.

### The Per-Client Time Cost

Managed service is not passive income. Here's the realistic monthly time per client:

| Activity | Frequency | Hours/Client/Month |
|----------|-----------|-------------------|
| Monthly review meeting | 1× | 1.0 |
| Prompt tuning / workflow adjustment | 2-4× | 2.0 |
| Agent monitoring + alerting response | Weekly | 2.0 |
| New use case exploration | 1×/quarter (1/3/month) | 1.5 |
| Ad-hoc support requests | Variable | 2.0-4.0 |
| Client communication overhead | | 0.5 |
| **Total** | | **9-11 hours** |

At 5 clients: **45-55 hours/month of client-facing work** — without writing a single line of product code.

At 10 clients: **90-110 hours/month** — which is 2.25-2.75 full-time employees of just client management.

### The Crossover Point Problem

The economics only work at scale, but you need to survive to get there:

| Clients | MRR | After Infra | Labor pool | Realistic? |
|---------|-----|-------------|------------|------------|
| 3 | €4,500 | €4,200 | 1 dev | Painful |
| 5 | €7,500 | €7,200 | 1.2 devs | Unsustainable |
| 10 | €15,000 | €14,500 | 2.4 devs | Barely viable |
| 15 | €22,500 | €21,500 | 3.5 devs | Starting to work |
| 20 | €30,000 | €28,500 | 4.5 devs | Real business |

**The gap between 5 and 15 clients is where most startups die.** Too much revenue to quit, not enough to hire. And the only way across it is to keep building while serving clients — which circles back to Assumption 1.

### Pricing Adjustments That Help (But Don't Solve)

The BRIEF proposes €800-1,500/month per agent. If we assume:
- 2 agents per client average = €1,600-3,000/client/month
- 5 clients = €8,000-15,000 MRR

This is better. But it requires selling 2 agents to every client, which requires:
- Clients to see value in 2 separate workflows
- More deployment complexity per client
- More monitoring overhead per client

The pricing isn't broken at the model level. The model level assumes 10+ clients, which is the problem.

### Verdict on Assumption 2

**The economics are not viable at 5 clients. They become marginally viable at 15+ clients.**

**The trap:** You need 3× more clients to reach sustainability, but getting those clients requires product quality and sales capacity that the current structure cannot produce.

**Recommended pivot:** The diagnostic (€490) and pilot (€3,000-6,000 setup) are the actual survival mechanism. These are one-time revenue that doesn't require ongoing management. But pilots convert to managed service, which re-creates the problem.

**A better structure:**  
- Lower managed service price (€600-800/month) to reduce client expectations
- Strict scope: 1 agent, 2 integrations, 1 workflow per client
- 10-hour/month support cap per client
- Explicit "unlimited" monitoring, limited optimization
- This converts managed service from a time sink into a semi-passive revenue stream

---

## Assumption 3: Quality Will Be Maintained With This Team Size

### What "Quality" Means in Managed Service

For Alizé, quality has concrete definitions:
- Agents that work reliably 9-5 business hours
- Response times under 30 seconds for standard queries
- RAG that retrieves relevant, accurate documents
- MCP integrations that don't break when third-party APIs change
- Prompt outputs that match client brand voice and accuracy standards
- Monthly reviews that show measurable ROI

**Every one of these requires someone to care, continuously.**

### The Reality of Running Production AI Agents for Clients

AI agents fail in non-obvious ways:
- A client's PDF upload changes format → RAG retrieval degrades silently
- A CRM API changes → MCP connection breaks → agent stops enriching leads
- A prompt works for 3 weeks → client updates their product catalog → outputs become stale
- User behavior changes → agent gets used for edge cases it wasn't designed for
- Model updates from Mistral or OpenAI change output format → downstream integrations break

**These failures are invisible until a client complains.** By then, trust is damaged.

### The Maëli Problem

Maëli is a graphic designer. She cannot:
- Debug a broken MCP server connection
- Tune a RAG retrieval prompt
- Respond to a 2 AM monitoring alert
- Redesign a workflow when a client's process changes

**Her value is real but misallocated if she's the fallback for technical issues.** The current structure assumes she's part of the "delivery team" alongside the two developers. She isn't — she's a creative resource.

### What "Good Enough" Looks Like at This Team Size

With 2 developers + 1 designer, quality will mean:
- 70% of clients get 80% of the promised value
- 30% of clients get 50% of the promised value (those with more complex setups)
- Some client issues take 3-5 days to resolve instead of 1-2
- New features ship 2-3× slower than competitors
- Case studies take longer to generate because clients need more hand-holding

**Is this acceptable?** For a bootstrapped startup, yes. For the premium positioning described in the BRIEF, it's a gap between promise and delivery.

### The Competitive Risk of Quality Failures

Alizé's positioning vs. Agentova relies on being "the managed option that actually works." If Alizé has reliability issues comparable to a self-service tool, the entire differentiation collapses.

> "We manage everything for you" only holds if "everything" is actually managed well.

One public failure at a French company — an agent that sends a wrong email, accesses the wrong data, or just doesn't work for 3 days — will generate a Trustpilot review that undermines the entire brand.

### Verdict on Assumption 3

**Quality is achievable at a minimum viable level. It is not achievable at the level implied by the BRIEF's premium positioning.**

**The gap:** The BRIEF describes a white-glove managed service. The team size supports reactive maintenance, not proactive optimization.

**What actually happens:**  
- Monitoring becomes "wait for client to complain"
- Monthly reviews become "look at the dashboard together"
- Prompt tuning becomes "change one line and hope"
- New use case expansion becomes "scope creep that delays other clients"

---

## Cross-Cutting Risk: The Sales-Development Trap

The most critical risk is not any single assumption — it's the **interaction effect** between all three.

Here's the trap:

1. To reach sustainable economics, Alizé needs 15+ clients
2. To get 15+ clients, Alizé needs active sales + marketing
3. Active sales brings in clients faster than 2 developers can onboard them
4. Under-resourced onboarding damages quality → churn
5. Churn destroys MRR → no hiring → no capacity for sales
6. No capacity for sales → stuck at 5-8 clients indefinitely

**This is the classic services company death spiral:** too small to invest in growth, too hungry to turn down bad-fit clients, too reactive to build a real product.

### The BRIEF's Go-to-Market Timeline

| Phase | Timeline | Revenue Needed |
|-------|----------|----------------|
| Phase 1: Seed | Months 1-3 | €0 (pilots) |
| Phase 2: Proof | Months 4-6 | €5,000-10,000 MRR |
| Phase 3: Scale | Months 7-12 | €10,000-20,000 MRR |
| Phase 4: Grow | Year 2 | €50,000+ MRR |

**The Phase 1-2 gap is the danger zone.** Zero or low revenue for 6 months with active costs.

If Louis and Gabin have living expenses covered (salaried elsewhere, savings, etc.), this is manageable. If they need Alizé to generate income immediately, Phase 1-2 is a cliff.

---

## Verdict: Recommended Actions for Louis

### Immediate (Do Now)

**1. Kill the 4-layer self-improvement architecture for launch.**
Cut to Layer 1 only (bounded memory). Layers 2-4 are post-product-market-fit features. Every sprint spent on speculative features is a sprint not spent on client delivery or product hardening. The 4-layer architecture reads like a research paper, not a launch plan.

**2. Accept that 2 developers cannot do both product and service simultaneously.**
Choose one:
- **Path A:** All-hands on product for 60 days, then launch with 1-2 pilot clients only (no active sales during build)
- **Path B:** Take clients now, assign one developer to "delivery" (80% client work, 20% product) and one to "product" (60% building, 40% delivery support)

Path A is faster to a real product. Path B is more conservative but risks building a half-finished product for longer.

**3. Simplify the tech stack before writing code.**
Current: Nuxt + Hono + Mastra + pgvector + Redis + BullMQ + MCP + Better Auth + 4-layer learning + Docker Compose → K8s + monitoring

**Realistic for 2 devs:** Nuxt + Hono + Mastra (embedded) + pgvector + one external service (Redis or managed) + Docker Compose only. No K8s, no offline DSPy, no session FTS.

**4. Price the managed service at what the market will bear, not what you need.**
The €800-1,500/month per agent pricing is aspirational. For first clients, €600-800/month per agent with strict scope (1 agent, 1 workflow, 2 integrations, 10h support/month) is more honest and more defensible. You can raise prices after 3 case studies exist.

### Short-term (Weeks 3-8)

**5. Do not sell more than 3 clients during the build phase.**
Every new client in the pipeline creates pressure to stop building and start deploying. A backlog of eager prospects is a resource trap. Qualify them, promise them a place in the cohort after launch, and close them post-launch.

**6. The first 3 clients should be friends/family or deeply discounted.**
The goal is case studies, not revenue. At €500/month for 3 months + €3,000 setup (deeply discounted from €6,000), the client gets a deal and owes a testimonial. Alizé gets social proof and learns what actually breaks.

**7. Hire or contract ONE person for client success / support before month 4.**
This person doesn't need to be a developer. They need to be able to:
- Respond to client messages within 4 hours
- Monitor a dashboard and escalate real issues
- Run the monthly review meeting from a template
- Take notes and translate client requests into tickets

A part-time VA with technical literacy costs €800-1,200/month. This single hire frees Louis and Gabin from client-facing reactive work and keeps them on product.

**8. Fix the Maëli allocation.**
Maëli should be 100% on brand, landing page refinements, and case study presentation design. She should have zero involvement in client technical issues. If she's being asked to help with client problems, that's a symptom that the developer capacity is already overstretched.

### Medium-term (Months 3-6)

**9. Do not add more clients until support response time is under 4 hours consistently.**
This is the throttling valve. If responding to clients takes 2 days, you're not ready for the next client. Growth should follow operational maturity, not the other way around.

**10. The partner channel is the only scalable GTM at this stage.**
Direct sales by Louis and Gabin consumes their best hours. A partnership with 3 expert-comptables or business consultants who refer clients in exchange for a 10% finder fee is the only model that scales without hiring a sales team. Focus all GTM energy on 1-2 partner relationships, not 20 cold LinkedIn outreach attempts.

**11. Build the minimum viable MCP integrations library first.**
Don't build custom MCP servers per-client. Build 3-5 generic ones (Gmail, Slack, Notion, HubSpot, generic REST) that can be configured per-client. Custom per-client MCP development is the fastest path to developer burnout.

---

## Summary Table

| Assumption | Challenge | Severity | Recommendation |
|------------|-----------|----------|----------------|
| "Stack is deliverable by 2 devs" | Cannot build + run simultaneously | **Critical** | Phase-gate: build first, then sell |
| "€1,200-3k/month is sustainable" | MRR math fails until 15+ clients | **Critical** | Lower price, stricter scope, contract support |
| "Quality maintained at team size" | Reactive ≠ managed service | **High** | Hire part-time client success before growth |
| "4-layer self-improvement at launch" | Speculative feature bloat | **High** | Cut to Layer 1 only |
| "Mastra is production-ready" | Stars ≠ hardening | **Medium** | Plan for significant debugging work |
| "Maëli is delivery capacity" | Designer ≠ engineer | **Medium** | Reallocate to brand/communications only |
| "Personal network → 5 pilots in 3 months" | Access ≠ conversion | **Medium** | Lower expectations, partner-first |

---

## The One-Line Verdict

**Alizé can work with 2 developers and 1 designer, but only if Louis accepts that the build phase and the sell phase cannot overlap, and that the first hire should be a client success coordinator — not a third developer.**

The current BRIEF describes a mature company. Louis has a startup's resources. The gap between the two is where good plans go to die.

---

*Operations Strategist — Pulse 5*  
*File: /data/workspace/alize/research/2026-03-30-pulse5-operations-debate.md*
