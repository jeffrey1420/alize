# CHALLENGE: D100 — "Internal Pilot Target = Web Agency (Maëli/Gabin)"

**Pulse:** 21
**Decision:** D100 — Internal pilot target = web agency (Maëli/Gabin web agency for Month 1 delivery rehearsal)
**Status:** UNCHALLENGED — U95 explicitly flags this as unresolved

---

## CRUX OF THE CHALLENGE

D100 assumes the Maëli/Gabin web agency is a "clean" internal rehearsal target. It is not. The decision has two fatal flaws and one structural contradiction that U95 correctly identifies but D100 ignores.

---

## ANGLE 1: COMPETITION CONFLICT — WHO OWNS THE PLAYBOOK?

### The Problem D100 Ignores

D100 treats the web agency as an internal proxy for Alizé's delivery rehearsal. But D77 and earlier decisions explicitly include **"digital agencies/e-commerce"** in Alizé's capability frame. If the web agency is a digital agency — which it is — then:

- The web agency IS in Alizé's target ICP
- The web agency COULD become Alizé's competitor after Month 1
- Alizé would be teaching its first pilot client how to replicate Alizé's own business

### The Conflict Scenarios

**Scenario A — The web agency becomes a competitor:**
Maëli/Gabin use the Month 1 rehearsal to learn Alizé's playbook: deployment methodology, client qualification criteria, workflow design patterns, pricing structure. By Month 3, they can replicate the service for their own client base. Alizé has just trained its first competitor and given them a validated playbook at zero acquisition cost.

**Scenario B — The web agency is a real client:**
If the web agency is a genuine Alizé prospect, then Alizé is delivering a paid pilot to a company it could sell to. The conflict is different but equally problematic: Alizé's delivery team is simultaneously building the product FOR a target client while learning how to deliver it. The pilot is compromised because the delivery team's attention is split between execution and learning.

**Scenario C — Neither party acknowledges the conflict:**
The web agency doesn't realize it has been used as a rehearsal proxy. When Alizé approaches them as a real prospect in Month 3+, they wonder why they were the free test case and what that implies about Alizé's pricing and confidence.

### The Playbook Ownership Problem

D88 says: "Delivery playbook IS the technical moat — built during delivery, structured as reusable system."

If the delivery playbook is Alizé's primary moat (D88), then deploying it first at the web agency means:
- The web agency has first-mover access to Alizé's moat
- If the web agency competes, they deploy a playbook they helped validate
- If the web agency doesn't compete, they still have the playbook

There is no version of this where Alizé benefits more than the web agency does.

### U95 Is Unresolved — D100 Was Decided Anyway

The debate log explicitly flags **U95: "Web agency Alizé conflict — does web agency compete with Alizé target clients?"** as **unresolved**. Yet D100 was decided without resolving it. This is a planning error: the decision gates on the answer to U95, but the decision was made before the answer existed.

---

## ANGLE 2: SCALE MISMATCH — NOT A VALID PROXY FOR PME TARGET

### The 2-3 Person Web Agency vs. 50-200 Person PME

These are categorically different organizations. Deploying an AI agent at a 2-3 person web agency does not validate deployment at a 50-200 person PME. It validates deployment at a micro-business.

| Dimension | 2-3 Person Web Agency | 50-200 Person PME |
|-----------|----------------------|-------------------|
| **Decision-maker** | Founder/owner (1 person) | DG, COO, DGA, Head of Ops |
| **Approval process** | None — owner decides | Formal, multi-step, possibly board |
| **Integration complexity** | 2-4 tools, simple stack | 10-20 tools, legacy systems |
| **Workflow standardization** | Founder-adjacent, fluid | Formalized, documented, multi-person |
| **Agent permission scope** | Owner = all access | Role-based, GDPR-constrained |
| **Operational consequence of failure** | Owner notices | Team impacted, management alerted |
| **Budget authority** | Owner has full authority | Budget approval, finance gate |
| **Pilot negotiation** | Informal, one conversation | Commercial terms, legal review |
| **Ongoing governance** | Owner manages | Requires training, change management |

### What D100's Internal Pilot Will NOT Validate

The stated objective of D100 is delivery rehearsal for external client pilots. But a 2-3 person web agency cannot validate:

1. **Multi-stakeholder buy-in**: Does the DG sign off? Does the COO drive requirements? Does the Head of Ops own the workflow? None of this happens at a 2-person agency.

2. **Role-based access control**: Alizé's governance model requires configuring permissions by role. A web agency founder has one role: everything. This doesn't test the access control architecture.

3. **Change management**: At 50-200 employees, deploying an AI agent requires training non-founder employees, managing resistance, and establishing escalation paths. A 2-person agency doesn't have these dynamics.

4. **Formal commercial negotiation**: Contract terms, liability, data processing agreements, exit clauses — these are negotiated differently with a small agency than with a PME with a legal department.

