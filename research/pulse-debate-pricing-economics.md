# Pilot Fee Economics Debate: The €4,500 Trap

## Challenged Assumption

**D24 assumes** the €4,500 pilot fee ("at-cost + case study rights") properly qualifies leads, funds realistic delivery, and creates a conversion pathway to the 12-month managed service tier. **This assumption has not been pressure-tested against delivery reality.**

The fee was designed around a round number that "feels right" — not around the actual cost of delivering a 15-day implementation or the conversion economics of an unknown vendor selling 12-month commitments to French PME DGs.

---

## 1. Unit Economics at 15 Delivery Days — €300/Day Is Not Realistic

**The math doesn't close.**

If the pilot requires 15 days of delivery work (a conservative estimate for a real workflow integration — discovery, design, build, test, train):

| Cost Item | Calculation | Cost |
|-----------|-------------|------|
| Delivery (15 days × €300/day) | 15 × 300 | €4,500 |
| Louis oversight/supervision | ~3 days × €400 | €1,200 |
| Infrastructure (Month 1) | ~€50-150 | €100 |
| Case study documentation | 0.5 day | €150 |
| **Total cost** | | **€5,950** |
| **Pilot fee** | | **€4,500** |
| **Loss per pilot (pre-conversion)** | | **-€1,450** |

**€300/day for a qualified implementation specialist who can:**
- Scoped a real business workflow
- Connect 2-4 tools via MCP or API
- Build a RAG pipeline
- Configure guardrails
- Train a non-technical team
- Write exit criteria documentation

...is a below-market day rate for anyone with those skills in the French market. The going rate for a qualified freelance AI integration specialist is €400-600/day. At €450/day × 15 days = €6,750 in delivery costs alone — nearly 50% above the pilot fee.

**The only way €300/day works is if the work is done by someone junior, rushed, or not fully qualified.** Which means: more revision cycles, more Louis oversight, lower quality, higher risk of the pilot failing to demonstrate value, and lower conversion to retainer.

**Conclusion: The €4,500 pilot fee as structured is NOT at-cost. It's subsidized. The subsidy comes from Louis's time, which has no hourly billing against it.**

---

## 2. 50% Conversion Rate — Does the Economics Still Work?

**Assume 2 pilots per month, 50% convert to €1,200/month managed service.**

Revenue over 6-month window:

| Scenario | Pilots Run | Converts | Pilot Revenue | Recurring Revenue (6mo) | Total | Delivery Cost | Net |
|----------|-----------|----------|--------------|--------------------------|-------|---------------|-----|
| 50% conversion | 12 | 6 | €54,000 | €43,200 (6 × 1,200 × 6) | €97,200 | €71,400 | **€25,800** |

Sounds OK. **But now model 50% conversion with realistic delivery costs (€450/day × 15 days):**

| Item | Calculation | Cost |
|------|-------------|------|
| 12 pilots × 15 days × €450 | 12 × 15 × 450 | €81,000 |
| Pilot revenue | 12 × €4,500 | €54,000 |
| **Subsidy per pilot** | | **-€2,250** |
| **Total subsidy across 12 pilots** | | **-€27,000** |

The recurring revenue (€43,200 over 6 months) is the only thing that closes the gap. **But it takes until Month 7-8 to break even on the acquisition cost of a non-converting pilot.** With a 12-month commitment requirement (D54/D58), the client has to stay for the full year — which is a big ask from a DG who just met Alizé.

**The real risk:** If Month 3 exit clause is exercised (client unhappy, performance bond triggered), Alizé has collected:
- €4,500 pilot (all)
- €1,200 × 3 months = €3,600 managed service
- **Total: €8,100**

But delivery costs for the full pilot + 3 months of managed service support could be €10,000-12,000. **Loss of €1,900-4,000 per early-exit client.**

**The 12-month commitment (D54) is supposed to fix this — but see point 4.**

---

## 3. Does €4,500 Attract Serious Buyers or Price-Sensitive Ones?

**The selection bias problem.**

A €4,500 decision for a 100-person company is not trivial — it's a meaningful internal budget approval. But it's positioned at exactly the price point that:
- Is too expensive for micro-businesses who would self-serve with Agentova (€49-99/month)
- Is too cheap to signal serious enterprise intent to a DG at a 200-person company
- Creates an expectation of a "small, contained" project, not a strategic engagement

**The €4,500 pilot attracts buyers who:**
- Have budget authority for €5K decisions but not €20K decisions
- Are curious enough to experiment but not committed enough to pay €7,500-10,000
- May be evaluating multiple vendors simultaneously (including Agentova, Magic AI, or internal build)
- Have lower switching costs — if they don't like the pilot, they walk away having only spent €4,500

