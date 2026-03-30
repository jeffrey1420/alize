# Stack Appropriateness Debate — Alizé Pulse 49

**Date:** 2026-03-30  
**Debate Topic:** Is the current technical stack still appropriate given commercial reality?  
**Assumption Challenged:** D45/D50 — that the existing Mastra + pgvector + Redis + OVHcloud stack is "sufficient for Month 1 pilots" and that infrastructure decisions should be deferred to Month 3.

---

## Core Question

Alizé has no clients, no revenue, no deployed system, and a research process that produced 48 pulses in a single day without executing a single commercial action. The technical stack was designed as if product-market fit were guaranteed. It isn't. The question is not whether the architecture is technically sound — it is whether spending €80-120/month on a production-grade stack before validating commercial demand is the right priority.

---

## Assumption Challenged

**D45/D50: "Existing stack sufficient for Month 1 pilots. Defer all infrastructure decisions to Month 3."**

This assumption treats the stack as a solved problem. It isn't. The "existing stack" (Mastra + pgvector + Redis + S3 on OVHcloud) is not deployed. It has never served a client. The infrastructure budget was revised upward from €25-40/month to €80-120/month — a 3-4x increase — before a single euro of revenue exists. D50 says defer to Month 3. But Month 3 arrives with €240-360 in infrastructure costs and zero clients to show for it.

The assumption that the stack is "sufficient" also assumes the pilots happen. If the pilots don't happen — which 48 pulses of research with zero execution suggests is possible — the stack is just a cost center with no corresponding revenue.

---

## Current State Assessment

| Metric | Value |
|--------|-------|
| Paying clients | 0 |
| Revenue | €0 |
| Deployed system | None |
| Infrastructure spend to date | Unknown (stack not deployed) |
| Monthly infrastructure run rate (budgeted) | €80-120/month |
| Research pulses in one day | 48 |
| Commercial actions completed | 0 |

**The stack was designed for a business that doesn't exist yet.** The architecture documents are thorough. The MCP integration patterns are well-specified. The multi-tenant isolation design is correct. None of it matters if there are no tenants.

The D45 decision — "skip n8n for Month 1, run on Mastra + direct API integrations" — acknowledged the infrastructure deferral but did not question whether the underlying stack investment was appropriate at this stage.

---

## Argument For Current Stack

**1. The architecture is well-designed.**

The 01-technical-architecture.md document is the best piece of research in this folder. PostgreSQL RLS, MCP integration, hierarchical agent patterns, CNIL compliance — these are the right choices for a French AI agent platform serving PME/ETI clients. If Alizé succeeds, this architecture will scale cleanly.

**2. MCP is the correct long-term integration layer.**

Once clients exist, MCP will enable rapid connection to Pennylane, Qonto, Cegid, and whatever French tools emerge. Building this capability in advance is defensible if the timeline to first client is short.

**3. Deferring infrastructure decisions to Month 3 (D50) is coherent.**

The argument: don't lock in infrastructure choices before understanding the actual workload. Pilot clients may have entirely different integration needs than the architecture anticipates. Waiting for evidence before committing is rational.

**4. pgvector + Redis + S3 is the minimum viable state layer.**

An agent platform without persistent state is not a platform. The vector store, session cache, and object storage are genuinely required for anything beyond a stateless demo. You cannot build a product on top of a demo.

---

## Argument Against Current Stack

**1. The stack is designed for a product company. Alizé is a services company that hasn't delivered a service yet.**

The architecture in 01-technical-architecture.md describes a multi-tenant SaaS platform with per-tenant schemas, RLS policies, MCP server registries, and CNIL-compliant data residency controls. This is the architecture of a product that has paying clients. Alizé has none. The architecture is not wrong — it is premature by approximately 6-12 months.

The correct architecture for Month 1 of a services business: a single Mastra instance, one client's workflow, direct API calls, and a spreadsheet tracking what's working.

**2. €80-120/month with zero revenue is a cash drain with no floor.**

The infrastructure budget was revised upward to support the full stack. With zero revenue, every euro of infrastructure spend is an euro of loss. At €120/month, that's €1,440/year before a single client pays. The real risk is not the monthly cost — it's that the infrastructure commitment creates a sunk cost dynamic where Louis feels compelled to "use what he built" rather than making the right commercial choices.

If Month 1 delivers one pilot client who needs a completely different integration pattern (e.g., they only need email + Google Sheets), the pgvector + Redis + S3 stack was unnecessary overhead.

**3. Mastra is the wrong runtime for consulting-mode delivery — D45 was right to work around it, but wrong to keep it as the foundation.**

