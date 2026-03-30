# Delivery Partner Risk Debate: The Hidden Execution Flaw

## Challenged Assumption

**The delivery model assumes a delivery partner can be found, contracted, and deployed at €350/day to execute pilots that Louis has not yet learned to execute himself — and that this partner will be available when things go wrong.**

The core assumption under scrutiny: Alizé can staff delivery capacity *before* it has validated that it knows how to deliver. Louis finds a delivery partner, briefs them on a playbook that doesn't exist yet, and the partner executes flawlessly while Louis handles the relationship. This assumption has at least five unvalidated sub-components, each of which could kill the first pilot independently.

---

## Arguments For

### 1. The success-fee structure compensates for the below-market day rate

The €350/day base + €1,500 conversion bonus = €4,750 effective total (per delivery-partner-strategy.md) approaches market rate when success fees are included. A motivated delivery partner who believes in the model may accept the reduced base in exchange for upside participation.

### 2. Louis + Gabin together can cover the delivery gap

If Gabin has genuine PME consulting experience, Louis doesn't need to brief a stranger — he has a co-deliverer who already knows his working style, can be trusted immediately, and whose incentives are aligned with Alizé's success. No sourcing time, no onboarding, no trust gap.

### 3. The first pilot is low-stakes enough to absorb delivery learning

Early pilots at discounted rates with forgiving clients can absorb the learning curve. The client expects imperfection. The case study doesn't need to be flawless — it needs to exist. Louis and a delivery partner can learn by doing together.

### 4. Delivery partner is replaceable if one fails

If the first delivery partner doesn't work out, Malt has thousands of freelancers. Alizé can iterate on the partner selection. The delivery model isn't structurally dependent on any single person — it can absorb one bad hire.

### 5. The €4,500 pilot fee covers the delivery cost with margin

