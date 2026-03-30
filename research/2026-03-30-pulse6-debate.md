# Alizé Research Pulse 6 — 2026-03-30

**Date:** 2026-03-30
**Pulse:** 6th research pulse of the day
**Agents:** Financial Viability Agent, Delivery Risk Agent, First Pilot Strategy Agent

---

## Agent 1: Financial Viability Agent

**Challenge:** Stress-test the financial model — 12-15 client viability threshold, pricing architecture, self-service tier, Year 1 GTM reach.

### Assumption Challenged: "12-15 clients needed for viability"

The assumption that economics don't work at 5 clients conflates two separate questions:
1. When do we have enough revenue to cover costs? (economics)
2. When do we have enough capacity to deliver without quality collapse? (operations)

**The math:**
- At 5 clients × €1,200/client/month average = €6,000/month revenue
- Infrastructure is only €50-150/month at that scale — not the binding constraint
- The real constraint: 2 developers can't actively serve 5+ clients AND build the platform

**Finding:** The 12-15 threshold is overstated. 7-8 clients at €1,200+/month covers costs IF founders accept below-market compensation. The real threshold is a capacity question, not a revenue question.

---

### Assumption Challenged: "Per-agent pricing at €800-1,500/month is correct"

Per-agent pricing creates perverse incentives:
- Client adds a second agent → revenue jumps but Alizé's delivery cost also jumps proportionally
- Client replaces an agent → revenue drops with no fault of Alizé's
- Annual contracting is harder because clients count "agents" not outcomes

**Finding:** Per-agent should become per-workflow. The workflow-tier model (€1,200/€2,800/€5,500/month) already debated is the right direction. Current per-agent structure is a liability.

---

### Assumption Challenged: "Self-service tier at €200-300 creates a funnel"

The brand dilution argument is backwards — Alizé has no brand yet. The real risk is opportunity cost.

Building a self-service product in Year 1 means NOT building:
- Better monitoring dashboards
- Faster integration connectors
- A case study from the first pilot client

**Finding:** Self-service tier is a Year 2 conversation. The opportunity cost of dev time in Year 1 exceeds the revenue potential.

---

### Assumption Challenged: "12-15 clients achievable in Year 1 via outbound-only GTM"

Sales velocity reality check:
- Cold outbound to qualified PME/ETI: 1-2% email response rate, 5-10% of those become qualified meetings
- From qualified meeting to signed pilot: 30-50% conversion
- 3-month sales cycle is a best-case — at 100-200 person ETI companies, €4,500 pilot can trigger formal procurement (2-3 months just to get PO)

**Realistic Year 1 pace:** 6-8 clients maximum, unless one founder goes full-time sales by Month 2.

**The core tension:** Alizé has no sales person. Louis + Gabin doing outreach, discovery, pilot management, AND building product = effective sales bandwidth is 30-40% of full-time. That cuts the realistic outcome to 6-8 clients.

---

### Financial Viability Verdict

| Assumption | Verdict |
|-----------|---------|
| 12-15 clients needed for viability | OVERSTATED — 7-8 covers costs if below-market salaries accepted |
| €800-1,500/month per agent | WRONG STRUCTURE — should be per-workflow |
| Self-service tier at €200-300 | YEAR 2, NOT YEAR 1 — opportunity cost is the real issue |
| 12-15 clients in Year 1 outbound | OPTIMISTIC — 6-8 is realistic given no dedicated sales |

---

## Agent 2: Delivery Risk Agent

**Challenge:** Stress-test the service delivery model — 2-dev capacity, product vs service framing, first hire, technical debt.

### Assumption Challenged: "2 developers can build the MVP AND deliver service simultaneously"

Context-switching research (Gloria Mark, UC Irvine): each interruption costs ~23 minutes of refocus time. Client work is interrupt-driven. Development requires sustained deep work. These are structurally incompatible work modes for one person.