**What it doesn't attract:**
- DGs who need strategic transformation, not a pilot
- Companies willing to commit to 12-month managed service (because they've already committed €4,500 and don't want another big commitment)
- High-conviction buyers who would pay €7,500-15,000 for a properly scoped pilot with exclusivity

**The irony:** The €4,500 pilot fee was designed to reduce risk for buyers. In practice, it may reduce Alizé's ability to attract high-conviction buyers who conflate "higher price" with "higher quality."

**Signal vs. filter:** The fee filters out buyers who can't afford €5K. It does NOT filter for buyers who are serious about AI transformation vs. those running a beauty contest.

---

## 4. Does a 12-Month Commitment Actually Close?

**Ask the DG question directly: "Sign a 12-month contract with an unknown vendor."**

The D54/D58 logic:
- Kill month-to-month → replace with 12-month commitment tier
- Month-to-month becomes premium (+20% flexible option)
- 12-month gets enhanced SLA + quarterly business reviews + priority use case development

**The problem:** A DG at a 100-person French company has seen dozens of AI vendors come and go. They know their IT projects run over budget. They know software implementations fail. And they know that signing a 12-month contract with a 2-person startup (Alizé, in Year 1) means:
- If Alizé fails, they lose 12 months of service + migration cost
- If the agent doesn't work, they've committed to paying €1,200-2,500/month for something broken
- Their procurement department may not even allow 12-month commitments to unknown vendors

**What a DG actually thinks:**
> "€1,200/month for 12 months is €14,400. Plus €4,500 pilot is €18,900. And I don't know these people. I don't know if they'll still be in business in 6 months. I don't know if their agents actually work. The pilot is supposed to prove it — but if I have to sign a 12-month contract AFTER the pilot to continue, I'm locked in regardless."

**The DG's real options:**
1. Negotiate the commitment down (3 months? 6 months?)
2. Demand a lower price for the 12-month commitment
3. Walk away and use Agentova at €99/month with no commitment
4. Build internally with ChatGPT API

**D54/D58 may actually reduce conversion rates** compared to a well-structured month-to-month with genuine performance evidence. The commitment requirement is Alizé protecting itself from early churn — but it signals distrust to the buyer.

**Alternative framing that might work:** 6-month initial commitment with 30-day written notice, framed as "we need 6 months to optimize the agent for your specific workflows, after which you can decide if the ROI justifies continuing." This is honest, achievable, and still gives Alizé meaningful revenue visibility.

---

## 5. Staged Payment (D52: 50% Kickoff / 50% Day 15) — Cash Flow Risk

**Yes, there's real risk here.**

Timeline:
- **Day 0:** Contract signed, €2,250 collected
- **Days 1-14:** Alizé delivers the majority of pilot work (discovery, design, build)
- **Day 15:** Alizé presents results and invoices second €2,250
- **Day 15+:** Client may dispute, delay, or refuse payment

**The problem:** By Day 15, Alizé has typically delivered 70-80% of the pilot's value. The second payment is due at the moment when:
- The client has seen the results (which they may be underwhelmed by)
- The client's team is tired from onboarding
- The DG is asking "so do we need to sign the managed service contract now?"
- The client realizes the second payment is the last cash outlay if they don't convert

**If the client disputes or delays:**
- Alizé has no leverage to collect the second €2,250 (work already delivered)
- Legal action is disproportionate to the amount
- The relationship is damaged at the worst possible moment (right before managed service conversion)
- Cash flow is hit: €2,250 received, €4,000+ spent = -€1,750 on that client

**The refund/performance bond structure in D52 helps** (next workflow free if metrics miss), but it doesn't solve the cash collection problem — it shifts the penalty to future work, not immediate cash.

**Better structure:** 70% at kickoff, 30% at delivery acceptance (Day 20-25), not Day 15. The milestone should be "client accepts deliverables" not "calendar reaches Day 15." This shifts risk back to the client and creates an incentive for them to engage actively with the delivery.

---

## Arguments For the Current Structure

**Defenders of the €4,500 pilot fee would say:**

1. **It's a loss leader.** The real money is in the 12-month managed service. The pilot acquires the client; the retainer amortizes the acquisition cost.

2. **It qualifies for commitment.** A DG who pays €4,500 is demonstrating real intent. The fee self-selects for serious buyers.

3. **The case study value is real.** First 3-5 case studies are worth more than €4,500 each in downstream sales leverage.

