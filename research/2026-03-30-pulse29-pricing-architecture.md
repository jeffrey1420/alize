# Pulse 29: Pricing Architecture Challenge

**Date:** 2026-03-30
**Pulse:** 29
**Topic:** Alizé Pricing Architecture
**Assumptions Challenged:** D125, D43, D72, D110, D123, D138

---

## Executive Summary

The pricing architecture evolved across 28 pulses from a €490 diagnostic → €4,500 pilot → €800-1,500/month managed service to a €1,500/2-week diagnostic → tiered €1,200-2,500/month managed service. The trajectory is right, but each component deserves a fresh challenge. Zero of these prices have been validated against actual French PME buyer behavior. All are theoretical.

---

## Challenge 1: The €1,500/2-Week Diagnostic Price (D125)

### The Assumption
D125: "€1,500/2-week diagnostic engagement (€750/day), framed as 'Diagnostic + Proof of Concept'"

### The Problem

**€1,500 is simultaneously too expensive and too cheap for an unproven brand.**

**Too expensive for a cold/warm first touch:**
- Alizé has zero published case studies, zero named clients, zero third-party validation
- At €1,500, a French DG/COO is buying a promise from a stranger they've never heard of
- The decision risk at €1,500 is "what if this person disappears after I pay them?"
- A French PME buyer meeting Alizé for the first time has no reference point for whether €1,500 is合理 or a scam

**Too cheap to signal quality:**
- Below €1,000: "This must not be very good if they're charging this little"
- €1,000-2,000: The "too cheap to trust, too expensive to try" gap
- Above €2,500: Signals premium, but requires credibility the brand doesn't have

**The day-rate math is also wrong:**
- €750/day implies Louis is a €150,000/year consultant (750 × 200 working days)
- No French PME buyer believes a solo founder is worth €150k/day-rate equivalent
- Agentova charges €49-99/month. ChatGPT Teams is €22/user. The buyer is already anchored low.
- €750/day is 7-15x the perceived value of a generic AI tool subscription

### The "Too Cheap to Trust" Gap — Specific Analysis

| Price Point | Buyer Perception | Behavior |
|-------------|-----------------|----------|
| €0 | Desperation, no skin in the game | High curiosity, low commitment, maximum free-rider problem |
| €200-490 | Cheap consultation | Some commitment, but easily abandoned |
| €1,000-1,500 | "Risky" — skin in game but unclear ROI | High dropout rate after payment, low conversion |
| €2,000-3,000 | Premium signal | Only if brand credibility exists |
| €3,000-5,000 | "Serious investment" | Requires social proof, case studies, references |

**The gap:** There is no price point between €490 and €3,000 that is both accessible AND credible for an unknown brand. D125 lands directly in the gap.

### Louis's Time Cost Reality
- Louis's stated opportunity cost: €200/day (D73)
- At €1,500 diagnostic, Louis earns €200/day × 5 days = €1,000 for his time
- The remaining €500 must cover: prep, report writing, discovery, follow-up
- Net margin per diagnostic at cost: -€500 to -€1,000 (Louis is subsidized)

### Verdict on Diagnostic Price
**The diagnostic should be either €0 (lead generation) or €2,500-3,000 (premium signal).**

The €0 option is a high-effort discovery call with a written ROI estimate included — free because the goal is qualification, not revenue. The €2,500-3,000 option only works if the framing is "we only take 4 clients per quarter" scarcity + Louis's specific credentials.

**Recommendation: FREE diagnostic call (30 min) + €490 written deliverable credit toward pilot.**
This kills the "too cheap to trust" problem by making the in-person interaction free (reducing friction to say yes) while the written deliverable adds perceived value without requiring upfront cash from a brand with zero trust.

---

## Challenge 2: The Tiered Monthly Model (€1,200-2,500/month)

### The Assumption
D43/D138: €1,200-1,500/month starter tier (single workflow) + €2,000-2,500/month multi-workflow tier

### The Problem: Budget Reality for 50-200 Person French PME

**The ROI math looks good on a spreadsheet. Does it survive a real budget conversation?**

French PME at 50-200 employees do not have a discretionary "AI budget." Every euro comes from:
1. Operating budget (the P&L line item that already exists)
2. An existing vendor contract that gets replaced
3. A headcount budget that gets reallocated

**What does €1,500/month mean to a 100-person French company?**
- Monthly payroll for 100 people at €3,500 average: €350,000
- €1,500/month = 0.04% of monthly payroll — trivially small
- But: that money doesn't come from "AI budget." It comes from "software licenses" or "consulting" or "operational overhead."
- The buyer must justify reallocating existing budget, not adding new budget

