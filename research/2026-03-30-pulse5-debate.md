# Alizé Research Pulse 5 — 2026-03-30

**Pulse date:** March 30, 2026 — 11:14 UTC
**Participants:** Sales Strategist, Positioning Strategist, Operations Strategist
**Research base:** BRIEF.md + debate-log.md + all prior pulse debates

---

## Overview

Three specialist agents deployed to debate aspects of Alizé not yet stress-tested: (1) first-90-days sales motion realism, (2) durability of vertical depth as a moat, (3) whether 2 devs + 1 designer can actually deliver the service. All three completed fully and wrote research files.

---

## Debate S1: First 90 Days Sales Motion

**Agent:** Lead Sales Strategist  
**File:** `/data/workspace/alize/research/2026-03-30-pulse5-sales-motion-debate.md`

### Assumptions Challenged

**1. "Personal network → 2-3 pilots in 90 days"**

The math is tight. The actual funnel:
- ~50 warm contacts in ICP → 30% respond → 15 meetings
- 25% advance to proposal → 4 qualified opportunities
- 40-50% close rate → 2 pilots

French B2B sales cycle for new vendor: 9-13 weeks minimum. 90 days is barely enough for one full cycle. Failure modes include network exhaustion (contacts defer to "show me results"), sales cycle compression being a fantasy (DG enthusiasm → 3-4 weeks of internal approval), wrong network segment (micro-SMBs or large enterprises instead of ICP), free-work temptation for friends, and the time trap (Louis is 25% of the company; sales outreach is dev time lost).

**Verdict:** Plausible but fragile. 1-2 pilots signed in 90 days is realistic. 2-3 requires everything going right.

**2. "Expert-comptable partner channel is viable in Year 1"**

This has five missing prerequisites: track record with 3+ clients in the segment (none), understanding of their tech stack (none), reciprocity to offer (none), known brand in the accounting community (none), clear value proposition (none). Expert-comptables are conservative, already stretched thin (Jan-Apr tax season), invested in their own tool ecosystem, and skeptical of new vendors. They serve micro-enterprises and small PME (mostly <20 employees), while Alizé's ICP is 50-200 employees. Only 7-15% of their clients fit Alizé's addressable range.

Channel timeline is 12-18 months before producing a single paid client. What can go wrong: wrong expectations set internally, 20 cold emails → 2 responses both "not interested," they refer micro-SMBs instead of ICP companies, compliance liability fear prevents formal referrals.

**Verdict:** Not viable Year 1. Learning meetings only. True channel investment starts Month 9+.

**3. "Content/SEO will generate inbound leads in months 4-6"**

SEO timeline reality: new domain + initial content → Google still learning, zero ranking (Month 1-3). First keywords indexed → page 3-5 for long-tail (Month 3-4). Modest rankings (page 2-3) → 10-30 visits/month, 1-2 leads (Month 6-9). Meaningful traffic → Month 9-12. True organic SEO differentiation → 18-24 months for competitive French B2B terms.

LinkedIn organic is a myth for unknown brands: Louis's network of 500 connections sees 10-20% of posts. Without 1,000+ engaged followers, reach is 50-150 impressions per post. To generate 5 inbound leads from LinkedIn organic requires 50+ posts AND virality. Content team: 0 people, 0 hours available.

**Verdict:** Content/SEO is Month 9+ initiative. Year 1 pipeline engine = systematic outbound + paid search experiments + podcast/editorial.

### Recommended 90-Day Sales Motion

- Weeks 1-2: Build prospect list (50 companies, ICP-filtered), write outreach sequences
- Weeks 3-6: Warm outreach (30 contacts) + cold outbound (50/week via Lemly/Apollo)
- Month 2-3: Systematic outbound dominant + first Google Ads experiments (€300-500/month) + podcast outreach
- Month 3 end: Evaluate closed pilots and pipeline. Hard stop on channels that aren't working.

### 90-Day Test Gates

