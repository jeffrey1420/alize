# Alizé Research — Pulse 16: Pilot Fee Economics Debate
**Date:** 2026-03-30
**Topic:** Does the €6,500 pilot fee cover realistic delivery costs?
**Agent:** Economics Strategist

---

## What Was Debated

Challenged the pilot fee economics. Pulse 15 established realistic delivery at 10-15 days at €350/day. D70 proposed raising the warm network pilot fee to €6,500. The question: does this work?

---

## Current Model Summary

**Delivery costs at different day counts:**
| Days | Delivery Cost (€350/day) | Revenue | Alizé Net |
|------|------------------------|---------|-----------|
| 5 | €1,750 | €6,500 | +€4,750 |
| 10 | €3,500 | €6,500 | +€3,000 |
| 15 | €5,250 | €6,500 | +€1,250 |
| 20 | €7,000 | €6,500 | -€500 |

But this only accounts for the delivery partner cost. Louis's involvement is unpriced.

---

## Agent Analysis

### The Delivery Partner Success Fee Miscalculates Risk

The contract caps delivery partner exposure at €3,250 (€350/day × 5 days + €1,500 success fee). At 10 delivery days, the partner delivers €3,500 in base value — already exceeding the cap. At 15 days, they're at €5,250 before the success fee.

The success fee only helps the delivery partner if (a) delivery takes ≤7 days AND (b) the pilot converts. At realistic 10-15 day delivery, the success fee is unreachable. The delivery partner has no incentive to invest extra time on quality.

### €350/day Is Below Market for Qualified PME Consulting

A delivery partner with genuine PME consulting experience — the ability to map processes, manage DG-level conversations, handle change management, and produce an ROI document — commands €450-600/day in the French market. €350/day buys someone junior or someone with availability problems. Quality delivery requires market-rate compensation.

### Louis's Time Is Completely Unpriced

The model treats Louis's Month 1 involvement as free. But Louis is spending 2 weeks on the first pilot instead of working at Grinto or developing Alizé's platform. At even a conservative €200/day opportunity cost, that's €2,000 of uncompensated time.

**Full pilot cost including Louis:**
| Cost item | Amount |
|-----------|--------|
| Louis time (10 days × €200/day opportunity cost) | €2,000 |
| Delivery partner (10 days × €350/day) | €3,500 |
| Infrastructure (Mastra, S3, tools) | €100 |
| Overhead, coordination | €300 |
| **Total** | **€5,900** |
| Revenue | €6,500 |
| **Net** | **€600** |

€600 margin on the first pilot doesn't cover any adversity — a client who pushes back, a scope change, or a 12-day delivery instead of 10, and Alizé loses money.

### The Case Study Upside Is Speculative

The model implicitly assumes the pilot generates a case study worth €10,000-50,000 in future business. But:
- If the pilot fails or delivers mediocre results, no case study
- If the client won't give a testimonial (NDA, privacy concerns), no case study
- If the metrics aren't compelling, the case study doesn't convert
- Even a good case study takes 3-6 months to generate measurable pipeline

The case study upside is real, but it's not cash flow. Alizé needs to pay its bills today.

### At 15 Days, The Model Breaks

| Scenario | Days | Louis Cost | Partner Cost | Total Cost | Revenue | Net |
|----------|------|-----------|-------------|-----------|---------|-----|
| Smooth | 10 | €2,000 | €3,500 | €5,900 | €6,500 | +€600 |
| Realistic | 12 | €2,000 | €4,200 | €6,200 | €6,500 | +€300 |
| Adversity | 15 | €2,000 | €5,250 | €7,250 | €6,500 | -€750 |
| Failure | 20 | €2,000 | €7,000 | €9,000 | €6,500 | -€2,500 |

The €6,500 price assumes delivery never exceeds 12 days. That's optimistic.

---

## Key Findings / Challenges

1. **€6,500 barely breaks even at 10 delivery days** — €600 margin doesn't cover adversity
2. **At 15 days, Alizé loses €750** — the price assumes best-case delivery
3. **Louis's time is unpriced** — €2,000 opportunity cost that the model ignores
4. **€350/day delivery partner rate is below market** — €400-450/day more realistic
5. **The success fee structure misaligns incentives at realistic day counts**
6. **The case study upside is speculative, not guaranteed cash**
7. **First 1-2 pilots may need to be written off as customer acquisition cost**

---

## Recommendations

### Option A: Raise the Price
- Warm network pilot fee: €8,500
- This gives ~€2,500 margin at 10 days, break-even at 13 days, and covers adversity
- Risk: may reduce conversion rate for an unknown brand

### Option B: Cut Delivery Scope
- Cap delivery at 7 days flat
- Pilot = one workflow, one integration, no RAG, no training session
- Keep €6,500 price
- Risk: scope may be too narrow to produce meaningful ROI

### Option C: Write Off First Pilots as CAC
- Accept first 2 pilots lose €1,000-2,000 each
- Frame as marketing/customer acquisition investment
- Target break-even from pilot 3 onwards
- Risk: cash runway issue if Louis doesn't have 3-6 months of runway

### Recommended: Hybrid
- Price at €7,500 for warm network pilots (not €6,500, not €8,500)
- Cap delivery at 10 days in the contract (not 5, not 15)
- Define explicit scope that fits 10 days
- Louis's involvement is explicit — he's not free labor, he's investing in his own company
- First 2 pilots at slight loss are acceptable as CAC if the case studies are real

---

## New Decisions or Unresolved Items

| ID | Topic | Status |
|----|-------|--------|
| D72 | Warm network pilot fee | €7,500 recommended (not €6,500, not €8,500) |
| D73 | Louis time cost | Explicitly priced at €200/day opportunity cost |
| U71 | Delivery partner day rate | €350 too low; €400-450 more realistic for qualified partner |
| U72 | First pilots as CAC | Accept first 1-2 pilots as customer acquisition investment |
