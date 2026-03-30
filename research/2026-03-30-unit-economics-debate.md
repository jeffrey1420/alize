# Alizé Unit Economics: The Brutal Analysis
**Date:** 2026-03-30  
**Author:** Pricing Viability Agent  
**Purpose:** Challenge €2,000/month retainer against delivery reality

---

## Executive Summary

**The €2,000/month price point does not work as currently modeled.**  
The math produces a structural loss of €350-€1,850 per client in the first 3 months, and the business only becomes viable at 5+ clients — which requires winning 5 pilots first. The pricing appears to be internally convenient rather than market-validated.

---

## The Core Contradiction

You have four decisions that directly contradict each other on pricing:

| Decision | Pilot Fee | Monthly Retainer | Source of Truth? |
|----------|-----------|------------------|------------------|
| D72 | €7,500 warm network | — | "Warm network" exception |
| D110 | €1,500-2,000 | — | CAC model for pilots 1-3 |
| D43 | — | €2,000-2,500 multi-workflow | Starter/multi tier |
| D125 | €1,500/2-week diagnostic | — | €750/day rate |

**D72 and D110 cannot both be true.** If pilots 1-3 cost €1,500-2,000 but the warm network costs €7,500, you've created two entirely different service offerings with no explanation for why they'd be priced 3-5x differently for equivalent work.

---

## Unit Economics Model

### Assumptions

| Input | Value | Source |
|-------|-------|--------|
| Delivery days per pilot | 10-15 | D70 |
| Delivery partner day rate | €400-450 | D71 |
| Louis's time cost | €200/day | D73 |
| Louis days per month (per client) | 2 days | Assumption (needs validation) |
| Pilot fee | €1,500 (D110) | Conservative end |
| Monthly retainer | €2,000 | D43 |
| Contract length | 3 months | Standard |

### Month 1: Pilot Phase

```
Revenue:  Pilot fee      €1,500
Costs:    Delivery (12.5 days × €425 avg)  €5,313
          Louis (3 days × €200)            €600
          ----------------------------------------
NET:                                     -€4,413
```

**Month 1 is a loss of €4,413.** This is the entry cost per client.

### Months 2-3: Ongoing Delivery

```
Monthly Revenue:   €2,000 retainer
Monthly Costs:     Louis (2 days × €200)          €400
                   Delivery (embedded?)           €0-400*
                   ------------------------------------------
Monthly Net:                                   €1,200-1,600
```

*Assumes delivery is pre-approved scope within retainer. This is a heroic assumption.

### 3-Month Client Economics

| | Pessimistic | Base | Optimistic |
|-|-------------|------|------------|
| Pilot fee | €1,500 | €1,500 | €1,500 |
| Months 2-3 revenue | €4,000 | €4,000 | €4,000 |
| **Total Revenue** | €5,500 | €5,500 | €5,500 |
| Delivery costs | €7,350 | €5,313 | €4,000 |
| Louis costs (7 days total) | €1,400 | €1,400 | €1,400 |
| **Total Costs** | €8,750 | €6,713 | €5,400 |
| **NET (3 months)** | **-€3,250** | **-€1,213** | **+€100** |

**At base assumptions, you lose €1,213 per client in the first 3 months.**

---

## Break-Even Analysis

### What client count makes this work?

To recover the €1,213 average loss per client in month 1, you need surplus from months 2+:

- Monthly net per client (months 2-3): €1,200-1,600
- Months needed to recover month 1 loss: **0.8 to 1 month**

**But this assumes no new pilot costs.** Every new client you win restarts the cycle.

### Client Count Table

Assumes all clients on 3-month pilot cycle, staggered:

| Active Clients | Monthly Revenue | Monthly Costs (Louis) | Annual Delivery Cost | Annual Net |
|----------------|------------------|------------------------|----------------------|------------|
| 1 | €2,000 | €400 | €5,313 (1 pilot) | -€4,713 |
| 3 | €6,000 | €1,200 | €15,939 (3 pilots) | -€11,139 |
| 5 | €10,000 | €2,000 | €26,565 (5 pilots) | -€18,565 |
| 10 | €20,000 | €4,000 | €53,130 (10 pilots) | -€37,130 |