5. **Integration depth**: A web agency's CRM is likely a spreadsheet or Notion. A 100-person professional services firm has a real CRM (Sage, Pennylane, HubSpot), email at scale, document management, and HR systems. The integration complexity is 5-10x different.

6. **ROI conversation**: A 2-person agency's "hours saved" ROI is trivial and explainable in one sentence. A 100-person company's ROI conversation requires quantified business metrics, departmental buy-in, and CFO-level validation.

### The Validation Criteria D100 Cannot Meet

An internal pilot should satisfy these criteria to justify Month 2 external pilot confidence:

- [ ] **ICP match**: Target has 20+ employees and operational complexity matching Alizé's ICP
- [ ] **Multi-person decision process**: Validation requires more than one person's sign-off
- [ ] **Role-based governance**: Agent permissions must map to actual organizational roles
- [ ] **Integration surface**: Minimum 3+ distinct business tools with realistic data
- [ ] **External reference value**: Outcome is credibly attributable to Alizé's method
- [ ] **No conflict of interest**: Target is not a competitor, partner, or potential acquirer

The web agency fails criteria 1, 2, 3, 4, and 6.

---

## PROPOSED ALTERNATIVE: WHAT D100 SHOULD HAVE BEEN

### Better Internal Pilot Options

**Option A: Grinto as Internal Pilot (if Grinto qualifies)**
- Grinto is Louis's company — truly internal
- If Grinto has 10-30 employees with real operational complexity, it satisfies ICP proximity
- No competition conflict because Grinto is not an Alizé prospect
- Limitation: Louis's conflicts of interest (founder of both) compromise objectivity

**Option B: A Warm Contact at a 20-50 Person Company (True External Pilot)**
- This is what D99/D100 tried to avoid — but D99/D100 created worse problems
- A warm contact at a 20-50 person company IS the right first external pilot target
- The distinction between "internal rehearsal" and "external client pilot" is artificial if the rehearsal has to be this compromised
- Better to run a discounted warm contact pilot with explicit "this is a rehearsal" framing than a free internal pilot with a competitor

**Option C: Louis Alone as the Internal Pilot**
- Louis manually runs the meeting report workflow for his own company (Grinto) for 4 weeks
- Documents the manual process, failure modes, client communication scripts
- No integration needed — just manual execution against the intended workflow
- Validates: whether the workflow is real, whether the ROI claim holds, whether the delivery script works
- Doesn't validate: technical deployment (that comes later with real clients)

### The Right Criteria for the Month 1 Internal Target

The internal pilot should validate **Louis's delivery process and documentation**, not Alizé's technical deployment. Therefore the internal target should:

1. **Generate real delivery material**: Meeting reports, client scripts, scope documents — output must be reusable
2. **Stress-test the workflow assumption**: Does meeting report generation actually save time? Prove it manually first
3. **Produce documentation**: Every step documented as if for a real client — this is the playbook
4. **Be independent of the web agency conflict**: No competition, no overlap, no playbook leakage risk
5. **Be completable in 4 weeks manually**: If Louis can't manually run this workflow for 4 weeks, the external pilot will fail too

The web agency satisfies #1 and #3 but fails #4 catastrophically and #2 partially.

---

## SUMMARY: WHY D100 IS FLAWED

### Fatal Flaws
1. **U95 unresolved**: D100 was decided before the competition conflict was assessed. The decision gates on U95 but didn't wait for it.
2. **Playbook leakage to potential competitor**: Deploying Alizé's moat (D88) at the web agency first gives that moat to an entity that may compete.
3. **Scale mismatch**: A 2-3 person web agency is not a proxy for 50-200 person PME deployment. It validates micro-business AI deployment, not PME deployment.
4. **Fails internal pilot validation criteria**: Cannot validate governance depth, integration complexity, multi-stakeholder buy-in, or commercial negotiation.

### Structural Contradiction
- D99 says "First deployment = delivery rehearsal, NOT a client pilot"
- D100 uses a web agency for that rehearsal
- But the web agency is potentially a client prospect (D95 flags this)
- And if it's NOT a client prospect (because it's a competitor), it has no value as a delivery rehearsal for Alizé's actual ICP

### The Self-Defeating Logic
- If the web agency is NOT in Alizé's ICP → it's not a valid rehearsal for Alizé's delivery
- If the web agency IS in Alizé's ICP → it competes with Alizé → playbook leakage risk
- D100 only works if the web agency is simultaneously in and out of Alizé's ICP, which is impossible

### Recommendation
- **Kill D100** — it should be reconsidered
- **Resolve U95 first** — before any internal pilot target is designated
- **Consider Option C** — Louis manually runs the workflow at Grinto for 4 weeks, documents everything, calls it the delivery rehearsal
- **Keep external pilot in Month 2** — but target a warm contact at a 20-50 person company with explicit "rehearsal + client" dual framing
