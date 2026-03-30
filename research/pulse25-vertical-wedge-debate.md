# Pulse 25: The Regulated Vertical Wedge — Against the Conventional Wisdom

**Author:** Alizé Vertical Wedge Agent  
**Date:** 2026-03-30  
**Purpose:** Challenge the conventional wisdom that Alizé should avoid regulated industries at launch. Argue the opposite.

---

## The Conventional Wisdom (As Stated)

In the Pulse debates, the prevailing view is that Alizé should **start simple**: pick a low-regulation vertical, prove the model, and avoid the compliance burden of regulated industries (legal, healthcare, finance, expert-comptables) until the company is stable enough to carry the liability.

**The argument against:**
1. Regulated industries add legal complexity — EU AI Act compliance, GDPR, sector-specific rules
2. The EU AI Act enforcement timeline is uncertain (possibly 2027-2028 per D12/D42), making investment in compliance-premised positioning risky
3. Regulated firms already have established software vendors (Pennylane for accounting, specific legal practice management tools) who have embedded AI strategies
4. The liability of being the AI system of record for a regulated firm's sensitive data on day one is too much for a brand-new company

**This paper argues: the conventional wisdom is backwards. Alizé should target a regulated vertical as its first beachhead.**

---

## The Case FOR Regulated Verticals at Launch

### 1. The Governance Gap Is Real — and It's Disqualifying for the Incumbents

Microsoft Copilot has documented compliance gaps in GDPR-sensitive and AI Act-sensitive contexts. These are not edge cases. For a French expert-comptable managing client financial data, or a cabinet médical handling patient records, the question "where does our data go when Copilot processes it?" has a legally complex answer that most DSI teams cannot resolve to their satisfaction.

The key insight from D97: **in regulated contexts, the governance gap is not an inconvenience — it's disqualifying.** A regulated SME cannot simply sign a DPA with Microsoft and move on. They need audit trails, data residency guarantees, documented human-in-the-loop controls, and evidence of AI Act conformity that a DSI could show to a regulator.

Alizé can provide this. Microsoft cannot provide it at the depth a regulated SME needs without expensive enterprise contracts and extensive configuration work. This is Alizé's wedge.

**The moat is temporal and structural, not just technical.** It will take Microsoft 18-24 months to close the governance gap for regulated SME use cases at the price point Alizé will occupy. That's a window. Alizé should step through it now, not wait for it to close.

### 2. The Moat Is Real — Generic Agents Can't Operate Here

Agentova, ChatGPT, and other horizontal AI tools have a fundamental problem in regulated verticals: **they are not built to be the system of record for regulated data.** They are inference engines. They process queries and return outputs. They do not maintain persistent audit logs, enforce permission boundaries, or provide evidence packages for regulators.

This creates a natural protection from bottom-up AI adoption that Alizé does not need to manufacture. When a cabinet d'expertise comptable discovers that their team is already using ChatGPT on client data (which they are, inevitably), the correct Alizé response is not "stop doing that." The correct response is: "ChatGPT is not designed for what you're doing. Here's what a compliant agent system looks like, and here's why it matters for your insurance and your clients."

Alizé doesn't need to outcompete free tools on price. It needs to operate in the territory where free tools are structurally inappropriate.

### 3. The Pricing Ceiling Is Higher — and the Buyer Is Pre-Qualified

Regulated firms pay premiums for compliance-assured tools. An expert-comptable firm paying €15,000/year for Cegid and Pennylane is not price-sensitive to an additional €1,500-2,500/month for a compliant AI agent. They are price-sensitive to risk: the risk of an AI system that exposes client financial data, or that cannot be explained to the Ordre des Experts-Comptables.

The buyer in a regulated firm is also pre-qualified by their own compliance requirements. They have already done the work of understanding that AI use has regulatory implications. They are not asking "should we use AI?" — they are asking "how do we use AI in a way that doesn't create liability?" This is a dramatically shorter sales cycle than educating an unregulated SME about why AI governance matters.

**The math changes when you price for the compliance premium, not the productivity savings.** A case study that reads "we helped a cabinet médical automate their patient report generation while maintaining AI Act conformity" is not a €200/month story. It is a €7,500-setup + €2,000/month story. The regulated nature of the work justifies the price point without extensive ROI modeling.

### 4. The Case Study Is the Business — Make It Unassailable

Alizé's growth model depends on case studies. The GTM strategy is referral-driven: prove it with one client, get introduced to three more. This only works if the case study is **unassailable**.

A case study from an unregulated vertical ("we helped a consulting firm automate their meeting reports") is weak. It can be replicated by anyone with Copilot. It doesn't generate referral enthusiasm because the buyer doesn't understand what was hard about it.

A case study from a regulated vertical ("we helped an expert-comptable firm automate their liasse fiscale review process with full audit trail and AI Act documentation") is unassailable. It demonstrates depth. It shows Alizé understands the domain. It validates that Alizé can operate where the compliance stakes are real.

**The first case study defines Alizé's market position for the next 18 months.** Start with the hardest one. Everything else becomes easier after.

---

## Responding to the Challenges

### Challenge 1: Why would a brand-new Alizé want the liability of regulated-industry compliance on their first pilot?

**Answer: Because the liability is the product, not a burden on the product.**

If Alizé is not ready to carry regulated-industry compliance, it is not ready to compete in the territory where compliance is the differentiator. The alternative — starting with an unregulated vertical and trying to move up-market later — means competing on productivity gains alone, which is a race to the bottom against free tools.

The liability concern is also overstated. Alizé does not need to be a fully-compliant AI system under the AI Act on day one. It needs to be **more compliant than the alternatives** (ChatGPT, Copilot defaults) for specific regulated workflows. This is achievable with:
- OVHcloud-hosted infrastructure (already decided)
- Explicit data processing agreements that stay within EU
- Human-in-the-loop validation gates on sensitive outputs
- Audit logs that are exportable on request

