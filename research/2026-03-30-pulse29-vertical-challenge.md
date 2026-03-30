# Pulse 29 — Expert-Comptable Vertical: Assumption Challenge

**Date:** 2026-03-30  
**Pulse:** 29  
**Topic:** Expert-Comptable Vertical Thesis (D126, D135, D81REVISED)  
**Assumption Challenged:** Expert-comptables are the right first vertical because: (1) governance gap disqualifies Copilot, (2) regulatory workflows provide clear case study metrics, (3) Louis's network has connections here.

---

## Challenge 1: The Governance Wedge Thesis Is Overstated

**D126 claims** the expert-comptable governance gap disqualifies Copilot, making this vertical a safe beachhead.

**The problem:** D127 already partially conceded this — governance gap is "overstated for 70% of ICP." But D126 doubles down on expert-comptables specifically without explaining why *this* 30% has a disqualifying gap while the rest of the market doesn't.

**Counterevidence:**
- French expert-comptable firms (cabinets) range from 1-5 person boutiques to 50+ person firms. The governance requirements are not uniform. A 3-person cabinet does not have the same data governance posture as a 30-person one.
- Copilot's actual governance limitations (data residency, DLP, audit logging) are problems for *enterprise* Copilot deployments — not the Microsoft 365 Copilot most small cabinets use, which is largely tenant-isolated and admin-configured.
- The EU AI Act compliance story is 2027-2028 (D42). The governance wedge is a future argument, not a present one.
- The expert-comptable sector has already adopted Copilot broadly. The ordinal IE "expert-comptable" has been using Microsoft tools for 15+ years. The governance anxiety is lower, not higher, than greenfield sectors.

**Verdict:** The governance wedge may justify *positioning* Alizé as governance-aware for regulated industries. It does NOT justify expert-comptables as a *vertical beachhead* specifically because of governance. The argument is backwards — Alizé should pursue sectors where governance actually disqualifies alternatives, not a sector where governance is used post-hoc to justify a network-driven choice.

---

## Challenge 2: The Workflow Is Still Not Specific Enough

**D81REVISED** says "expert-comptable administrative workflow" — but what does this actually mean?

**The problem:** "Administrative workflows around client management/regulatory reporting" describes 40% of what an expert-comptable does. It is not a workflow, it is a department. D124 correctly identified that meeting report generation was too thin. D81REVISED replaced it with something equally vague.

**What a real first workflow requires:**
- (a) Measurable ROI in 30 days — which specific task, measured how?
- (b) No sensitive client data exposure — which tasks at an expert-comptable firm don't involve client financial data?
- (c) Clear case study value — a story a future prospect immediately understands

**Counterevidence from the debate log itself:**
- D81 was REVISED because meeting reports were too thin (€200/month ROI stories). 
- Expert-comptable firms touch client financial data for everything: tax returns, payroll, bilan, TVA declarations, social declarations. The "no sensitive data" requirement essentially eliminates 90% of their actual work.
- The remaining 10% (internal firm admin: scheduling, internal文档, team coordination) is operationally trivial and produces negligible ROI.

**Specific workflows that were supposedly considered and rejected:**
- Customer service (sensitive data)
- Sales admin (sensitive data)  
- HR Q&A (sensitive data)
- Meeting report generation (too thin)

**What this reveals:** The expert-comptable vertical was chosen for network reasons, not workflow reasons. The workflow justification was assembled after the vertical was decided.

**Verdict:** D81REVISED is not a workflow choice — it is a placeholder. Without naming the exact task (e.g., "automated TVA declaration preparation from extracted source documents" or "client document collection status tracking"), the workflow thesis does not hold.

---

## Challenge 3: Network Dependency Makes This a Premature Lock-In

**D135 modified D126** to say "go where Grinto relationships exist first." This is an honest revision. But it reveals the actual thesis: the vertical is expert-comptables because Louis has relationships there, not because the vertical is optimal.

**The problem:** U123 and U129 are still unresolved. The debate log does not actually confirm Louis has expert-comptable warm contacts. D135 says "validated through warm intros" — but U138 flags this as still needing validation.

**If Louis's expert-comptable network is thin (< 5 warm contacts):**
- D47 blockers require: live website + completed pilot + ICP locked + delivery confirmed before cold outreach
- Expert-comptable association outreach (U131) requires case studies Alizé doesn't have yet
- Cold outbound to expert-comptables is blocked until Month 3+ at earliest
- This means the vertical choice is locked in based on an unverified assumption about network access