| Metric | Target | Acceptable | Concerning |
|--------|--------|------------|------------|
| Qualified meetings held | 15+ | 10-14 | <10 |
| Proposals sent | 5+ | 3-4 | <3 |
| Pilots signed | 2+ | 1 | 0 |
| Pipeline value (M4-6) | €15k+ | €8-14k | <€8k |
| Outbound reply rate | 3%+ | 2-3% | <2% |

---

## Debate S2: What Survives If Mistral Launches a Competitor in 18 Months

**Agent:** Positioning Strategist  
**File:** `/data/workspace/alize/research/2026-03-30-pulse5-positioning-debate.md`

### Assumptions Challenged

**1. "Vertical depth = durable moat"**

Vertical depth is the RIGHT direction, but the reasoning is imprecise. The moat layers decompose differently:

| Moat Layer | Replicable with 3-5 experts in 6 months? |
|---|---|
| Domain workflow knowledge | YES — domain experts know the workflows |
| Pre-built agent configurations | PARTIAL — can be reverse-engineered |
| French SaaS integrations (Cegid, Pennylane) | NO — requires real API development |
| Accumulated client feedback loops | NO — requires real deployments and time |
| Workflow library compounding | NO — requires 12+ clients minimum |

The moat is NOT in domain knowledge (Mistral can hire French accountants). The moat IS in integration development, accumulated deployment experience, and workflow compounding. Vertical depth is durable only if: (1) the vertical's dominant SaaS tools remain fragmented, (2) workflows are complex enough that generic agents fail, (3) client relationships create genuine switching cost.

The Pennylane risk is real: if Pennylane (raised €50M+ in 2024, AI features on roadmap) launches its own AI agent layer handling 80% of standard accounting tasks natively, the integration moat collapses. "Vertical depth" framing should be replaced with "operational expertise + integration depth" — more precise, leads to better decisions.

**2. "French hosting + EU AI Act = trust signal"**

Neutralized by every major platform. Microsoft Copilot Studio offers EU data residency. Salesforce Agentforce runs on EU infrastructure. HubSpot Breeze AI features run on EU infrastructure. Google Gemini for Workspace available in EU regions. EU data residency is now a commodity requirement, not a differentiator. The trust signal has shifted from "where data lives" to "how the agent is governed": audit trails, permission structures explicable to a DG, human-in-the-loop configurations that are demonstrable, EU AI Act documentation a DSI could show a regulator.

**3. "Professional services first"**

The challenge: professional services (consulting firms) are actually a POOR first vertical. They're typically small (5-30 employees), have heterogeneous workflows that are hard to standardize, and don't have the operational repetitive-task density that makes AI ROI obvious. Better first verticals: accounting firms (outsourced accounting, DAF-as-a-service) or healthcare admin (clinics, medical practices). These have standardized repetitive workflows, genuine task density, and decision-makers who understand ROI calculations.

### What Actually Survives Mistral

The durable moat is operational expertise + integration depth, not vertical knowledge:
- French SaaS tool integrations (Cegid, Sage, Pennylane) require real API development
- Accumulated client feedback loops require real deployment history
- Workflow library compounding requires 12+ clients minimum

The honest framing: "Alizé's moat is operational expertise in deploying and managing AI agents for French businesses — specifically the integration development, accumulated deployment experience, and workflow compounding that takes years to build." Vertical depth is the strategy; operational expertise is the actual moat.

---

## Debate S3: Can 2 Developers + 1 Designer Actually Deliver This?

**Agent:** Operations Strategist  
**File:** `/data/workspace/alize/research/2026-03-30-pulse5-operations-debate.md`

### Assumptions Challenged

**1. "The technical stack is deliverable by 2 developers"**

The architecture is a distributed agent platform with 9 distinct complex layers: Nuxt 3 frontend, Hono + Mastra runtime, pgvector RAG pipeline, MCP integration, multi-tenancy, real-time SSE/Redis/BullMQ, self-improvement layers, monitoring, and infrastructure management.

What's missing from the org chart: DevOps/platform engineer, client success/account manager, sales (even part-time), technical support.