**The math:** If Alizé signs even 2 early clients in Month 1, Louis is on calls 2-4 hours/day. An 8-hour day with 3 hours of calls doesn't leave 5 hours of productive development — it leaves 2-3 hours of fractured, error-prone output.

**Finding:** The 2-developer team can build the MVP OR deliver early service — not both simultaneously at quality.

---

### Assumption Challenged: "'Product vs. service' is a binary choice"

The current framing suggests either (a) delay product for pure service or (b) build product first. This is a false dichotomy.

- "Pure service first" means Alizé is selling human-managed AI agents, not a managed AI agent platform — different business, different unit economics
- The real question: what is the minimum viable agent run loop that enables a non-developer to deliver early client engagements?

**Finding:** The question should be "who is the delivery layer and what is the minimum viable product to support it?" — not product vs. service.

---

### Assumption Challenged: "Client success is the right first hire framing"

Client success is a post-product-market-fit, at-scale role. Alizé is pre-launch with 1-3 pilots. Framing the first hire as "client success" leads to:
- Hiring someone who optimizes for client happiness metrics rather than delivery reliability
- A person who cannot do the job because the product isn't stable enough
- A role with no leverage — dependent on Louis/Gabin building the underlying capability

**What Alizé actually needs:** An implementation/delivery specialist — someone who sets up and runs agent workflows, configures integrations, monitors performance. Not a CS manager.

**Finding:** "Client success" is the wrong framing. The first non-dev hire should be an implementation/delivery contractor who can execute manual agent management.

---

### Assumption Challenged: "Louis going client-facing in Month 3 solves the delivery problem"

**Why this is wrong:**
1. Month 3 is too late — if Louis has been building relationships from Day 1, the transition already happened, just messily
2. Louis going client-facing doesn't give you a robust agent platform — it just changes who's managing the client chaos
3. By Month 3, if early clients have had bad experiences, hiring a CS person is too late to save those relationships

**Finding:** The delivery/implementation person (contractor or hire) needs to be in place by Month 1-2 — not Month 3.

---

### Technical Debt Under 50% Time on Calls

If Louis spends 50% of Month 1-3 on client calls:
- 3-month MVP → 5-6 month MVP
- Agent run loop robustness deferred → hallucinated outputs, failed runs, client-visible failures
- Observability gap → manual diagnosis of "agent isn't working"
- Integration fragility → hardcoded client-specific integrations, no abstraction
- Prompt management skipped → inconsistent quality, undebuggable outputs

**The vicious cycle:**
```
Louis 50% on calls → platform takes 2x longer
→ More manual delivery → Louis pulled into more calls
→ Platform slips → clients get impatient
→ Pressure to "just fix it" → more bespoke code
→ More technical debt → slower future development
→ Platform instability → more client escalations
```

---

### Delivery Risk Verdict

| Assumption | Verdict |
|-----------|---------|
| 2 devs can build AND deliver simultaneously | WRONG — capacity collapses at 3+ clients |
| Product vs. service is a binary | FALSE DICHOTOMY — question is delivery layer + minimum viable platform |
| "Client success" is the right first hire | WRONG FRAMING — implementation/delivery specialist needed, not CS manager |
| Louis goes client-facing in Month 3 | TOO LATE — contractor needed by Month 1-2 |

---

## Agent 3: First Pilot Strategy Agent

**Challenge:** Stress-test the first pilot assumptions — vertical selection, company size, Louis's network leverage, pilot pricing structure.

### Assumption Challenged: "Professional services first"

Professional services firms are actually harder first-pilot targets:
- High expectations, low tolerance for mediocre outputs — one bad deliverable = lost reference
- Non-standardized processes — each client engagement is different = harder AI delivery
- Long sales cycles — sophisticated buyers who comparison-shop
- Being targeted by every other AI vendor (Sage, Pennylane, Harvey, Casetext, Deloitte AI)