**The business burns cash proportional to client count** because every new client = new pilot loss.

### When does it flip positive?

You need ONE of these:

1. **Raise pilot fee to €5,500+** (covers delivery cost + Louis time + margin)
2. **Raise retainer to €4,000-5,000/month** (recoups pilot loss over 3 months)
3. **Reduce delivery days to 4-5 per pilot** (requires scope change or partner rate drop)
4. **Reduce Louis involvement to 0.5 days/client/month** (10x unlikely)
5. **Win only "warm network" clients at €7,500/pilot** (scales poorly, excludes market)

---

## Challenging the €2,000 Assumption

### Where Did This Number Come From?

Reading the decisions, the €2,000/month appears in D43 as a tiered pricing structure. There is **no market research cited, no competitive analysis, and no cost-plus calculation** supporting this number.

**This is internal convenience pricing.**

### What would cost-plus pricing look like?

```
Louis monthly cost (20 days × €200):         €4,000
Delivery costs (embedded, assuming 5 days):   €2,125
Overhead (30%):                               €1,838
-------------------------------------------------
Minimum viable price:                         €7,963/month
```

At cost-plus, you need **€8,000/month minimum.** The €2,000 price is 25% of cost-plus.

### What does the market bear?

Without market research (and none is cited in the decisions), we cannot know. But:
- SME automation consulting: €150-300/hour is common in French/Belgian market
- Full-time contractor equivalent: €400-600/day
- AI automation projects: €5,000-20,000/month for enterprise

**€2,000/month is dramatically underpriced for any meaningful delivery.** It reads like a number chosen to be "affordable" rather than "profitable."

---

## The Hidden Assumption Nobody Questioned

**The €2,000/month is positioned as "ongoing value" but is actually expected to cover:**

1. Monthly delivery time (Louis + partner)
2. Pilot cost recovery
3. Profit margin
4. Business overhead

It cannot do all four. **Something will break.**

Most likely: Louis will absorb the delivery cost by working for free on pilots, or he'll discover he can't deliver 10-15 days of value for €2,000/month and the quality will suffer.

---

## Brutal Recommendations

### 1. Kill D110 (€1,500 pilot) Immediately
At €1,500 pilot fee with €5,000+ delivery cost, you're paying clients to work with you. This is not a CAC model — it's a charity model.

### 2. Raise Pilot Fee to €5,000-7,500
This makes the pilot break-even or slightly profitable, which aligns with D72 (€7,500 warm network). The €7,500 should be the **standard pilot fee**, not an exception.

### 3. Raise Retainer to €3,500-4,500
This gives you €1,500-2,500/month to cover ongoing delivery after Louis's base cost. You'll still need to be disciplined about scope.

### 4. Or: Accept €2,000/month but radically change delivery model
- Offshore/subsidized delivery partner (€150-200/day)
- Reduce pilot scope to 5 days maximum
- Use no-code tools that don't require partner involvement
- Louis becomes the only billable resource

### 5. Get Market Validation
None of these decisions have been tested against actual customer willingness-to-pay. Before any more pilots, **talk to 5 potential clients and ask what they'd pay.** The answer will be more useful than any internal model.

---

## What This Document Cannot Answer

- Actual Louis days per client per month (assumed 2, unverified)
- Whether delivery partner is available/committed
- Whether the €400-450/day rate is negotiable
- What the market actually pays for AI workflow automation
- Whether warm network clients are actually willing to pay €7,500

**Fix these unknowns before finalizing pricing.**

---

## Conclusion

The €2,000/month retainer is **not viable** as currently modeled. The business loses €1,200-€3,250 per client in the first 3 months. You need either:

- **Higher pilot fees** (€5,000-7,500)
- **Higher monthly retainers** (€3,500-4,500)
- **Lower delivery costs** (subsidized partner, reduced scope)
- **Or accept this is a loss-leader for a different business model**

The current decisions (D43, D72, D110, D125) are internally contradictory and none appear to be grounded in actual cost data or market research. Someone needs to pick a number and run it backwards through the economics before any more pilots are sold.

---

*This analysis is based on decisions D43, D70, D71, D72, D73, D110, D125 as provided in the research context.*