The infrastructure math: 5 clients × 2 agents × 2-4 integrations = 10-20 integrations to maintain. Each new client requires new MCP server configs, new RAG corpus, new permissions. Two developers cannot simultaneously build the product, deploy and configure per-client agents, manage integrations, respond to support, tune prompts, run monthly reviews, and do sales.

**The 15-hour/day problem:** Even working 10 hours/day each, the team has ~20 developer-days/month per person. At 5 clients, monthly service delivery (monitoring, support, prompt tuning, monthly reviews) consumes 30-50% of available time. That leaves 10-14 days/month for product development. At that rate, the frontend completeness described in the BRIEF (11 pages/sections) takes 6-9 months.

**2. "Monthly managed service at €1,200-3,000/month is sustainable"**

At 5 clients × €1,500/month average = €7,500 MRR. After infra (€200-300/month), that's €7,200-7,300 gross. The two developers are both part-time on Alizé (assuming they're also doing Grinto work and Kuroba). If Louis and Gabin each allocate 50% of time to Alizé, that's 1 FTE equivalent. At French developer rates of €45-65k/year full-stack, 1 FTE costs €3,750-5,400/month gross. The €7,500 MRR covers ~1.5 FTE at market rates — but the service also requires Maëli's time for design work.

The economics only work if: developers are not paid at market rates (bootstrapped/student rates), client count stays below 5-8 (above that, service delivery consumes all developer time), or the service is genuinely light-touch after initial deployment (which requires significant automation).

**At 10 clients at €1,500/month = €15,000 MRR.** Still insufficient for 2 FTE + Maëli + infra + tools + overhead = €10,000-13,000/month minimum. The economics only work at 15+ clients, which requires 12-18 months to reach.

**3. "Quality will be maintained with this team size"**

Managed service requires: monitoring agent performance daily, responding to failures within hours, prompt tuning weekly, workflow adjustments bi-weekly, monthly performance reviews, handling support requests within SLA. For 5 clients with 2 agents each = 10 agent instances.

The quality risk: when two clients have simultaneous issues (agent returning wrong data, integration breaking, user adoption failing), the team can only address one at a time. The second client waits. If this happens monthly, one client per quarter feels neglected. That client doesn't renew.

The design problem: Maëli is the only designer. She's also handling Kuroba. Alizé's GTM requires landing page updates, case study graphics, sales deck, LinkedIn visuals. All design requests queue behind whatever Kuroba crisis is active.

### The Core Delivery Risk

The brief's own "Near-term (Month 3-6)" section lists: /agents page, /monitor page, /integrations page, /workflows/builder — plus client deployments, training, monthly reviews. These cannot all be delivered simultaneously by 2 developers while maintaining service quality for existing clients.

**The realistic delivery path:**

| Phase | Focus | Clients |
|-------|-------|---------|
| Month 1-3 | Core engine + 1 pilot client | 1 client |
| Month 4-6 | Core stable + 2-3 more pilots | 3-4 clients |
| Month 7-12 | Product hardening + new features | 5-8 clients |
| Year 2 | Scale with first hire | 10-15 clients |

**The hidden bottleneck:** Client success is not a role. It's currently Louis doing discovery + deployment + training + reviews. At 5 clients, Louis is doing this full-time plus trying to code. He cannot do both at quality.

### Verdict: What Needs to Change

Three things need to shift before the delivery plan is credible:

1. **Scope the product to what 2 developers can actually build in 90 days.** The MVP is: working agent runtime + 1 MCP connector + basic monitoring dashboard + landing page. Everything else is post-pilot.

2. **Add client success as a visible role by Month 3.** This can be Louis transitioning from developer to client-facing, or a part-time contractor. But it cannot be "we'll handle it."

3. **Revise the financial model.** The current model assumes economics work at 5 clients. They don't. The viable model requires reaching 12-15 clients before Alizé pays for itself. Build the plan to that reality, not the optimistic 5-client scenario.

---

## Cross-Cutting Themes — Pulse 5

Three independent agents converged on these conclusions:

1. **The 90-day sales plan is optimistic but fixable.** The math works if Louis is systematic and disciplined. The expert-comptable channel and content/SEO are not Year 1 assets — they're Year 2 investments. Outbound + network is the real Year 1 engine.

