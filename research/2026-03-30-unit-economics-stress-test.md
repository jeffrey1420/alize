# Unit Economics Stress Test — 2026-03-30

## The Model Under Test

**Core assumption:** "€2,000/month per client is sustainable for a 1-person delivery operation."

**What's actually in the BRIEF.md:**
- Managed Service: €800-1,500/month per agent
- Multi-Agent ETI package: €2,500-4,000/month
- The €2,000/month figure appears in the debate log as a claimed sustainability threshold, not a documented BRIEF.md price point
- 25% gross margin claimed at this price level
- Pilot setup: €3,000-6,000

**The inconsistency to flag:** BRIEF.md lists €800-1,500/month as the managed service price. The €2,000/month sustainability claim is never derived from a cost model — it's asserted. These are different numbers serving different purposes, and neither has been validated against actual delivery costs.

---

## Load Test: 1-Person Delivery

### The Capacity Problem

Louis is not a SaaS business. He is a solo operator performing three distinct roles simultaneously:

| Role | Hours/Week (conservative) | Hours/Week (realistic) |
|------|---------------------------|------------------------|
| Delivery lead (agent deployment, monitoring, troubleshooting) | 5-8 hrs/client | 8-12 hrs/client |
| Account manager (check-ins, reporting, renewals, expansions) | 2-3 hrs/client | 3-5 hrs/client |
| Sales lead (outbound, discovery calls, proposals, closing) | 10-15 hrs/week | 15-20 hrs/week |

### Active Client Capacity

**Conservative estimate:** Each client consumes 8 hrs/week of delivery + 2.5 hrs/week of account management = **~10.5 hrs/week/client**

**Realistic estimate:** Client complexity varies. Professional services clients (the stated target) typically have messy tools, unclear scopes, and need more hand-holding. Realistic load: **~15 hrs/week/client**

| Scenario | Max Clients | Why it Breaks |
|----------|------------|---------------|
| Conservative (10.5 hrs/client) | **4 clients** | Louis at 42 hrs/week just for delivery + account management, 0 time for sales |
| Realistic (15 hrs/client) | **3 clients** | Louis at 45 hrs/week, sales pipeline dies, no new clients ever |

**Critical finding:** Louis cannot handle 4 active clients AND run an active sales pipeline. Something stops. In practice, the sales pipeline stops — which means no new clients, which means MRR never grows beyond whatever he has today.

### The €2,000/month sustainability claim breaks here

If Louis has 4 clients at €2,000/month, that's **€8,000 MRR**. But he's working 45+ hours/week with zero capacity for business development. The moment a client churns, he's exposed. There is no growth engine in this model — only decay.

---

## The 25% Margin Claim

### Reconstructing the cost model

To have a 25% gross margin at €2,000/month/client, Louis's cost to deliver must be ≤€1,500/client/month.

**Direct delivery costs (per client, monthly):**
- Infrastructure (OVHcloud share, ~1/10th of €100-150 VPS): €10-15
- Tools, subscriptions, licenses: €50-100 (estimate)
- Subtotal direct costs: **€60-115/client/month**