**Counterevidence:**
- The debate log explicitly says "network-first warm outreach" is the GTM strategy (D26, D130)
- If Louis's warm network doesn't map to expert-comptables, the vertical choice collapses on its own logic
- The vertical was adopted before U52 (ICP network mapping) was completed — sequence error

**Verdict:** D126/D135 lock Alizé into a vertical before validating network access. If U123 is unresolved, this is the most consequential unresolved item in the entire debate log. Everything else is downstream of this.

---

## Challenge 4: Domain Knowledge Gap Is Potentially Fatal for the First Workflow

**U124** is unresolved: "Does Louis have domain knowledge for accounting firm admin tasks?"

**The problem:** D125 sets a €1,500/2-week diagnostic engagement. This diagnostic requires Louis to:
1. Identify which expert-comptable workflow to automate
2. Credibly advise on AI automation feasibility
3. Scope the engagement correctly so the 2-week period produces a meaningful result

Louis has no identified expert-comptable domain expertise. "Delivery rehearsal" (D122) was framed as internal roleplay — but that doesn't build domain knowledge, it builds delivery mechanics.

**What happens if the first workflow is scoped wrong:**
- An expert-comptable firm will judge Alizé on whether the agent understands their actual practice, not whether it has good AI
- Mis-scoping a TVA extraction workflow vs a document collection workflow vs a reporting workflow produces completely different pilot outcomes
- A failed or irrelevant first pilot at an expert-comptable firm (Louis's supposed network contact) burns the only warm intro he has in that vertical

**Counterevidence:**
- D113 (Louis delivers pilot 1 solo) and D122 (simulated rehearsal) were both debated — the debate log itself shows uncertainty about Louis's delivery capability
- Domain ignorance compounds with regulatory complexity — expert-comptable work is governed by specific French accounting standards (PCG, plan comptable). Getting the wrong workflow wrong is worse than no pilot.

**Verdict:** U124 should have been resolved before D126 was adopted. Without domain knowledge (or an embedded domain expert), the first expert-comptable pilot risks being scoped around what Louis *can* build, not what the client *needs*. This is the wrong way to build a case study.

---

## Verdict

**The expert-comptable vertical thesis needs modification, not abandonment — but the modification is more fundamental than D135 implies.**

The thesis was built on three legs:
1. Governance gap → leg is weak (D127 already conceded this is overstated)
2. Clear workflow metrics → leg is missing (D81REVISED is still not a specific workflow)
3. Network connections → leg is unverified (U123/U129 unresolved)

**Two genuine flaws:**
1. **The vertical was chosen before the network was mapped.** D126 was adopted in Pulse 25. U52 (ICP network mapping) is still unresolved. This is a sequence error — the most consequential one in the debate log.
2. **The workflow was chosen before the domain knowledge existed.** D81REVISED names "expert-comptable administrative workflow" as the first use case. Without naming the specific task, the case study thesis collapses — you can't produce a compelling case study from a vague workflow.

**What the thesis needs:**
- Resolve U123 first (Louis's expert-comptable network size). If < 5 warm contacts, the vertical requires cold outreach Alizé can't do yet. Move to a vertical where the network IS confirmed.
- Name the specific workflow. Not "client management/regulatory reporting" — a specific task with a specific ROI metric. E.g., "extraction and validation of TVA data from supplier invoices for monthly declarations."
- Add domain knowledge as a prerequisite. Either Louis learns the domain (4-6 weeks of reading + interviews) or an expert-comptable practitioner is embedded in the first pilot.

---

## What This Means for the TODO

**Immediate (before any expert-comptable outreach):**
- U123/U129 must be resolved this week. Not "noted." Resolved. How many warm expert-comptable contacts does Louis actually have? If the answer is < 3, D135's logic doesn't hold and a different vertical is needed.
- U124 needs a concrete answer: how does Louis build expert-comptable domain knowledge before the first diagnostic? Reading material? Informational interviews with practitioners? This cannot be skipped.

**If expert-comptable vertical is confirmed:**
- The first specific workflow must be named in writing. "Administrative workflow" is not a scope. Name the task.
- The first pilot contract must include domain expert access — either embedded in the client organization or as an advisor to Alizé.

**If expert-comptable vertical is invalidated by network mapping:**
- Pivot to a vertical where Louis's warm network IS confirmed (D26 network-first GTM is the right mechanism for this discovery)
- Apply the same workflow specificity standard: name the exact task before committing to the vertical

**The real sequencing problem:** Alizé has been debating vertical strategy for 29 pulses without completing the network mapping that would make vertical choice evidence-based rather than assumption-based.
