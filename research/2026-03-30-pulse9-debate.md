# Alizé Research Pulse 9 — 2026-03-30

**Date:** 2026-03-30
**Pulse:** 9th research pulse of the day
**Agents:** GTM Strategist, Platform Strategist, Conversion Strategist

---

## Agent 1: GTM Strategist — Outbound-Only Assumption

### Assumption Challenged
> "Outbound-only is the right Year 1 pipeline engine."

### Key Findings

**Cold outreach math is brutal for an unknown brand:**
- Cold email reply rate for unknown B2B brand: 2-3% (best case, excellent execution)
- To book 10 discovery calls/month: 333-500 outreach attempts/month = ~75-125/week = full SDR capacity
- €4,500 commitment from cold outbound: probably 1-3% of meetings held
- Cold outbound alone: ~1-4 pilots in Year 1 for an unknown brand

**Warm outreach via Grinto's network is 5-10x more efficient:**
- Reply rates: 40-60% (warm) vs 2-3% (cold)
- Discovery call rate: 60-80% (warm) vs 40-50% (cold)
- Pilot conversion: 30-50% (warm) vs 5-10% (cold)

**Warm network inventory: 70-135 Year 1 prospects:**
- Grinto clients/partners: 10-30
- Louis's personal network: 20-40
- French tech ecosystem: 30-50
- Local business networks: 15-30

**Revised pipeline math (warm-first):**
- 70 contacts × 40% reply = 28 discovery calls
- 28 calls × 35% pilot conversion = ~10 pilots Year 1
- 10 pilots × 75% retainer = 7-8 retainer clients

**Conclusion:** Network-first, outbound supplement. Warm outreach is the foundation that makes cold outbound work. Do not sequence these as equals — warm comes first.

---

## Agent 2: Platform Strategist — Platform Build Assumption

### Assumption Challenged
> "Alizé must build its platform (agent runtime, MCP, RAG) in parallel with manual pilot delivery."

### Key Findings

**Pure-service model for 6-12 months is viable and lower risk:**
- Use n8n, Make.com, direct API calls — no custom platform needed for Year 1
- AI automation consulting day rate in France: €800-1,500/day
- Louis alone could bill €15-20K/month at 60% utilization
- Service margins: 40-60%

**Platform costs (honest accounting):**
- Backend engineer: €80-120K/year
- Agent runtime MVP: 3-6 months dev
- Year 1 investment: €100-250K
- Opportunity cost: engineering bandwidth diverted from client delivery

**What platform actually buys:**
- Scalability: realized at 12-18 months
- Margin expansion: realized at 18-24 months
- IP/Defensibility: if it works
- Year 1 platform investment is a Year 2+ optimization problem

**Build risk without validation:**
- Platform built in vacuum = wrong abstractions
- €150-250K burn without revenue validation
- Sunk cost fallacy when evidence arrives
- Mastra cloud, Yeai, crewAI Pro are already competing in this space

**Recommendation:** Kill parallel platform build. Service-first for Months 1-12. Platform decision at Month 12+ based on real renewal data. Use n8n/Make.com in the interim.

---

## Agent 3: Conversion Strategist — Natural Retainer Conversion Assumption

### Assumption Challenged
> "The 30-day pilot naturally converts to monthly retainer because ROI is self-evident."

### Key Findings

**Actual SaaS trial-to-paid conversion for workflow-change products in French PME:**
- General SaaS: 10-25% (self-serve), 30-60% (sales-assisted)
- High-touch, workflow-change-required: 15-35%
- French PME is particularly resistant to workflow disruption
- Conservative assumption for Alizé: 35-50% conversion rate

**The ROI math doesn't work on "hours saved":**
- 3 hours/week saved = €75-150/week labor value = €3,900-7,800/year
- Alizé retainer: €9,600-18,000/year
- Net annual value from strict labor perspective: -€5,700 to -€10,200/year
- "3 hours saved" is NOT ROI-positive at €800-1,500/month

**The framing that works:** Outcome ownership, not time savings. "We handle this completely so your team focuses on revenue-generating work." The client must feel they have transferred operational ownership.

**Behavioral difference between "works" and "converts":**
- Works: agent does task correctly, client impressed, cancels after 30 days
- Converts: agent removes client from workflow, client can't absorb the gap, asks for another workflow at day 30

**Current conversion mechanism is the weakest design:**
- Passive: client watches reports for 30 days
- No urgency trigger: "think about it another month" is always available
- Default = "pause" = churn

**Proposed conversion mechanism — hybrid outcome guarantee:**
- €4,500 pilot (non-refundable)
- Month 1 retainer at €1,200/month
- If client not satisfied at day 30: €600 refund (50% money-back)
- After month 1: month-to-month, no commitment

**Conversion threshold: 55%+ needed for viability.** At 35% conversion, Alizé needs 17-20 pilots/year just to hit 6-7 retained clients.

---

## Decisions Reached This Pulse

| ID | Topic | Decision | Source |
|----|-------|----------|--------|
| D26 | GTM channel priority | Network-first (warm outreach via Grinto's network) as primary Year 1 channel; cold outbound secondary, Month 2+ | GTM agent |
| D27 | Platform strategy | Kill parallel platform build; pure-service-first for Months 1-12; platform decision deferred to Month 12+ | Platform agent |
| D28 | Conversion mechanism | Hybrid outcome guarantee: €4,500 pilot + month 1 €1,200 retainer with 50% refund if unsatisfied + month-to-month after | Conversion agent |
| D29 | Pilot ROI framing | Reframe from "hours saved" to "operational ownership transfer" — client must feel they have delegated the task entirely | Conversion agent |
| D30 | Pilot qualification | Workflow must be operationally critical (not "nice to have") — reject "nice to have" workflow pilots | Conversion agent |

---

## New Unresolved Items

| ID | Topic | Options | Blocker |
|----|-------|---------|---------|
| U28 | Month 1 activity mapping | Who specifically does Louis contact first? What's the outreach sequence? | Needs Louis to list 50 warm contacts |
| U29 | SDR timing | When to hire cold outbound support — Month 3-4 after case studies exist? | Budget decision |
| U30 | n8n vs Make.com | Which tool for interim service delivery? | Technical evaluation |
| U31 | Platform decision criteria | What specific signals at Month 12 trigger "build platform" vs "compose" vs "stay service"? | Need to define thresholds |

---

## Recommendations for Louis

1. **Flip the GTM priority this week:** Month 1 = warm outreach, not platform build. Map 50-75 warm contacts in Louis's network. No cold outbound SDR until Month 3-4.

2. **Kill the platform build plan for Year 1:** Use n8n or Make.com for client automations. Document what's painful and manual — those become the platform roadmap at Month 12+.

3. **Restructure the pilot conversion mechanism:** Add explicit conversion design to pilot delivery. By day 20, the client must feel operational dependency. Month 1 retainer includes a 50% refund guarantee to remove the "do I stay or go" decision friction.

4. **Qualify pilots on workflow criticality:** Reject "nice to have" workflow pilots. Accept only operations that the client would genuinely notice losing. The difference between 40% and 70% conversion is which workflow gets automated.

---

*Pulse 9 complete — 2026-03-30 12:08 UTC*