None of these require Alizé to be a certified AI system under the AI Act. They require Alizé to be visibly more governed than the free option. That's a low bar, and it's the correct bar for a first pilot.

### Challenge 2: What if the EU AI Act enforcement timeline is 2027-2028, not 2026?

**Answer: The uncertainty is an argument FOR moving faster, not for waiting.**

If enforcement is 2027-2028, then the market awareness of AI governance risk will grow throughout 2026. Every month that passes, more regulated SMEs will have read about the AI Act in their industry publications (the Ordre des Experts-Comptables, the Ordre des Avocats) and will be asking questions they don't have answers to.

Alizé wants to be in those conversations now, before the market crystallizes around established vendors. If Alizé waits until 2028 to enter regulated verticals, it will be competing against the compliance tools those vendors have built in response to AI Act enforcement. The window closes.

The uncertainty in the enforcement timeline also means the governance gap is at its widest right now — Microsoft and HubSpot haven't closed it yet, and they won't close it in 2026. The window is open. Walk through it.

### Challenge 3: Expert-comptables already have Pennylane — why would they buy Alizé?

**Answer: Pennylane is accounting software. Alizé is an AI agent. They do not solve the same problem.**

Pennylane automates accounting workflows within its platform. It does not:
- Connect to a cabinet médical's patient management system and generate compliant reports
- Interface with a property manager's lease database and automate the quittancement process
- Pull from a legal practice's document management system and generate reviewed briefs

Alizé's target is not "do the accounting better than Pennylane." Alizé's target is "automate the administrative workflows adjacent to regulated work that Pennylane doesn't touch."

The expert-comptable market is ~20,000 firms in France. They serve ~3 million clients. They are drowning in administrative work that has nothing to do with accounting: client communication, document collection, reporting to regulators, internal process management. Pennylane handles the accounting. Alizé handles the administrative layer around it.

This is the same reason a legal practice has Clio (practice management) and still needs other tools. Software categories don't subsume each other just because they both use the word "AI" in their marketing.

---

## The Case for a Specific Regulated Vertical

Not all regulated verticals are equal. The right first vertical for Alizé should have:

1. **High administrative task density** — repetitive, standardizable workflows that don't require deep sector expertise to automate
2. **Visible compliance stakes** — the buyer already worries about AI governance and can articulate why it matters
3. **Fragmented tool landscape** — no dominant incumbent has solved the AI agent problem for this specific workflow
4. **Referral network density** — the firms talk to each other and share vendor recommendations

**Recommended first vertical: Expert-comptables (accounting firms), specifically targeting the administrative workflows around client management and regulatory reporting — not the core accounting work.**

This is not the same as "automate accounting." It's "be the AI employee who handles the administrative overhead so the expert-comptable can focus on advisory work." The AI Act compliance story is immediately legible to this buyer. The pricing premium is justified by the compliance assurance. The referral network (through the ordres and industry associations) is dense.

**Alternative first vertical: Property managers (gestionnaires de biens immobiliers).** High administrative task density, GDPR-sensitive tenant data, fragmented tool landscape (no dominant AI agent solution), and a compliance story (tenant data handling) that is immediately understandable.

**Avoid at launch:** Healthcare admin (patient data stakes are maximum, sector-specific regulatory complexity is highest) and legal (the legal market in France has strong existing software vendors with embedded AI strategies and the buyer profile skews toward DSI veto holders).

---

## The Counter-Risk: What the Conventional Wisdom Gets Right

The conventional wisdom is not entirely wrong. There are real risks to starting in a regulated vertical:

1. **Sales cycle length**: Regulated firms have longer decision cycles because they involve DSI and compliance review. This could blow up the 3-month pilot timeline.

2. **Delivery complexity**: Regulated workflows require domain knowledge that Louis and the Alizé team may not have on day one. "We learned delivery by doing delivery" is riskier in a regulated context.

3. **Liability exposure**: If Alizé's agent malfunctions in a way that exposes regulated data or generates non-compliant outputs, the liability is real and potentially existential for a brand-new company.

**These are real risks. They argue for careful pilot selection, not for avoiding the vertical entirely.**

---

## Recommendation

**Alizé should target a regulated vertical — specifically expert-comptables — as its first beachhead, not despite the compliance burden but because of it.**

The governance gap is real. The moat it creates is durable. The pricing premium is justified. The case study is unassailable. And the window to operate in this space before the incumbents close the gap is 18-24 months, not indefinite.

**The conventional wisdom — "start simple, avoid regulated" — is the path of least resistance that leads to competing on productivity gains alone, which is a race to the bottom against free tools.**

**The differentiated path — "start with the hardest case study that demonstrates genuine compliance capability" — is harder to execute but creates a market position that is structurally defensible.**

The one modification to the conventional wisdom that makes sense: **start with ONE regulated vertical, not three.** The expert-comptables are the right first. They have the highest task density, the clearest referral network, and the most legible compliance story. Prove it there. Use that case study to expand to property managers and legal practices in Year 2.

---

## Decision Points for Louis

- **U121 confirmed YES**: The governance-disqualifying-for-Copilot wedge is the correct first vertical argument. D81's meeting report workflow (low compliance stakes, weak case study value) should be replaced with an expert-comptable administrative workflow as the first use case.
- **The vertical is**: Expert-comptables — specifically the administrative layer around client management, not the core accounting work
- **The timing is**: Now. The window is 18-24 months before incumbents close the governance gap.
- **The risk mitigation**: One pilot, carefully selected, with a client who understands what they are buying (compliance capability, not just productivity). Do not spread across multiple sectors in Year 1.