**Labor cost (Louis's own time):**
- 10-15 hours/client/month at what cost?
- If Louis wants minimum wage equivalent: €1,850/month / 160 hours = **€11.56/hour**
- If Louis wants a reasonable freelance rate: €50-80/hour

**Let's be generous and say Louis values his time at €25/hour (below market, high optimism):**

| Hours/Client/Month | Labor Cost @ €25/hr | Total Cost | 25% Margin Price |
|--------------------|---------------------|------------|-----------------|
| 5 hrs | €125 | €185-240 | €247-320 |
| 10 hrs | €250 | €310-365 | €413-487 |
| 15 hrs | €375 | €435-490 | €580-653 |
| 20 hrs | €500 | €560-615 | €747-820 |

**The €2,000/month price point implies a cost structure where Louis is extracting labor at well below minimum wage**, OR the 25% margin claim is based on infrastructure costs only and completely ignores labor.

### At what MRR does Louis earn minimum wage equivalent?

If Louis wants to earn **€1,850/month (French minimum wage net)** and his direct costs are €60-115/client/month, here's the math:

- **Revenue needed:** €1,850 + (clients × €115)
- **At 1 client (€2,000 MRR):** €2,000 - €115 = €1,885. **Margin: 93.5% over direct costs, but Louis is working 15 hours/week for €1,885 = €3.14/hour**
- **At 4 clients (€8,000 MRR):** €8,000 - €460 = €7,540. But Louis is working 45+ hours/week. €7,540 / 180 hours = **€4.19/hour**

**The minimum wage equivalent threshold requires Louis to work more hours than exist in a week.** At 4 clients, he's already at capacity. At minimum wage equivalent, he'd need ~150 hours/month (37.5 hrs/week), but 4 clients already consumes 45+ hours/week. He cannot scale out of this.

**Finding:** The €2,000/month price does not generate enough margin for Louis to pay himself a living wage while also maintaining the sales pipeline required to replace churned clients. The model is not sustainable — it's a slow extraction of labor at below-market rates.

---

## Conversion Sensitivity

### The pilot-to-recurring conversion risk

**Current model assumption:** Pilots convert to recurring clients.

**Reality check:** No conversion rate has been validated. For the purpose of this stress test, we'll use three scenarios:

| Scenario | Pilot → Recurring Rate | Result |
|----------|----------------------|--------|
| Optimistic | 3 of 3 convert (100%) | Viable — barely |
| Realistic | 1 of 3 converts (33%) | Cash flow crisis |
| Pessimistic | 0 of 3 convert (0%) | Business doesn't start |

### 6-Month Cash Flow Analysis (2 of 3 pilots don't convert)

**Assumptions:**
- 3 pilots signed at €4,500 each (midpoint of €3,000-6,000 range)
- Pilot duration: 2 months (setup + monitoring)
- Only 1 pilot converts to €2,000/month recurring
- Louis's personal burn rate: €2,500/month (optimistic — rent, health insurance, living costs)
- Month 0 = Month 1

| Month | Revenue | Expenses | Cash Position |
|-------|---------|----------|---------------|
| 0 | €0 | €2,500 | -€2,500 |
| 1 | €4,500 (pilot 1 setup) | €2,500 | +€2,000 |
| 2 | €9,000 (pilot 2 + 3 setup) | €2,500 | +€8,500 |
| 3 | €9,000 + €2,000 (1 recurring) | €2,500 + delivery costs | +€17,000 |
| 4 | €2,000 (1 recurring only) | €2,500 | +€16,500 |
| 5 | €2,000 | €2,500 | +€16,000 |
| 6 | €2,000 | €2,500 | +€15,500 |

**Wait — the pilots generated €13,500 in setup fees in months 1-2.** This looks like cash, but:

1. Each pilot requires 2-4 weeks of setup work — that's labor Louis isn't billing separately for
2. Pilot setup fees of €3,000-6,000 are one-time. They don't recur.
3. The €2,000/month recurring doesn't start until month 3 at the earliest
4. With only 1 of 3 converting, MRR is €2,000 — not enough to sustain operations

**Revised 6-month picture accounting for delivery labor (unpaid):**

| Month | Revenue | Delivery Labor Cost (€0/hr, optimistic) | Cash Position |
|-------|---------|------------------------------------------|---------------|
| 1-2 | €13,500 (3 pilots) | €0 | +€11,000 |
| 3 | €11,000 (setup complete, 1 recurring) | €0 | +€9,500 |
| 4 | €2,000 | €0 | +€9,000 |
| 5 | €2,000 | €0 | +€8,500 |
| 6 | €2,000 | €0 | +€8,000 |

**With 1 conversion:** Louis has earned €13,500 over 6 months = €2,250/month average. He has €8,000 in the bank at month 6, but zero sales pipeline (he's been delivering pilots and hasn'tprospected), and his only recurring client is paying €2,000/month — well below his burn rate.

**The cash looks positive because pilot fees are subsidizing it. This is a consulting cash flow, not a SaaS recurring revenue model.**

### Is €4,500 pilot revenue a real business or an expensive networking habit?

**The honest answer: it's a consulting engagement with a conversion hook.**

Problems with this framing:

1. **Pilot fees are front-loaded consulting income, not business momentum.** The €4,500 arrives once, then the client either converts or doesn't.
2. **No conversion = €4,500 for 2 months of work.** At 80 hours of setup labor (conservative), that's €56/hour — but Louis's real cost is higher and the hours are untracked.
3. **The recurring revenue doesn't start until month 3-4 at earliest.** If conversion is 33%, the business runs on one-time fees until a pipeline of recurring clients builds — which requires a sales motion Louis doesn't have time for.
4. **The €4,500 pilot price is not defensible at scale.** As a solo operator, Louis cannot run 3 simultaneous pilots. Sequential pilots generate €3,000-6,000 every 2 months, which is lumpy and requires constant delivery + sales + account management.

**Conclusion:** €4,500 pilots work as a customer acquisition strategy only if conversion is high AND Louis has enough capacity to run pilots sequentially while prospecting the next batch. He doesn't. The model requires him to be in constant sales mode on top of delivery, which he cannot sustain.

---

## The Realistic MRR Trajectory

### 12-Month Projection (Realistic Scenario)

**Assumptions:**
- Louis can handle max 4 active managed clients (capacity ceiling)
- Each managed client pays €1,000-1,500/month (middle of €800-1,500 range — using BRIEF.md's actual stated prices)
- Sales cycle: 1 new client per 2 months (optimistic, given no dedicated sales time)
- Churn: 1 client per 6 months (realistic for PME)
- Pilot conversion: 50% (generous)

| Quarter | Active Clients | MRR | Notes |
|---------|---------------|-----|-------|
| Q1 | 1-2 | €1,000-3,000 | Pilots converting, early stage |
| Q2 | 2-3 | €2,000-4,500 | Growing, but capacity constrained |
| Q3 | 3-4 | €3,000-6,000 | Hit ceiling — cannot add more without help |
| Q4 | 3-4 | €3,000-6,000 | Plateau. Churn replaces growth. |

**Revenue ceiling: €6,000 MRR** — not €8,000 (which would require €2,000/client at 4 clients, above the BRIEF.md stated range).

At €6,000 MRR, Louis earns €6,000 - (4 × €115 infrastructure) = €5,540 gross. Minus his time (180+ hours/month), he's earning the equivalent of **€30/hour** — below any reasonable freelance developer day rate, and below what Agentova charges per agent (€49-99/month).

**The €2,000/month per client sustainability claim is NOT supported by the capacity analysis.** The realistic ceiling is €1,000-1,500/client at 4 clients, generating €4,000-6,000 MRR — which is solo consulting income, not a scalable business.

---

## Revised Recommendation

### What needs to change

**1. Pricing must be restructured, not defended.**

The €800-1,500/month per agent range (BRIEF.md) is more honest than the €2,000/month sustainability claim (debate log). But neither number was derived from a cost model.

**Specific challenge to the pricing model:** The 25% margin claim is fiction. It ignores labor entirely. The correct framing is:
- If Louis wants €1,850/month (minimum wage), he needs €1,850 + €460 (infrastructure at 4 clients) = **€2,310/month minimum revenue floor**
- At €1,200/client/month (midpoint of range), that requires **2 clients just to cover burn rate** before any profit

The price must cover: infrastructure + Louis's labor at market rate + sales + account management + a buffer for growth investment.

**2. The solo operator model fails at 3+ clients.**

The two-body problem (delivery + sales in one person) was identified in Pulse 6 and again in Pulse 46. Nothing has changed. The revised model must account for:

- Either a delivery contractor by Month 2 (so Louis can focus on sales)
- Or a productized service that reduces delivery time per client (templates, playbooks, self-service onboarding)

**3. Pilot conversion rate must be treated as a hypothesis, not a plan.**

The model assumes pilots convert to recurring. With zero validated conversion data, the business plan should assume 33% conversion as the base case, not the exception. At 33% conversion, Louis needs 3 pilots in parallel to generate 1 recurring client — but he can't handle 3 pilots simultaneously.

**4. Kill the "€2,000/month/client is sustainable" talking point.**

Replace it with a real cost model: what does Louis need to earn per month, what does delivery actually cost, and what client count and price point gets him there? The current claim is not defensible under scrutiny.

**5. The pilot pricing range (€3,000-6,000) is too wide.**

€3,000 and €6,000 are different conversations with different buyers. A tighter pilot package (one workflow, fixed scope, fixed timeline) at a fixed price creates clearer expectations and easier sales conversations.

### The real question

Louis is not building a SaaS business. He's building a solo consulting practice with a productized service wrapper. That's a valid business — but it has a completely different unit economics model than the one being discussed.

**If the goal is a sustainable solo consulting practice:** Price at €1,500-2,000/month per workflow, target 3-4 clients, accept that Louis earns €4,000-6,000/month for his expertise and labor. This is viable but not a venture-scale business.

**If the goal is a venture-scale business:** The model requires either (a) a delivery team that Louis manages, or (b) a productized self-service tier that scales without his direct involvement. Neither exists yet. The €2,000/month sustainability claim applies to neither scenario.

---

*Stress test completed: The €2,000/month per client sustainability claim is not supported by the capacity model, the cost model, or the conversion sensitivity analysis. The number needs to be replaced with a real unit economics framework based on Louis's actual hourly capacity, realistic conversion rates, and explicit cost structure.*