The pulse 10 tooling debate correctly identified that n8n on OVHcloud was the right delivery layer for Month 1. D45 walked that back because "n8n MCP is not production-stable." The conclusion — run on Mastra + direct API calls — is a developer's solution to a delivery problem, not a client's solution.

Mastra is an agent framework. It is designed to orchestrate multi-step agentic workflows with memory, tools, and state. For a consulting-mode delivery where Louis is manually orchestrating a client's first workflow, Mastra adds ceremony without value. The agent runtime is the wrong abstraction for a human-driven first delivery.

What Month 1 actually needs: a shared Gmail/Notion/Pennylane account, a simple prompt running on Claude API, and Louis actively performing the workflow while documenting every step for the playbook.

**4. The 48-pulse research process has produced zero commercial action. The stack is a research artifact, not a business asset.**

This is the most uncomfortable point. The infrastructure was designed in a research document. It was never deployed. It was never tested against a real client's requirements. It exists as a collection of markdown files describing an architecture that has not been instantiated.

D50 says "defer to Month 3." This means Month 1 and Month 2 are spent with either (a) no infrastructure (pure manual delivery) or (b) building infrastructure that hasn't been validated by real client needs. Neither outcome is improved by committing to pgvector + Redis + S3 now.

**5. The no-code approach (Zapier/Make.com/n8n) for Months 1-3 is not a compromise — it is the correct commercial choice.**

Before Alizé has one paying client with measurable outcomes, the question is not "what infrastructure enables scale?" The question is "what delivers value to the first client fastest?"

Zapier handles 80% of PME automation use cases. Make.com connects to Pennylane and Qonto. n8n self-hosted on OVHcloud (even with MCP stability concerns) handles the workflow orchestration. None of these require pgvector. None require Redis. None require the full Mastra agent runtime.

The first client does not need a vector store. They need their invoice processing automated. Build that with Zapier. Prove it works. Then, and only then, decide whether the infrastructure needs to change.

---

## Recommendation

**Challenge D50. Kill the full stack investment until Month 2 at the earliest.**

The specific recommendation:

1. **Month 1: Zero infrastructure cost.** Louis delivers the first pilot manually using direct API calls (Claude API + client tool integrations). Track the workflow in a spreadsheet and a shared Notion doc. Cost: €0 in infrastructure beyond development tools already in use.

2. **Month 2: Validate before investing.** If the pilot produces measurable outcomes and the client wants to continue, assess what infrastructure would have actually been needed. Was pgvector used? Was Redis used? Did the Mastra runtime add value or overhead?

3. **Month 3 onward: Build for the validated use case.** At Month 3, with evidence from one or two real clients, the infrastructure decision becomes data-driven rather than assumption-driven. At that point, pgvector + Redis + Mastra may be the right choice — or the evidence may show a simpler stack is sufficient.

4. **Kill the €80-120/month infrastructure budget until revenue exists.** A budget implies an intention to spend. With zero clients, there is no justification for this spend level. Redirect the cost-plus margin analysis to the unit economics debate instead.

**The assumption to challenge:** D50's "existing stack sufficient for Month 1 pilots" is true only if the pilots use that stack. If the pilots succeed with manual delivery, the "existing stack" was never validated and the infrastructure decision remains unmade at Month 3 — meaning the deferral was a delay, not a strategy.

---

## What This Means for Technical Decisions

| Decision | Current State | Recommendation |
|----------|--------------|----------------|
| Mastra runtime for Month 1 | Build and deploy before first client | Do not deploy until Month 2 evidence warrants it |
| pgvector | Included in full stack design | Defer — not needed for manual-first delivery |
| Redis | Included in full stack design | Defer — session state not needed for single-client manual delivery |
| OVHcloud VPS | Budgeted at €80-120/month | Kill budget until revenue exists |
| n8n self-hosted | Deferred to Month 3 | Use n8n cloud (€20/month) or Zapier for Month 1-2 delivery |
| MCP integration layer | Core architectural component | Build as needed, client by client — not upfront |
| Landing page / demo | Still not live | This is more commercially urgent than any infrastructure decision |

**The one thing that should be built before any infrastructure decision:** A live landing page with a waiting list signup. The research process has spent 48 pulses debating what to build without ever putting a signup form in front of a potential client. The stack does not matter if the pipeline is empty.

The technical decisions should follow commercial validation, not precede it.

---

## Summary

The architecture is correct for a product company with clients. Alizé is a services company with zero clients. The stack was designed top-down from technical assumptions rather than bottom-up from validated client requirements. D50's deferral is not a strategy — it is a delay with a budget commitment attached.

The right stack for Month 1 is the one that costs nothing and delivers the first client outcome. That is not pgvector + Redis + Mastra + OVHcloud. That is a direct API call, a documented workflow, and Louis's time.

Build the business first. Build the platform second.