**The tiered pricing confusion problem:**
- "Which tier do I need?" = the first conversation after someone sees the pricing page
- Without clear scope definition, tiered pricing creates indecision
- Decision paralysis = lost sale

**What does €1,500/month buy relative to alternatives?**

| Alternative | Monthly Cost | Notes |
|-------------|-------------|-------|
| Part-time admin assistant | €1,500-2,000 | 20 hrs/week, real human, grows with company |
| ChatGPT Teams (50 users) | €1,100/month | €22/user, no integration, no governance |
| Agentova (1 agent) | €99/month | Self-service, no deployment |
| Intern | €1,500-1,800 | Real employee, managed by someone |
| External consultant (1 day/week) | €2,400-3,600 | €600-900/day, actual expertise |

**The €1,500/month Alizé agent must beat all of these.** The pricing only works if:
1. The ROI is measurable and > 3x within 90 days
2. The "no management required" claim is true
3. The switching cost from existing tools is acceptable to the buyer

**The tier structure itself creates a problem:**
- Starter tier at €1,200-1,500/month: Single workflow, 1 integration
- Multi-workflow at €2,000-2,500/month: 2-3 workflows, more integrations
- The gap between "I need one workflow" and "I need three workflows" is €500-1,000/month
- Most Year 1 clients will be at the starter tier, generating €14,400-18,000/year
- At that revenue level, 5 clients = €72,000-90,000 ARR — barely viable for Louis solo + contractor

**The real problem with tiered pricing:**
The tier names (starter/multi-workflow) describe what Alizé delivers, not what the client gets. Buyers think in problems ("I need my invoices processed automatically") not workflows. Tiered pricing that requires the buyer to translate their problem into "how many workflows do I need?" adds friction at exactly the wrong moment.

### Verdict on Tiered Pricing
**Kill the "starter/multi-workflow" naming. Replace with outcome-based tiers:**
- "Essential" (€1,200/month): One critical workflow, one integration, email support
- "Professional" (€1,800/month): Two workflows, up to three integrations, monthly check-in, priority email
- "Business" (€2,500/month): Three workflows, unlimited integrations, dedicated Slack, weekly metrics

This is still tiered, but the naming signals what you GET, not what Alizé does.

**More importantly: the real issue is not the tier structure. It's whether €1,200/month is sustainable at 5-10 clients with Louis + 1 contractor.**
At 5 clients × €1,200 = €6,000/month revenue. Minus Louis's opportunity cost (€0 if already funded) minus contractor (€350-400/day × 2 days/month = €700-800). Minus tools/infrastructure (€150). Net: €5,000-5,150/month. That's viable only if Louis isn't paying himself a market salary.

---

## Challenge 3: First 3 Pilots as CAC (D110/D123)

### The Assumption
D110: "First pilot pricing: €1,500-2,000 for pilots 1-3 (CAC model)"
D123: Revised to "Pilot 1 = free performance test; Pilot 2+ = full price with case study rights"
D125: Revised again to "€1,500/2-week diagnostic engagement"

### The Unit Economics Problem — Specific Numbers

**D110 CAC model at €1,500/pilot:**
- Louis's time: 10 days × €200/day opportunity cost = €2,000
- Delivery partner (if used): €350/day × 10 days = €3,500
- Total cost per pilot: €3,500-5,500
- Revenue: €1,500
- **Loss per pilot: €2,000-4,000**

| Pilots | Revenue | Louis Time Cost | Delivery Cost | Total Cost | Net Loss |
|--------|---------|---------------|---------------|------------|----------|
| 3 pilots | €4,500 | €6,000 | €10,500 | €16,500 | **-€12,000** |
| 5 pilots | €7,500 | €10,000 | €17,500 | €27,500 | **-€20,000** |
| 10 pilots | €15,000 | €20,000 | €35,000 | €55,000 | **-€40,000** |

**The "Louis time is free" fiction:**
- If Louis is doing this as a side project while employed at Grinto, his time has zero financial cost
- But his available hours are not infinite — and he's already at capacity (D91/D82)
- 10 days of Louis's time across pilots = 2+ weeks of full-time work
- If Louis is the bottleneck (D82 confirmed), CAC pilots consume the scarce resource that gates all other progress

**How many CAC pilots can Louis personally fund?**

