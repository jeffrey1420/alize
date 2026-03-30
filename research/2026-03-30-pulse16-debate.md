# Alizé Research — Pulse 16 Debate
**Date:** 2026-03-30
**Time:** 13:53 UTC
**Topic:** Three simultaneous debates: governance differentiator, pilot fee economics, vertical strategy frame
**Agents:** Governance strategist, Economics strategist, Vertical frame strategist

---

## What Was Debated

Three independent specialist agents challenged three core assumptions from the accumulated research:

1. **Governance Differentiator** — Is "delegation with accountability" actually differentiating, or just services firm boilerplate?
2. **Pilot Fee Economics** — Does the €6,500 pilot fee actually cover realistic delivery costs?
3. **Vertical Strategy Frame** — Is "pick a vertical" the right Year 1 frame, or a distraction?

---

## Agent 1: Governance Differentiator Challenge

### Challenged Assumption: "Governance/accountability is Alizé's key differentiator"

Existing research positions Alizé's differentiation as:
- "Delegation with accountability" (D53, D56)
- EU AI Act compliance, French infrastructure
- Security, audit logging, human validation gates
- "Alizé is accountable for results" (D56)

### Key Counter-Arguments

**"Accountability" is what every services firm claims. It is not a differentiator.**
Every consulting firm, every ESN, every freelance "AI consultant" in France claims to take accountability for their work. It's the default promise of anyone who charges €350/day. If Alizé leads with "we're accountable," a prospective DG hears exactly what they hear from every other vendor. There's no differentiated signal in that phrase.

**French PME buyers at 20-50 employees buy on relationship and price, not governance.**
The decision maker at a 30-person company is typically the founder/CEO/DG who wears every hat. They don't have a procurement department, a CISO, or an IT team asking about EU AI Act compliance. They care about: does this solve my problem, can I trust the person, and is the price reasonable. Governance frameworks are invisible to this buyer until something goes wrong.

**The EU AI Act is not a 2026 selling point. It might be a 2027-2028 selling point.**
Pulse 4 already established: EU AI Act compliance is 2027-2028 relevant, not 2026. But then why is it still embedded in the differentiation framing? If the brief says it's not relevant until 2027-2028, it shouldn't be in the external positioning at all — not even as a secondary point. Leading with something that doesn't matter to the buyer yet is worse than saying nothing about it.

**"Delegation with accountability" is jargon. It means "we do what we say."**
The phrase "delegation with accountability" is a services industry platitude. It translates to: "we'll do the work and stand behind it." That's not positioning — that's the baseline expectation of any professional engagement. Real differentiating language would be: specific outcomes, specific timelines, specific consequences.

**The actual differentiator buried in governance may be: audit trails for regulated industries.**
If Alizé targets accounting firms (expert-comptables) or legal practices, then audit trails, access controls, and compliance documentation are genuinely valuable. These buyers have regulatory obligations. But for a general "PME 20-50 employees," governance positioning is too abstract to land.

### Recommendation
**Governance/accountability should be demoted from positioning to proof point.**
- Remove "accountability" language from hero and headlines
- Keep audit trails, compliance features as secondary technical proof points
- Replace primary positioning with: specific outcome claims (what the agent does, how fast, how reliably)
- If targeting regulated verticals (accounting, legal), governance becomes a primary differentiator — but only for those verticals

### New Unresolved
| ID | Topic | Status |
|----|-------|--------|
| U69 | Does governance/accountability differentiate for non-regulated PME buyers? | Likely not — demote from hero positioning |
| U70 | Is there a regulated-industry sub-vertical that makes governance the core differentiator? | Potentially expert-comptables, legal — worth exploring |

---

## Agent 2: Pilot Fee Economics Challenge

### Challenged Assumption: "€6,500 pilot fee covers realistic delivery costs"

Pulse 15 established: realistic delivery is 10-15 days at €350/day. D70 proposed raising the pilot fee to €6,500 to cover this.

### Current Model Analysis

**Base delivery costs:**
| Days | Delivery Cost (€350/day) | Proposed Fee | Net Alizé |
|------|------------------------|-------------|-----------|
| 5 | €1,750 | €6,500 | +€4,750 |
| 10 | €3,500 | €6,500 | +€3,000 |
| 15 | €5,250 | €6,500 | +€1,250 |
| 20 | €7,000 | €6,500 | -€500 |

**But the model is more broken than the above suggests.**

The €3,250 cap on delivery partner exposure (D49) was designed for 5 delivery days. At 10-15 days, the delivery partner either:
1. Eats the loss (won't happen — they'll quit or do a bad job)
2. Rushes the delivery (pilot fails, no case study)
3. Bills for more days (contradicts the contract)