2. **Vertical depth framing needs precision.** "Operational expertise + integration depth" is more accurate than "vertical knowledge." The Pennylane risk is real and underweighted in current planning.

3. **The team size is the most underweighted risk in the entire BRIEF.** Two developers cannot simultaneously build a distributed agent platform and deliver managed service to 5+ clients at quality. The product scope needs to contract significantly, or the service delivery model needs to change (automated monitoring, reduced SLA, or third-party support partnership).

---

## What Changed vs. Existing Research/TODO

### New items for TODO

| ID | Topic | Change | Priority |
|----|-------|--------|----------|
| N19 | Sales motion realism | Personal network math is tight; 1-2 pilots in 90 days realistic; systematic outbound is the real engine | HIGH |
| N20 | Expert-comptable channel | Not Year 1 viable; Month 9+ at earliest; treat as learning meetings only | HIGH |
| N21 | Content/SEO timeline | Month 9+ before meaningful inbound; Year 1 engine = outbound + paid search | HIGH |
| N22 | Vertical depth reframing | Replace "vertical knowledge moat" with "operational expertise + integration depth"; acknowledge Pennylane consolidation risk | MEDIUM |
| N23 | Team delivery capacity | 2 devs cannot build platform + deliver service simultaneously; scope MVP to 90-day deliverable | HIGH |
| N24 | Financial model revision | Economics don't work at 5 clients; viable at 12-15 clients; plan to that reality | HIGH |
| N25 | Client success role | Must be defined by Month 3; Louis transitions to client-facing or contractor hired | HIGH |

### Existing items reinforced (unchanged)

- MCP demotion from marketing (still valid)
- Workflow-tier pricing (still correct direction)
- Fixed €4,500 pilot price (still correct)
- Professional services vs. horizontal (still valid — but challenge: is consulting firms the right first vertical?)

### Items needing Louis decision (NEW)

| ID | Topic | Options |
|----|-------|---------|
| U12 | First vertical selection | Professional services (consulting firms) vs. accounting firms/healthcare admin — trade-offs on task density, ROI clarity, decision-maker sophistication |
| U13 | Service delivery model | Self-deliver (Louis does client success) vs. automated-light (heavy monitoring automation, reduced SLA) vs. partner model (third-party handles support) |
| U14 | Product scope for 90 days | What specifically gets built in the first 90 days? Need a hard list. |

---

## Files Generated This Pulse

- `/data/workspace/alize/research/2026-03-30-pulse5-sales-motion-debate.md` — Sales motion critique (~18KB)
- `/data/workspace/alize/research/2026-03-30-pulse5-positioning-debate.md` — Moat durability stress-test (~20KB)
- `/data/workspace/alize/research/2026-03-30-pulse5-operations-debate.md` — Team delivery capacity analysis (~19KB)

---

## Debate Log Entry

| Date | Debates | Key Outcomes |
|------|---------|-------------|
| 2026-03-30 AM | Pricing, Competitive, GTM (partial) | Diagnostic €490 questioned; per-agent pricing challenged; platform giants as primary threats; workflow-tier proposed |
| 2026-03-30 | Technical, Vertical, Revenue | Architecture overengineered; horizontal positioning dangerous; MCP demoted; ETI tier aspirational; pilot price range too wide |
| 2026-03-30 midday | Landing Page, Network GTM, Differentiation | Landing page converts poorly; GTM structural ceiling at 5 clients; managed service + French hosting = thin moat; vertical depth is only durable differentiator |
| 2026-03-30 late | Managed service, Tech stack, Mistral threat | "Managed service" = liability headline; MCP = infrastructure; Mistral is 2027-2028, not 2026 |
| 2026-03-30 11:14 | Sales motion, Positioning moat, Operations capacity | Network math tight (1-2 pilots realistic); vertical depth reframed as "operational expertise"; 2 devs + service delivery is the #1 execution risk |

*Pulse 5 completed: 2026-03-30 11:17 UTC*
*Next pulse: 2026-03-31 morning or triggered by Louis decisions*