Assuming Louis has personal runway for 3 months, living expenses covered, and can allocate €2,000/month to Alizé:
- Monthly Alizé time budget: ~10-12 days (remaining after Grinto + web agency)
- Each pilot requires 10-15 days (D70 confirmed)
- Louis can deliver 1 pilot every 4-6 weeks at maximum dedication
- 3 CAC pilots = 30-45 days of Louis's time over 3-4 months
- Financial outlay: €3,000-5,000 in opportunity cost (assuming opportunity cost = €100-150/day for a side project)
- **Louis can personally fund 3-4 CAC pilots before Alizé needs to generate revenue**

### The Conversion Dependency

The CAC model only makes sense if:
1. Diagnostic → pilot conversion rate is high (D28 assumed 35-50%, but this is unvalidated)
2. Pilot → monthly retainer conversion rate is high (assumed 60-80%, also unvalidated)
3. The case study value exceeds the loss per pilot

**What "high conversion rate" actually means:**
- 35-50% diagnostic-to-pilot: For every 2 diagnostics, 1 buys a pilot
- 60-80% pilot-to-retainer: For every 5 pilots, 3-4 convert to monthly

If both hold:
- 10 diagnostics → 4-5 pilots → 2.5-4 retainers
- 10 diagnostics × €1,500 = €15,000 diagnostic revenue
- 4 pilots × €1,500 = €6,000 pilot revenue (but CAC model means this is artificially low)
- 3 retainers × €1,200/month × 12 months = €43,200/year potential revenue from cohort

**The math only works if the conversion funnel is real.** There is zero data supporting these conversion rates for an unproven brand in an emerging category.

### CAC Model — Revised Recommendation

**The CAC model is right in principle, wrong in execution.**

D125's €1,500 diagnostic is a better CAC structure than the original D110 €1,500 pilot — because:
1. Diagnostic requires less commitment from the buyer than a full pilot
2. The diagnostic creates the relationship and scoped opportunity for the pilot
3. The diagnostic is deliverable without a full deployment (lower risk for Alizé)

**But the math is still wrong:**
- €1,500 for a 2-week diagnostic = €750/day = loss leader
- The diagnostic should be FREE as lead generation, with a €3,000-5,000 pilot commitment from day 1 of discovery

**Revised model:**
- **Step 1:** Free 30-min discovery call (qualification, relationship building)
- **Step 2:** €3,000 diagnostic + proof-of-concept (2 weeks, deliverable: scoped workflow working on their data, with measurement framework)
- **Step 3:** €7,500 pilot deployment (only if diagnostic delivered value)
- **Step 4:** €1,200-1,500/month retainer

This means the first 2-3 "pilots" at €3,000 are still subsidized (loss of ~€2,000-3,000 each) but with a clear commercial structure that doesn't signal desperation.

---

## Challenge 4: The Diagnostic → Pilot → Retainer Funnel

### The Assumption
D3: "Diagnostic → Pilot → Monthly retainer" funnel
D28: "35-50% diagnostic-to-pilot conversion rate" (asserted, not measured)
D29: "Pilot-to-retainer: must architect for operational dependency"

### The Conversion Rate Problem

**There is zero data supporting any conversion rate for Alizé specifically.**

The 35-50% diagnostic-to-pilot figure (D28) is pulled from consulting industry averages, not from:
- French PME buyers in 2026
- AI agent services (emerging category with no benchmarks)
- Unknown brands without case studies

**What actually drives conversion in a new service category:**

| Factor | Impact on Conversion |
|--------|-------------------|
| Brand trust | High — but Alizé has zero |
| Social proof | Very high — Alizé has zero |
| Price relative to perceived risk | High — €1,500 diagnostic is moderate risk |
| Outcome clarity | High — poorly defined outcomes kill conversion |
| Urgency | Medium — French PME buyers rarely feel urgency for AI |
| Relationship warmth | High — warm intro dramatically increases conversion |

**The funnel assumes buyers move linearly from diagnostic to pilot to retainer. In reality:**

1. **Diagnostic → No follow-up:** 40-60% of prospects disappear after a discovery call, even a paid one. The written deliverable helps but doesn't guarantee the next conversation.

2. **Pilot → Retainer decision:** This is the hardest transition. The buyer must feel operational dependency — not just "this is nice" but "I cannot go back to how we did this before." Achieving that requires the pilot to be scoped around a genuinely critical workflow (not "nice to have" — D30 already flagged this).

3. **Retainer duration:** Even if they convert, French PME buyers expect exit clauses. D54 introduced "12-month with month-3 exit" which is more realistic but reduces LTV from 12 months to 3-4 months guaranteed.

### The Real Funnel Problem

**The funnel assumes Alizé controls the buyer's decision process. It doesn't.**