At 5 delivery days × €350 = €1,750 base cost (if the partner doesn't convert), the pilot still generates +€2,750 margin. Even at 10 days, the economics hold. The model is cash-positive even with execution risk.

---

## Arguments Against

### 1. €350/day is 40-50% below market for the actual profile needed

The BRIEF estimates "Typical French ESN day rate €600-900/day." The delivery-partner-strategy acknowledges this ("€550-770/day all-in") and proposes the discount. But the discount assumes a partner who:
- Has n8n/Mastra technical skills (niche, premium)
- Has French PME consulting experience (rare combination)
- Willing to accept 40% below market

**These three requirements do not overlap at €350/day.** The market for this profile is closer to €500-700/day base. At €350, Alizé will attract:
- Junior freelancers (technically competent, no PME consulting experience)
- Offshore contractors (cheap, but French business norms gap is fatal)
- Technically skilled people desperate for revenue (unreliable)

The success fee (€1,500) is supposed to bridge the gap — but success fees attract mercenaries, not partners. A competent PME consultant with AI workflow skills will not accept a 40% discount for the *chance* at a €1,500 bonus. They'll take the €600/day market rate from a company that pays it.

**Louis will not find the right profile at €350/day.** The delivery-partner-strategy document recommends €350-400 base + success fee, but even the document admits this is "40-50% discount." That discount will show in the quality of candidates.

### 2. Louis cannot brief a delivery partner on something he hasn't done

This is the fatal logical loop. D23 says Louis delivers Month 1 manually + documents. D31 (which supposedly supersedes D23) says Louis cannot solo-deliver and needs a business lead from Day 1. These are contradictory — but the deeper problem is that **neither assumes Louis has executed a pilot before the delivery partner arrives**.

Louis is being asked to:
1. Execute the first pilot successfully (so the client converts and the case study exists)
2. Simultaneously document everything (so the delivery partner can replicate it)
3. Do this without having done a pilot before

This is not a delivery model. It's a hope. The knowledge transfer gap is not "Louis documents poorly." It's "Louis has no delivery knowledge to transfer." The delivery partner receives a playbook written by someone who has never run the playbook. The partner is flying blind and Louis doesn't know enough to notice.

**The delivery-partner-strategy document (Step 5, Week 1) says "Louis observes: how does the partner ask questions?"** — Louis is the student, not the teacher. The "playbook" becomes a record of what the delivery partner did, not what Louis knows. If the delivery partner leaves after Month 1, Louis is in the same position he started: no delivery knowledge.

### 3. The D23/D31 contradiction was never resolved — it was acknowledged

D23: "Month 1: Louis delivers pilot manually + documents; Month 2 contractor executes against playbook"

D31: "Business lead needed from Day 1; Louis cannot solo-deliver (REVISES D23)"

These are on the same day, from the same process, saying opposite things. The TODO shows D31 as "RESOLVED (D23)" but the two decisions are irreconcilable. D23 assumes Louis can deliver. D31 says he cannot. **The delivery model doesn't know what it is.** The plan should not proceed until this contradiction is explicitly resolved with a single decision.

### 4. No escalation path exists when the first pilot fails

The delivery model has no defined failure mode. If the pilot goes wrong in Week 2 and the delivery partner is unavailable (sick, on another engagement, non-responsive), Louis has two options:
- Attempt to fix it himself (no delivery experience)
- Delay the pilot (client loses confidence, case study timeline slips)

**The model assumes delivery partner availability as a constant.** It does not model the realistic scenario where the delivery partner is unavailable at a critical moment. Every client engagement has critical moments. The delivery partner model has no redundancy.

### 5. Gabin's consulting background has not been validated

U41 in the TODO: "Ask Gabin directly — does he have client-facing PME consulting experience?"

This was marked as unresolved on March 30. The delivery-partner-strategy says "Step 1: Confirm Gabin's Role (Day 1-3)" — but the document was written before Day 1-3 happened. As of the current session, **Gabin's consulting background has not been confirmed.**

The entire "Louis + Gabin vs. external delivery partner" decision tree is built on an unverified assumption. If Gabin does not have PME consulting experience, the "try Gabin first, then fallback to external" plan means:
- Day 1-3 wasted confirming what was already suspected
- Day 3-10: start Malt outreach with no pilot client identified yet
- Delivery partner found Day 17-21 at the earliest
- First pilot may not start until Week 4

If Gabin *does* have PME consulting experience, Louis should be talking to him TODAY, not waiting for a calendar check. The delay in answering U41 is actively blocking the delivery timeline.

---

## Recommendation

**Do not proceed with the delivery partner model until three things are confirmed:**

### Decision 1: Resolve the D23/D31 contradiction explicitly

Pick one. Either:
- Louis delivers Month 1 alone (accepting the risks, with a fallback plan for failure)
- Louis cannot solo-deliver (in which case, who does? Gabin? An external partner? Both?)

This decision must be made before any delivery partner is sourced. The delivery model cannot be designed until the delivery team composition is defined.

### Decision 2: Answer U41 TODAY — Is Gabin's consulting background validated?

If yes: Formalize Louis + Gabin as the Month 1 delivery team. No external partner needed for the first pilot. Begin pilot immediately.

If no: Begin Malt outreach immediately with a clear brief. Do not wait. The sourcing timeline (10-17 days) means the delivery partner won't be available for Month 1 regardless — so the first pilot should be scoped for Louis + Gabin (or Louis alone with a stripped-down scope) with explicit written documentation of every decision made.

### Decision 3: Revise the €350/day assumption

€350/day will not attract the profile described in the delivery-partner-strategy. Either:
- Raise the base to €450-500/day (market rate for junior-mid, still below senior)
- Accept that the delivery partner will be junior and supplement with Louis's technical knowledge
- Structure the engagement as a revenue-share partnership rather than day-rate employment

The success-fee model is correct in principle. The base rate is the problem.

---

## New Decision (if any)

**D56: Louis must complete one delivery milestone BEFORE the delivery partner is contracted**

Before Alizé engages any external delivery partner, Louis must have:
1. Run one full discovery call with a real prospect (not a sales call — a delivery scoping call)
2. Identified one specific workflow to automate
3. Built one automated workflow end-to-end (even manually, even if ugly)

This does not require a paying client. It requires Louis to *do the job* once so he can transfer knowledge. The current model has Louis learning delivery from a delivery partner who is also learning Alizé's model. This is double-blind and produces no transferable knowledge.

**The delivery model depends on Louis having delivery knowledge. He currently does not. The first step is not finding a partner — it's Louis doing one delivery exercise.**

---

## Unresolved Questions

1. **What is Louis's actual delivery experience?** — Has he ever run a discovery call with a PME client? Mapped a business process? Managed a client through a 30-day implementation? If the answer is no, the delivery model is built on a fictional foundation.

2. **Who owns the client relationship if the delivery partner is also client-facing?** — The delivery-partner-strategy says the delivery partner runs the discovery call, process mapping, and ROI presentation. Louis is supposed to be "technical executor." But if the delivery partner is the primary client relationship owner and they are an external contractor, what happens when the partner leaves after Month 1? The client relationship leaves with them.

3. **What happens to the delivery model if the first pilot takes 15 days instead of 5?** — The €1,750 base cost assumption (5 days × €350) is not locked to any scope. If the delivery partner works 12 days, the pilot economics break. There is no scope cap in the contracting model described.

4. **Is the delivery partner's incentive aligned with quality or with speed?** — At €350/day, the partner earns more by finishing faster. But a rushed pilot = poor adoption = no conversion = no success fee. The incentive structure punishes taking the time to do it right.

5. **What is the hand-off if the delivery partner is also the one who built the playbook?** — The model assumes Louis + delivery partner deliver Month 1, then a *different* contractor executes Month 2+ against the playbook. But if the delivery partner is the one with the tacit knowledge (from delivering Month 1), the playbook will be incomplete. The Month 2 contractor inherits the limitations of the Month 1 partner, not the full scope of what delivery requires.

---

*Delivery Partner Risk Debate — Alizé — 2026-03-30*
*Subagent: Delivery Strategist*
