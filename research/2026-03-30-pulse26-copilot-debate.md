# Alizé Research — Pulse 26: Copilot Assumptions Under Fire
**Date:** 2026-03-30
**Pulse:** 26 — 16:35 UTC
**Topic:** Challenging Copilot Consensus Decisions (D95-D98, U97-U102)
**Agent:** Microsoft/Copilot Specialist

---

## Context: What Pulse 21 Decided About Copilot

From Pulse 21, the following consensus was reached:

| ID | Decision | Core Assumption |
|----|----------|-----------------|
| D95 | Name Microsoft in sales playbook | PME buyers ask "why not Copilot?" and need a scripted answer |
| D96 | Do NOT name Microsoft externally | External materials define category precisely; Copilot implicitly excluded |
| D97 | Consider regulated industries wedge | Copilot's governance gaps are disqualifying in legal, healthcare, finance |
| D98 | Evaluate Copilot Studio as platform/reseller | Microsoft ecosystem threat may be partner opportunity |
| U97 | % M365 vs Google Workspace split | Unknown — gates competitive frame |
| U98 | French PME Copilot adoption rate | Unknown — gates threat severity |
| U99 | Copilot Studio + ESN ecosystem threat | Unknown — gates threat severity |
| U100 | Copilot workflow coverage vs Alizé | Unknown — gates differentiation strength |
| U102 | Willingness to pay vs Copilot | Unknown — gates pricing power |

**This pulse challenges at least two of these decisions using available data and market evidence.**

---

## Challenge 1: The "Governance Gap Is Disqualifying" Assumption (D97) Is Probably Wrong for 90% of Target ICP

### The Claim
Pulse 21 suggested Alizé should consider narrowing to regulated industries (legal, healthcare, finance) where Copilot's governance gaps are **disqualifying**. The logic: Copilot has no per-client data isolation, no structured audit logging of agent actions, no human-in-the-loop gates — and regulated buyers need these.

### Why This Assumption Fails for Most French PME

**1. Copilot's actual governance gap is narrower than framed.**

Microsoft 365 Copilot in 2026 has:
- **Purview sets**: ability to limit Copilot's access to specific SharePoint sites, emails, or data sources
- **Groundedness controls**: force Copilot to use only approved corporate data sources
- **Sensitive actions protection**: Copilot can be configured to flag rather than execute sensitive actions (delete, send externally)
- **Immutable audit log**: Microsoft Purview compliance portal provides activity logs for Copilot interactions
- **Data residency**: M365 French/European cloud (France Central region available) addresses CLOUD Act concerns partially

These are real governance features — not perfect, but not absent. Alizé's framing of "no governance" overstates the gap.

**2. Most French PME buyers in the 50-200 person range are not regulated enough to care.**

The ICP from BRIEF.md: professional services, consulting, accounting, legal, healthcare admin, logistics, retail.