**The success fee doesn't work at realistic delivery hours.**
At €350/day, a delivery partner who spends 10 days delivers €3,500 in value. The success fee is €1,500 if the pilot converts. So the total theoretical package is €5,000 for 10 days = €500/day effective rate. For a qualified delivery partner with PME consulting experience, €500/day is at or below market rate in France. You'd be hiring someone cheap, not good.

**Louis's involvement is unpriced.**
The model assumes Louis delivers Month 1 manually (D23) and then a contractor takes over. But Louis's time has a real cost (his Grinto salary, opportunity cost). If Louis spends 2 weeks on the first pilot, that's a significant uncompensated cost that the pilot fee doesn't account for.

**What the pilot actually costs Alizé:**

| Cost item | Low estimate | High estimate |
|-----------|-------------|---------------|
| Louis time (10 days × €200/day opportunity) | €2,000 | €2,000 |
| Delivery partner (10 days × €350/day) | €3,500 | €3,500 |
| Infrastructure (Mastra, S3, tools) | €100 | €100 |
| Overhead (coordination, admin) | €200 | €400 |
| **Total** | **€5,800** | **€6,000** |
| Revenue | €6,500 | €6,500 |
| **Net** | **+€500-700** | **+€500-700** |

At 15 days, Alizé loses money on every pilot.

**The case study upside is priced in but not guaranteed.**
The model implicitly assumes the pilot generates a case study worth €10,000-50,000 in future business. But if the pilot fails, or the client won't give a testimonial, or the metrics aren't compelling — the upside disappears and Alizé has taken a loss.

### Key Findings
1. **€6,500 barely breaks even at 10 delivery days** — the margin is €500-700, which doesn't cover any adversity
2. **At 15 days, Alizé loses money** — so the €6,500 price assumes delivery never goes over 12 days
3. **The delivery partner success fee misaligns incentives at realistic day counts** — €350/day + €1,500 bonus caps out at €3,250, which is below market for 10 days of qualified PME consulting work
4. **Louis's time is unpriced** — the model implicitly treats his involvement as free
5. **The case study upside is speculative** — it's not revenue, it doesn't pay the bills this month

### Recommendations
1. **Increase warm network pilot fee to €8,500** — this gives €1,500-2,000 margin at 10-day delivery and covers 12 days at break-even
2. **OR reduce delivery scope aggressively** — cap delivery at 7 days, limit the pilot to one workflow, one integration, no RAG — this keeps the €6,500 price viable
3. **Price Louis's involvement explicitly** — either as part of the pilot cost, or accept that the first 1-2 pilots are marketing investments, not profit centers
4. **Renegotiate the delivery partner contract** — remove the success fee cap; pay €400-450/day market rate for qualified delivery; success fee becomes an additional bonus, not part of the base compensation

### New Decisions / Unresolved
| ID | Topic | Status |
|----|-------|--------|
| D72 | Warm network pilot fee | Raise to €8,500 OR cut delivery scope to 7 days max |
| D73 | Louis time cost | Explicitly account for Louis's involvement in pilot economics |
| U71 | Delivery partner day rate | €350 too low for qualified PME consultant; €400-450 more realistic |
| U72 | First pilot as marketing investment | Accept first 1-2 pilots lose money; write them off as customer acquisition cost |

---

## Agent 3: Vertical Strategy Frame Challenge

### Challenged Assumption: "Alizé should pick ONE vertical for Year 1"

The research has oscillated between: digital agencies → professional services → accounting/legal → back to professional services. The assumption is that Alizé needs vertical focus to build depth and defensibility.

### Key Counter-Arguments

**The vertical-first thesis assumes Alizé has more specialization than it does.**
Vertical depth means something when you have industry-specific knowledge, pre-built workflows, regulatory expertise, and relationships in that industry. Alizé has none of these at Year 1. Saying "we're focusing on professional services" without any actual professional services expertise is a marketing claim, not a differentiator. The buyer in that vertical will see through it immediately.

**What Alizé actually has is: workflow expertise.**
Louis knows how to build AI agents that automate repetitive business tasks. That's horizontal — it applies to invoice processing, CRM data entry, meeting note extraction, ticket routing, HR Q&A. These workflows exist in every industry. The expertise is in the automation, not in the vertical.

**"Pick a vertical" constrains the addressable market when Alizé has no proof points.**
If Alizé chooses professional services as the vertical, then for Months 1-6, every outreach conversation must be with professional services companies. But Louis's warm network may have only 5-10 professional services fits. Now Alizé has constrained itself to a tiny market segment before it has any evidence that this segment actually converts.