4. **Louis's time is the subsidy.** If Louis does the delivery himself at effectively €0 incremental cost (he's drawing a salary regardless), the €4,500 covers external costs (tools, infra, subcontractors) without needing margin.

5. **The 12-month commitment protects against churn.** If conversion is 70%+, the economics work. Month-to-month is a churn trap.

**These arguments are valid for the FIRST 3-5 pilots where Louis has motivation, network relationships, and case study urgency.** They are NOT valid for scaling to 12-15 clients with external delivery partners and cold outbound.

---

## Arguments Against the Current Structure

1. **€4,500 is below delivery cost at realistic day rates.** The model only works if Louis is the delivery mechanism, which contradicts D31 (Louis cannot solo-deliver).

2. **50% conversion is optimistic for a cold outbound channel.** Warm network might hit 70%+. Cold outbound through LinkedIn will be 30-40% at best in Year 1.

3. **The 12-month commitment ask is disproportionate to the trust established.** D54/D58 will lose deals that month-to-month would win.

4. **Stage payment at Day 15 creates a collection risk at the worst possible moment** — right before conversion decision.

5. **The €4,500 price point attracts buyers who are cost-conscious, not transformation-committed.** These buyers are the ones who churn when they see the managed service price.

6. **The subsidy structure doesn't scale.** If every pilot loses €1,000-2,000 before conversion, Alizé needs to fund 12+ pilots with cash runway before the recurring revenue covers the acquisition cost.

---

## Recommendation

### 1. Raise the pilot fee to €6,500-7,500 for standard scope, €10,000+ for complex multi-tool integrations

At €7,500 and €450/day = 16.7 days of delivery. That's realistic. The case study rights and conversion expectation justify a higher price from serious buyers. **If the prospect balks at €7,500, they're not the right client.**

### 2. Restructure payment to 70/30 — kickoff/acceptance

70% at contract signature (Day 0), 30% at client sign-off on delivery acceptance (Day 20-25). This protects Alizé's cash flow and creates a clear mutual obligation.

### 3. Replace 12-month commitment with 6-month initial + 30-day exit clause

The 6-month commitment gives Alizé enough revenue visibility to staff properly. The 30-day exit clause after Month 3 gives the DG an escape hatch without penalizing Alizé for poor performance. **Frame it as: "We need 6 months to fully optimize the agent for your specific workflows. After that, you can decide to continue month-to-month."**

### 4. Differentiate pilot price by channel

- **Warm network pilots (first 5):** €4,500 is fine — these are relationship-driven, case study investments
- **Cold outbound pilots:** €7,500 minimum — the conversion rate is lower, the client quality is lower, the subsidy is not justified

### 5. Track conversion rate by channel from Day 1

If warm network converts at 70%+ but cold outbound converts at 40%, the economics of the cold outbound pilot program need to be re-priced or restructured.

---

## New Decision (if any)

**D59 — Pilot fee restructure:** The €4,500 pilot fee should be re-priced to €7,500 for standard scope (single workflow, 2-4 tools, 10-15 delivery days). The 70/30 payment structure replaces D52's 50/50. The 12-month commitment is replaced with a 6-month initial commitment + 30-day exit clause after Month 3.

**D60 — Channel-differentiated pricing:** First 5 pilots (warm network) maintain €4,500 as case study investment pricing. Cold outbound pilots are €7,500 minimum.

---

## Unresolved Questions

1. **What is the actual delivery day count for a single-workflow pilot?** If it consistently runs 18-22 days, €4,500 is impossible to defend at any day rate.

2. **What is Louis's actual capacity?** If Louis is doing delivery + sales + management + marketing, there is no slack for 2 pilots/month. The €4,500 assumes delivery capacity that hasn't been validated.

3. **What is the actual conversion rate on cold outbound pilots vs. warm network pilots?** This is the most important number for the financial model, and it's unknown.

4. **Does the 6-month commitment close at the same rate as 12-month?** Alizé needs to test this in real sales conversations, not assume the DG will accept any commitment.

5. **Is there a payment enforcement mechanism for the Day 15 invoice?** If a client refuses to pay, what is Alizé's recourse on a €2,250 invoice? (The answer is: almost none, practically.)

6. **What happens to the D54/D58 decision if the DG pushes back on 12-month?** Does Alizé walk away from the deal, or does Louis negotiate? Who has authority to offer 6-month?

---

*This debate challenges the commercial model at its most critical conversion point. The pilot is the gateway to the entire recurring revenue stream. If the pilot economics are subsidized and the commitment requirement is a bridge too far, the funnel collapses before it starts.*