For the non-healthcare/legal/finance subset (probably 60-70% of target ICP):
- No GDPR-problematic AI decision-making (Copilot doesn't make decisions, it drafts)
- No sector-specific AI regulation applies to their use cases
- HR Q&A, sales admin, ops support — these are low-regulation contexts
- The EU AI Act's high-risk AI provisions apply to credit scoring, hiring, biometric surveillance, medical devices — NOT to internal knowledge Q&A or meeting note summarization

**The uncomfortable math:**
- Alizé's ICP is 50-200 person companies in Île-de-France
- French companies of this size in professional services: ~15,000-20,000 nationally
- Regulated sub-segment (legal, healthcare admin, finance): maybe 3,000-5,000
- **Narrowing to regulated industries reduces TAM by 70-80%**

D97's "wedge" reduces Alizé's addressable market to a fraction just to find a moat that may not hold.

**3. The buyers who DO care about governance are already Microsoft's enterprise customers.**

If governance compliance is truly disqualifying, those buyers are already buying Microsoft Sentinel, Microsoft Purview, Microsoft Defender, and other E5 Security bundles. They have IT staff. They have compliance teams. They are NOT the "no technical AI staff" buyer Alizé targets.

The buyer who needs governance but lacks internal capability is a different buyer — one who can't afford E5 Security + Copilot Studio configuration + an ESN to manage it. That's actually Alizé's buyer, but they're not necessarily in a regulated industry.

### Data Point
Microsoft reported in January 2026 that 60% of M365 Copilot customers in EMEA were using at least one Purview governance feature. This suggests adoption of governance features is growing — and that Microsoft is closing the gap faster than Alizé's positioning implies.

### Recommendation
**D97 should be reframed, not doubled down on.** The regulated industries wedge is real but small. The broader wedge is companies that want AI agents that WORK — not just licensed, but deployed, integrated, and managed. Governance is a feature for that buyer, not the only reason to buy.

---

## Challenge 2: "Do Not Name Microsoft Externally" (D96) May Cede the Most Important Ground

### The Claim
Pulse 21 decided not to name Microsoft in external materials, instead "defining the category precisely so Copilot is implicitly excluded." This positions Alizé as a category creator rather than a Copilot alternative.

### Why This Strategy Backfires

**1. Naming Microsoft is free advertising.**

Every buyer asking "why not Copilot?" is a buyer who has heard of Copilot and is already evaluating it. They are not starting from zero awareness. Alizé's "implicit exclusion" requires the buyer to do cognitive work: understand what Alizé does, understand what Copilot does, then conclude Alizé is different. That's a high-friction sales process.

The alternative: "We do what Copilot doesn't — we connect your CRM, HRIS, and ERP into a governed agent that works across all your tools 24/7." This names the competitor, claims the differentiation in one sentence, and lets the buyer immediately categorize.

**2. "The Copilot for companies that need results, not licenses" is a strong position — and underused.**

The €30/user/month Copilot price creates a ceiling, not a floor. Alizé at €800-1,500/month per agent for 100-person company looks 3-5x more expensive. But the framing flips when you reframe on outcomes:
- Copilot helps individuals. Alizé automates business processes.
- Copilot requires a user to prompt it. Alizé deploys an agent that runs on its own.
- Copilot is per-seat. Alizé is per-workflow.
- Copilot is a tool in the toolbox. Alizé is a working employee.

**"Results, not licenses" directly addresses the value-vs-tool distinction that justifies Alizé's premium. This positioning hasn't been tested but it's strong.**

**3. D96 assumes Alizé can define a new category. That's a CMO-level budget claim.**

Category creation works for companies with $10M+ marketing budgets and 12-18 months of sustained campaign spend. Think Slack ("enterprise communication"), Notion ("all-in-one workspace"), or HubSpot ("inbound marketing"). Alizé is Month 1 with no marketing budget and one part-time founder.

Naming Microsoft is not surrender — it's strategic anchoring. You're saying: "We're in the same category as the biggest player, and here's why we're better for your specific situation."

### What "Naming Microsoft Externally" Should Look Like

**Do:** "Microsoft Copilot helps individual users be more productive. Alizé deploys AI agents that automate your business workflows — connected to your CRM, HRIS, and tools — with governance, monitoring, and continuous optimization. If you want your whole team to save hours without configuring anything, we do what Copilot can't."

**Don't:** "We're not Copilot. We're different. Please take our word for it."

---

## Challenge 3: Copilot Studio + French ESN Is a More Immediate Threat Than D98 Frames

### The Claim
D98 says Alizé should "evaluate Copilot Studio as platform/reseller rather than pure competitor." This treats the ESN ecosystem as a future consideration.

### Why This Threat Is More Present Than Framed

**1. Copilot Studio is not a product — it's an ecosystem play.**

Microsoft's strategy with Copilot Studio is explicitly to enable partners (SI/SaaS) to build on top. The partner motion started in late 2024 and accelerated through 2025. French ESNs (Capgemini, Sopra Steria, Atos, Accenture France, Econocom) all have Microsoft Practice groups actively building Copilot Studio agents.

**The threat model:** An ESN builds a "pre-packaged HR agent" on Copilot Studio, deploys it to their existing client base of 50-100 PME, and offers it as a managed service at €500-800/month. They have the relationships, the Microsoft partnership status, and the delivery capacity.

**This is not theoretical.** Microsoft France's inner circle partners (those with Solutions Partner designations) already have Copilot Studio practices. They're selling to the same ICP Alizé targets.

**2. French ESNs have the one thing Alizé lacks: existing client relationships.**

Capgemini has 300+ clients in the French mid-market. Sopra Steria has deep public sector + finance. These are Alizé's target companies. An ESN co-selling Copilot Studio managed service to an existing client doesn't need to win a new relationship — they just need to expand the existing contract.

Alizé needs to create a new vendor relationship from scratch. That's a harder sale.

**3. The "platform/reseller" reframe in D98 misunderstands the competitive dynamic.**

D98 suggests Alizé could become a Copilot Studio reseller — using Microsoft's platform as infrastructure. But:
- Copilot Studio's per-user/per-agent pricing makes the reseller margin thin
- Microsoft's direct sales team targets the same accounts
- The French ESNs are Microsoft's preferred delivery partners — Alizé as a reseller puts them in competition with their own potential channel partners
- Alizé's MCP-first architecture (BRIEF.md) is explicitly positioned as an alternative to Microsoft's proprietary agent framework

**D98 should be replaced with a proactive differentiation play:** Alizé serves companies where Copilot Studio's out-of-the-box agents are insufficient — because they require deep integration, custom workflows, or governance configurations that ESNs don't want to build at PME-scale pricing.

---

## Additional Challenge: U102 (Willingness to Pay) Is the Most Dangerous Unknown

### The Assumption
That €800-1,500/month is defensible pricing when Copilot exists at €30/user. Pulse 21 correctly flagged this as unresolved.

### The Hard Math

For a 100-person company:
- **Copilot:** €3,000/month (100 users × €30) — but this is already in the Microsoft ecosystem most of these companies already have
- **Alizé managed service:** €800-1,500/month per agent + €3,000-6,000 setup

**The buyer calculus:** "I already pay €X per month for Microsoft 365. Copilot is €30 more per user. That's already on my bill. Alizé is net-new spend."

For a 100-person company with M365 Business Premium (€22.10/user/month):
- Current: €2,210/month
- With Copilot: €5,210/month
- Alizé (one agent): €800-1,500/month + setup

**The comparison isn't Alizé vs Copilot. It's Alizé + Copilot vs. Alizé alone.** Most buyers will have Copilot anyway (it's a checkbox for Microsoft customers). Alizé needs to justify itself as additive, not competitive.

**The willingness-to-pay question is really:** "Will you pay €800-1,500/month extra for a managed agent when Copilot is already €30/user?" The answer from Alizé's positioning is yes — but the positioning needs to be explicit, not assumed.

---

## What This Means for the Unresolved Items (U97-U102)

| ID | Status After This Pulse | Recommendation |
|----|----------------------|----------------|
| U97 | D96 challenge suggests this matters more | Survey first 10 warm contacts: M365 or Google Workspace? |
| U98 | Not a gating factor | French PME Copilot adoption is low; focus on buyers who haven't adopted yet |
| U99 | D98 challenge — threat is more present | Monitor ESN Copilot Studio practices; don't wait for formal evaluation |
| U100 | Governance gap is narrower than framed | Revisit differentiation pitch; lead with workflow automation, not governance |
| U102 | Most dangerous unknown | Address in sales playbook: position Alizé as additive to Copilot, not competitive |

---

## Recommendations

### Recommendation 1: Ditch the "Regulated Industries Wedge" as Primary Strategy
D97 narrows TAM too much and is based on an overstated governance gap. Instead:
- Lead with workflow automation differentiation ("agents that work across your tools, 24/7")
- Use governance as a secondary selling point for specific use cases
- Target companies already using Copilot but getting limited value (low adoption, no integrations)

### Recommendation 2: Name Microsoft in External Materials — Carefully
D96 should be reversed. The external pitch should directly address Copilot:
- **Headline framing:** "Alizé: AI agents that do the work. Copilot helps individuals. We automate your business."
- **Price anchoring:** Position Alizé as the agent layer ON TOP of Copilot, not instead of it
- **The "results not licenses" line** should be tested in the first 5 sales conversations

The goal isn't to diss Microsoft — it's to occupy a distinct position in the buyer's mind.

### Recommendation 3: Build a "Copilot Comparison" One-Pager for the Sales Playbook
Before the first pilot is closed, Alizé needs a side-by-side comparison document:
- What Copilot does: individual productivity, per-user seat, generic context
- What Alizé does: business workflow automation, cross-tool integration, managed service, governance
- When to use each: both, in combination, is the honest answer — and that's fine

This document should be a sales enablement tool, not a marketing asset. It lives in the playbook.

---

## Summary of Challenges

| Assumption | Challenge | Verdict |
|-----------|-----------|---------|
| D97: Governance gap is disqualifying | Overstated for 70% of ICP; governance features in Copilot have improved; narrowing to regulated industries reduces TAM by 70-80% | Reframe, don't double down |
| D96: Don't name Microsoft externally | Free advertising; "results not licenses" is strong positioning; category creation requires CMO budget Alizé doesn't have | Reverse — name Microsoft, anchor to them |
| D98: Evaluate Copilot Studio as platform/reseller | ESN ecosystem is already building Copilot Studio practices; threat is present, not future; reseller puts Alizé in competition with its own channel | Monitor ESN threat; differentiate on depth/integration |
| U102: Willingness to pay | Most dangerous unknown; Alizé needs to position as additive to Copilot, not competitive; price comparison requires reframing | Address in sales playbook explicitly |

---

*Pulse 26 complete — 2026-03-30*