**The vertical debate is an avoidance of the harder question: what is Alizé's actual unfair advantage?**
The research keeps asking "which vertical" when the real question is "what can Alizé do that a generic AI consultant cannot?" If the answer is "we know how to build reliable agent workflows that connect to business tools," that's horizontal. If the answer is "we have pre-built agents for accounting firms," that's vertical. But Alizé doesn't have the pre-built assets yet to claim vertical depth.

**A workflow-first frame might be more honest and more defensible.**
Instead of "AI agents for professional services," Alizé could position as "AI agents that handle your repetitive operational tasks." The vertical emerges from the client's industry, not from Alizé's positioning. This is more honest because it reflects what Alizé actually does.

**The real Year 1 strategy should be: take warm network pilots that fit the ICP, learn from delivery, and let vertical insight emerge.**
Louis's warm network is the only market signal Alizé has. The right move is to map that network against the ICP (50-200 employees, 3+ tools, operational overhead), identify which contacts fit, and pursue those. If 70% of warm contacts are in e-commerce, start with e-commerce. If they're in professional services, start there. Strategy should follow evidence, not precede it.

### Recommendations
1. **Stop pre-determining the vertical.** The research doesn't have enough data to pick correctly, and picking wrong wastes Months 1-3.
2. **Use workflow type as the primary filter, not industry.** The qualification criteria should be: is this a repetitive, standardized, operational workflow with measurable ROI? If yes, it's a candidate. Industry is secondary.
3. **Map Louis's warm network first.** U52 is still unresolved. Until Louis maps his warm contacts against ICP, any vertical decision is speculation.
4. **Consider repositioning as "operational workflow automation" rather than "vertical AI agents."** This is more honest, more flexible, and more defensible at Year 1.

### New Decisions / Unresolved
| ID | Topic | Status |
|----|-------|--------|
| U73 | Is vertical-first the right frame for Year 1? | Challenged — workflow-first may be more honest |
| U74 | What is Alizé's actual unfair advantage at Year 1? | Likely workflow expertise (horizontal), not vertical knowledge |
| U52 | Louis's warm network ICP map | Still unresolved — should gate all vertical decisions |

---

## Cross-Cutting Tensions

| Tension | Resolution |
|---------|------------|
| Governance as differentiator vs. table stakes | Demote from hero; keep as proof point for regulated verticals only |
| €6,500 pilot fee vs. realistic delivery costs | Either raise to €8,500 or cut delivery scope to 7 days; first pilots may need to be written off as CAC |
| Vertical-first vs. workflow-first | Workflow-first is more honest at Year 1; vertical emerges from network mapping, not predetermined |

---

## Summary for Louis

Three critical assumptions were challenged:

1. **Governance/accountability:** This is what every vendor promises. It doesn't differentiate. Replace with specific, concrete outcome claims in external materials. Governance becomes relevant only for regulated verticals (accounting, legal).

2. **Pilot fee economics:** €6,500 barely breaks even at 10 delivery days and loses money at 15. Either raise the warm network pilot fee to €8,500 or cut delivery scope aggressively to 7 days max. The first 1-2 pilots may need to be written off as customer acquisition cost — the case study upside is speculative.

3. **Vertical-first strategy:** The vertical debate is a distraction from the harder question. Alizé's actual expertise is workflow automation (horizontal), not vertical knowledge. The right Year 1 frame is: take warm network pilots that fit the ICP, learn from delivery, and let vertical insight emerge from evidence.

---

## New Decisions Log

| ID | Topic | Decision | Source | Date |
|----|-------|----------|--------|------|
| D72 | Warm network pilot fee | Raise to €8,500 OR cut delivery scope to 7 days max | Economics agent | 2026-03-30 |
| D73 | Louis time cost | Explicitly priced in pilot economics | Economics agent | 2026-03-30 |

## New Unresolved Items

| ID | Topic | Blocker |
|----|-------|---------|
| U69 | Does governance differentiate for non-regulated PME? | Likely no — demote from positioning |
| U70 | Is there a regulated vertical where governance is core? | Expert-comptables, legal practices |
| U71 | Delivery partner day rate | €350 too low; €400-450 more realistic |
| U72 | First 1-2 pilots as marketing investment | Accept losses on early pilots as CAC |
| U73 | Vertical-first vs workflow-first framing | Workflow-first more honest at Year 1 |
| U74 | Alizé's actual unfair advantage | Likely workflow expertise, not vertical depth |
| U52 | Louis warm network ICP map | Still unresolved — gates all outreach strategy |

*Pulse 16 — 2026-03-30*