French PME buying behavior:
- Decision maker is often the founder/DG directly (not a committee)
- buying cycle for €1,500-5,000 services: 2-6 weeks
- Buying cycle for €1,500+/month recurring: 4-12 weeks
- Most PME will not commit to €1,500/month without seeing the pilot working for 60+ days

**The real funnel is more like:**
- 10 discovery calls → 6 agree to written proposal → 4 sign diagnostic → 2 complete diagnostic → 1 signs pilot → 0.7 converts to retainer
- **Real conversion: ~10% of discovery calls to retainer**

At 10%:
- 30 discovery calls → 3 retainers at €1,200/month
- 30 discovery calls × 30 min each = 15 hours of Louis's time
- 3 retainers × €1,200 × 12 = €43,200 potential annual revenue
- But first year is likely month-to-month after month 3 exit, so €1,200 × 3 months × 3 clients = €10,800 guaranteed first-year revenue

**This is a very different picture than the "35-50% conversion" model assumed in D28.**

### What the Funnel Actually Requires

For the funnel to work, Alizé must:
1. Qualify ruthlessly — only pursue operationally critical workflows, not nice-to-have
2. Create operational dependency in the pilot — the client must feel pain without the agent
3. Have a commercial structure that survives the 4-12 week buying cycle
4. Price so the pilot → retainer jump is a no-brainer, not a new decision

**The "operational dependency" requirement is the key insight from D29.** If the pilot is scoped correctly:
- The client's team will build habits around the agent's output
- Removing the agent will create a workflow gap
- The retainer becomes "how do we keep this running" not "should we sign another contract"

Without operational dependency, the retainer decision is always "can we negotiate the price down?" or "should we try to build this ourselves?"

---

## Unit Economics Table: CAC Pilot Model Sustainability

### At D125's €1,500/2-week diagnostic structure:

| Scenario | # Pilots | Revenue | Louis Days | Louis Cost (@€200/day) | Contractor Days (@€400/day) | Contractor Cost | Total Cost | Net Margin | Verdict |
|----------|---------|---------|-----------|------------------------|----------------------------|-----------------|------------|------------|---------|
| 3 pilots | 3 | €4,500 | 15 | €3,000 | 15 | €6,000 | €9,000 | **-€4,500** | Unsustainable |
| 5 pilots | 5 | €7,500 | 25 | €5,000 | 25 | €10,000 | €15,000 | **-€7,500** | Unsustainable |
| 10 pilots | 10 | €15,000 | 50 | €10,000 | 50 | €20,000 | €30,000 | **-€15,000** | Unsustainable |
| 10 pilots (Louis solo, no contractor) | 10 | €15,000 | 50 | €10,000 | 0 | €0 | €10,000 | **-€5,000** | Marginally sustainable if Louis's time is free |

### At revised €3,000 diagnostic structure:

| Scenario | # Pilots | Revenue | Louis Days | Louis Cost (@€200/day) | Contractor Days (@€400/day) | Contractor Cost | Total Cost | Net Margin | Verdict |
|----------|---------|---------|-----------|------------------------|----------------------------|-----------------|------------|------------|---------|
| 3 pilots | 3 | €9,000 | 15 | €3,000 | 15 | €6,000 | €9,000 | **±€0** | Breakeven |
| 5 pilots | 5 | €15,000 | 25 | €5,000 | 25 | €10,000 | €15,000 | **±€0** | Breakeven |
| 10 pilots | 10 | €30,000 | 50 | €10,000 | 50 | €20,000 | €30,000 | **±€0** | Breakeven |

**Key insight:** At €3,000 diagnostic, Alizé breaks even on delivery costs if Louis prices his time at opportunity cost and contractor is €400/day. This is not profit — it's cost recovery. The real value of the diagnostic is the pilot opportunity, which at €7,500 (post-diagnostic) generates real margin.

### At full commercial model (diagnostic → pilot → retainer):

| Phase | Price | Cost | Margin |
|-------|-------|------|--------|
| Diagnostic | €3,000 | €5,000 (Louis + contractor) | -€2,000 (subsidized) |
| Pilot | €7,500 | €8,000 (10 days Louis + contractor) | -€500 (subsidized) |
| Retainer/month | €1,200 | €400 (2 days contractor/month) | +€800/month |

**The economics make sense only if:**
1. 30% of diagnostics convert to pilots
2. 70% of pilots convert to 12-month retainers
3. Churn after month 12 is <30%

At those rates: 10 diagnostics → 3 pilots → 2.1 retainers × €800/month × 12 months = **€20,160 margin from one cohort of 10 diagnostics**