**Better alternative:** Digital agencies / e-commerce (10-50 employees)
- Highly standardized processes (content production, SEO reporting, social media scheduling, lead qualification)
- Fast delivery — high standardization, digital-first processes, easy API integrations
- Fast closability within Louis's network (Grinto connections → web agencies, e-commerce clients)
- High reference value — digital agencies talk to other agencies

**Finding:** "Professional services first" is driven by product intuition, not GTM wisdom. The first pilot should target digital agencies/e-commerce.

---

### Assumption Challenged: "First pilot target: 50-200 employees"

Problems with 50-200 for a first pilot:
- Multiple decision-makers (owner + managers + IT + operations)
- Procurement can require contracts, legal review, multi-signature
- Higher expectations = higher risk of losing reference client

**Better alternative:** 20-30 person companies
- One decision maker — founder/owner, one call closes it
- Faster delivery — fewer users, simpler processes, less change management
- More acute pain — "we can't hire fast enough" is felt strongly
- Lower downside risk — one small client, not a reference account
- Still referenceable — a 25-person company is still a real PME

**Finding:** The goal of the first pilot is proof of delivery and proof of concept, not landing the biggest contract. 20-30 person companies are the right first pilot target.

---

### Assumption Challenged: "Discounted pilot lowers the barrier"

A discounted pilot (e.g., 50% off) is the worst of both worlds:
- Client has full-price expectations despite discount
- Alizé has no margin buffer — scope creep eats the discount
- Doesn't filter for real commitment — discount hunters vs. genuinely bought-in clients

**"Free" has its own problems:**
- No skin in the game = no real engagement
- "Pilotitis" — they kick the tires but don't actually integrate into workflows
- Free = perceived low value

**Better alternative: AT-COST with strict deliverables**
- Charge 100% of cost (actual time/infrastructure), zero margin
- Set strict, written success criteria
- Lock scope for 30/60/90 days
- Get testimonial/case study rights in writing
- Include 90-day review — if working, convert; if not, both sides walk

**Finding:** "Discounted" signals low value. "At-cost with strict deliverables" signals "we're serious, and so are you."

---

### Louis's Network: Where It Actually Points

Louis has:
- Grinto connections → web agencies, digital agencies, their clients
- MyDigitalSchool → digital marketing ecosystem (agencies, brands with marketing teams, e-commerce)

This network does NOT point to professional services. It points to digital agencies and e-commerce businesses.

**Finding:** Follow the Grinto connections to digital agencies — warmest leads, easiest delivery.

---

### Pilot Strategy Verdict

| Assumption | Verdict |
|-----------|---------|
| Professional services first | WRONG — digital agencies/e-commerce faster to close and deliver |
| 50-200 employee target for pilot | WRONG — 20-30 person companies better for first pilot |
| "Discounted" pilot | WRONG FRAMING — at-cost with strict deliverables + case study rights |
| Louis's network → professional services | WRONG — network points to digital/e-commerce |

---

## Cross-Cutting Findings from Pulse 6

### Three Assumptions That Survived Scrutiny
1. The pricing architecture (per-workflow) — confirmed correct direction, needs execution
2. Vertical depth as a moat — still valid, but should be digital agencies first, not professional services
3. Client relationships as a defensible moat — still valid, but first you need a client

### Three Assumptions That Were Challenged
1. "12-15 clients for viability" → 7-8 is economically viable; the real constraint is capacity
2. "Professional services first" → digital agencies/e-commerce better for first pilot
3. "Louis goes client-facing in Month 3" → delivery contractor needed by Month 1-2

### New Findings Not Previously Debated
1. Per-agent vs per-workflow pricing is still unresolved (U5 in TODO)
2. The delivery model (who is the client-facing layer) is the biggest unaddressed risk
3. Year 1 realistic client target is 6-8, not 12-15 — this changes the financial model

---

*Pulse 6 complete — 2026-03-30 11:27 UTC*