Break-even for the diagnostic/pilot subsidy: 10 diagnostics × -€2,500 average loss = **-€25,000 investment to generate €20,160/year recurring.**
Payback period: ~15 months. Not great, but not insane for a Year 1 SaaS-like business with service delivery.

---

## Verdict: Concrete Revised Pricing Recommendation

After challenging all four assumptions, here is the committed recommendation:

### Revised Pricing Structure

**Entry:** Free 30-min discovery call (not €1,500)
- Purpose: Qualification, relationship, discovery of operationally critical workflow
- Louis's time investment: 30 min
- No deliverable, no report — just a conversation that qualifies fit

**Diagnostic + Proof of Concept: €2,500 (not €1,500)**
- 2 weeks, Louis + contractor
- Deliverable: One working agent on one operationally critical workflow, measured against agreed KPIs
- Includes: Integration with 1-2 business tools, RAG on up to 100 documents, team training session
- Credibility signal: €2,500 is expensive enough to signal "we're serious" but not so expensive it requires committee approval
- Credit: €1,000 credited toward pilot if client proceeds

**Pilot Deployment: €7,500 (not €1,500-2,000)**
- 3-4 weeks setup + 30 days monitoring
- Deliverable: Agent in production, measured against pilot KPIs
- This is where real margin lives. The diagnostic is a loss leader. The pilot must generate margin or the business doesn't work.
- At €7,500 and 12 days delivery: breakeven at €625/day blended rate

**Monthly Retainer: €1,200-1,800/month (tiers renamed, not repriced)**
- Essential (€1,200): One workflow, email support
- Professional (€1,500): Two workflows, bi-weekly check-in, Slack support
- Business (€1,800): Three workflows, weekly metrics, dedicated Slack channel

**First 3 clients are NOT CAC at €1,500.**
They are diagnostic-subsidy clients at €2,500 (loss of ~€2,000 each) with a clear path to pilot at €7,500. Louis should target 3 diagnostics in Month 1, convert 2 to pilots, convert 1 to retainer. That's a viable Month 1-3 outcome, not 3 pilots as free CAC.

---

## What This Means for the TODO

1. **Kill the €1,500 diagnostic (D125).** Replace with free discovery call + €2,500 diagnostic with credit mechanism.
   - **Impact:** Reduces barrier to first conversation. Louis can have more discovery calls without financial risk to the buyer.

2. **Kill the CAC framing for pilots 1-3 (D110).** The language "first 3 as CAC" signals desperation and sets a price anchor that will be hard to raise later.
   - **Impact:** First pilots are subsidized diagnostics, not charity. €2,500 is still below cost but structured as a commercial engagement.

3. **Kill the "starter/multi-workflow" tier naming.** Replace with outcome tiers that describe what the client gets, not what Alizé does.
   - **Impact:** Reduces "which tier do I need?" friction on the pricing page.

4. **Add explicit conversion rate assumptions to the business model.** The D28 assumption of 35-50% diagnostic-to-pilot conversion is not validated. Model the business at both 20% and 50% to understand sensitivity.
   - **Impact:** Forces honest conversation about how many discovery calls are needed to hit 3 pilots.

5. **Louis must deliver the first diagnostic himself (D113 confirmed).** The delivery rehearsal is not optional — it's the only way Louis learns enough to supervise contractors and document the playbook.
   - **Impact:** Louis's Month 1 calendar is 60-70% Alizé, not a side project at the margins.

6. **The business model only works if pilot → retainer conversion is >60%.** This requires the pilot to be scoped around an operationally critical workflow (not "nice to have") and the client to feel operational dependency by day 30.
   - **Impact:** Alizé must reject pilots scoped around low-criticality workflows even if the client wants them. This is a commercial discipline requirement.

---

## Summary of Revised Pricing

| Offer | Price | Change from D125/D43 |
|-------|-------|---------------------|
| Discovery call | FREE | Was €1,500 |
| Diagnostic + PoC | €2,500 | Was €1,500 |
| Pilot | €7,500 | Was €1,500-2,000 (CAC) |
| Essential tier | €1,200/month | Was €1,200-1,500 |
| Professional tier | €1,500/month | Was €2,000-2,500 |
| Business tier | €1,800/month | Was not offered |

**Key shift:** Move the commercial interaction earlier (free discovery, not €1,500) and raise the pilot price to €7,500 to fund the business. The €2,500 diagnostic is a bridge, not the destination.

---

*Pulse 29 — Pricing Architecture Agent — 2026-03-30*
