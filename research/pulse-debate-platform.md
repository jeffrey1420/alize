# Pulse Debate: Does Alizé Need a Platform at All in Year 1?

**Date:** 2026-03-30  
**Debate:** Platform Build vs. Pure-Service Delivery  
**Side:** Challenge the "build platform in parallel" assumption

---

## Challenge Statement

The prevailing assumption is that Alizé must build its platform (agent runtime, MCP connectors, RAG pipeline) in parallel with manual pilot delivery — that it's the "foundation that enables scaling." This challenge contests whether that foundation is necessary, wise, or even correct for Year 1.

**Core question:** What if Alizé is actually a premium consulting firm that happens to use AI agents, rather than a platform company that delivers services?

---

## 1. Could Alizé Deliver Pilots as a Pure-Service Play for 6-12 Months?

**Yes — and it may be the smarter move.**

A pure-service model means Louis + contractor executing manually using existing tools (Make.com, Zapier, n8n, direct API calls). Here's what this looks like:

| Model | Month 1 | Months 2-6 | Months 6-12 |
|-------|---------|-----------|-------------|
| **Pure Service** | Louis delivers 1 pilot manually | Louis + contractor deliver 2-3 pilots | Expand to 4-5 active clients |
| **Platform First** | Platform build starts | Platform still in dev | Platform launches, still validating clients |

**What pure-service buys Alizé:**

- **Immediate revenue validation** — Actual POs, actual renewals, actual NPS
- **Zero dev cost** — No €60-120K engineering burn
- **Learn what clients actually want** — Not what Louis assumes they want
- **Cash flow positive faster** — Consulting margins can reach 60-70% at scale

**Realistic service economics (French market):**
- AI automation consulting day rate: €800-1,500/day
- A mid-size pilot engagement: 10-20 days = €8,000-30,000 per pilot
- Louis alone could bill €15-20K/month at 60% utilization
- Add contractor at €5-8K/month, Alizé margins on labor: 40-60%

**The playbook isn't product — it's process.** If the manual delivery is scriptable enough to hand to a contractor in Month 2, it's a *service operation*, not a platform. That's not a criticism — that's a viable business model.

---

## 2. What Does Building a Platform Buy Alizé vs. Premium Consulting?

This is the central strategic question. Let's be concrete about what a platform actually adds:

### Platform Benefits (The Case For)

| Benefit | Description | Realistic Timeline |
|---------|-------------|-------------------|
| **Scalability** | 10x clients without 10x headcount | 12-18 months to realize |
| **Margin expansion** | Marginal cost per client approaches zero | 18-24 months |
| **IP/Defensibility** | Can't easily replicate a custom agent runtime | If it works |
| **Pricing power** | Platform = "product" premium over "service" rates | If market accepts it |
| **Recurring revenue** | Platform subscriptions vs. project-based consulting | Year 2+ |

### Platform Costs (The Honest Accounting)

| Cost | Number |
|------|--------|
| **Backend/infra engineer** | €80-120K/year (France) or €300-500/day contractor |
| **Agent runtime dev (estimated)** | 3-6 months to MVP |
| **MCP connectors** | 2-4 weeks per connector type |
| **RAG pipeline** | 2-3 months for production-grade |
| **Total Year 1 investment** | €100-250K (conservative) |
| **Opportunity cost** | Engineering bandwidth diverted from client delivery |

### The Honest Question

**Does Alizé need a platform to reach €100K ARR?** No. A solo practitioner or small team with existing tools can reach that.

**Does Alizé need a platform to reach €1M ARR?** Almost certainly yes — but that's a Year 2+ problem, not a Year 1 problem.

If Alizé can't validate demand at the service level first, the platform becomes a very expensive way to learn that lesson.

---

## 3. What Are the Risks of Building Before Validating?

### Risk 1: Building in a Vacuum

Without paying clients, Alizé is guessing about:
- Which workflows actually save clients time/money (vs. which sound good in a pitch)
- What clients will actually renew for
- What integrations matter most
- What the "right" abstraction level is for the platform

**Outcome:** Platform features built on assumptions, not evidence. Classic premature optimization.

### Risk 2: Cash Burn Without Validation

| Scenario | Year 1 Burn | Year 1 Revenue | Status |
|----------|-------------|-----------------|--------|
| **Platform first** | €150-250K | €0-50K (late pilots) | Likely negative cash |
| **Service first** | €30-60K (Louis + contractor) | €100-200K | Cash positive by Month 4-6 |

The service-first model self-funds validation. The platform-first model requires external capital or Louis depletes savings.

### Risk 3: Sunk Cost Fallacy

If Alizé spends 6 months and €150K building a platform, then faces weak renewal rates, the decision becomes emotionally loaded:
- "We have to keep going — we already invested so much"
- Platform gets force-fitted to bad product-market fit
- Good money after bad

### Risk 4: Time-to-Market Advantage Erodes

AI agent tooling is evolving rapidly:
- **Mastra cloud**: Production-ready agent runtime with deployments
- **Yeai**: Managed multi-agent orchestration
- **crewAI Pro**: Cloud-hosted agent teams
- **n8n cloud + AI nodes**: Workflow automation with LLM integration

If Alizé builds a platform in Year 1, it competes with these tools in Year 2 — but without the user base, community, or maturity they have.

---

## 4. Could Alizé Use Existing No-Code/LLM Orchestration Tools Instead?

**Yes — and this is a strong option.**

| Tool | Best For | Cost | Dev Time to Production |
|------|----------|------|----------------------|
| **n8n (self-hosted or cloud)** | Workflow automation with AI nodes | Free self-hosted / €20-100/mo cloud | Days to weeks |
| **Make.com** | Visual workflow automation | €9-100+/mo | Days |
| **Zapier** | Simple trigger-action automations | €0-100+/mo | Hours to days |
| **crewAI Pro** | Multi-agent orchestration | Early access/pricing TBD | Days to weeks |
| **Mastra (cloud)** | Agent runtime with memory + tools | In beta, pricing TBD | Days |
| **AgentVerse** | Multi-agent simulation | Open source | Weeks |

### The Composition Over Build Argument

For Year 1, Alizé could compose:
- **n8n** for workflow orchestration
- **OpenAI/Anthropic API** for LLM inference
- **Pinecone/Weaviate** for vector storage (if RAG needed)
- **Existing MCP connectors** where available
- **Custom scripts** where no tool exists

**This is not glamorous. It works.** Many successful AI services companies (especially in Europe) operate exactly this way for years before (or instead of) building custom platforms.

### The Counter-Argument (Why Build)

- Custom platform enables proprietary IP
- Better margins at scale
- Customer data stays in-house
- Unique workflows that generic tools can't support

**Verdict:** These benefits are real but not Year 1 priorities. They're Year 2+ optimization decisions.

---

## What This Changes If the Assumption Is Wrong

**The assumption:** Platform is the foundation that enables scaling.

**If the assumption is WRONG** — meaning:
- Clients don't need/want a platform (they want results)
- The market is actually buying "AI consulting" not "AI platform"
- Clients prefer working with humans who use tools vs. self-serve platforms

**Then:**
- 6 months of platform dev = wasted
- €150-250K burn = existential risk for a bootstrap
- First-mover advantage in "platform" is actually disadvantage (built the wrong thing)
- Competitors who went service-first got real renewal data; Alizé has a half-built platform and no revenue

**If the assumption is RIGHT:**
- Service-only model hits a ceiling at ~€200-300K ARR (Louis + 2 contractors)
- Platform enables 10x scale but only matters in Year 2+
- Still valid to delay platform until after validation

**The asymmetry matters:** Being wrong about platform in Year 1 is catastrophic. Being wrong about platform in Year 2 is fine — you pivot to platform from a position of revenue and evidence.

---

## Decision Recommendation

### Reject the "Parallel Platform Build" Assumption for Year 1

**Recommended path:**

| Phase | Timeline | Focus | Expected Outcome |
|-------|----------|-------|------------------|
| **Phase 1: Service Validation** | Months 1-6 | Louis delivers 3-5 pilots manually with existing tools | €50-150K revenue, 3+ renewal signals |
| **Phase 2: Service Maturation** | Months 6-12 | Contractor(s) deliver against playbook; document what's replicable | Profitable service operation, €200-400K ARR |
| **Phase 3: Platform Decision** | Month 12+ | Based on real data: build, buy, or compose | Informed decision with evidence |

**What to do instead of building platform:**
1. Use **n8n cloud** or **Make.com** for client automations
2. Script repeatable workflows into **playbooks** (not code)
3. Track what's manual and painful — those become platform features later
4. Charge premium rates for the human expertise, not the infrastructure

**The key insight:** The platform is a *hypothesis*, not a foundation. Validate it with revenue before investing in it.

### The One Exception

If Louis has clear evidence that:
- A specific client segment will only buy a *platform* (not a service engagement), AND
- Competitors are already building and Alizé will be late to a established market

Then platform investment is justified. But this requires evidence, not assumption.

---

## Summary

| Question | Answer |
|----------|--------|
| **Can Alizé deliver pilots as pure-service for 6-12 months?** | Yes — and it's the lower-risk path |
| **What does platform buy vs. consulting?** | Scale at Year 2+, but risky at Year 1 |
| **Risk of building before validating?** | High — potential €150-250K burn for a product no one renews |
| **Use existing tools instead?** | Yes — n8n, Make.com, crewAI can cover 80% of Year 1 needs |

**Bottom line:** Kill the parallel platform build. Go service-first. Build the platform (or decide to compose it) in Month 12+ based on real renewal data. The platform is not the foundation — *revenue and renewals* are the foundation.

---

*This document challenges the platform-first assumption. The opposing view (platform-first) has merit if Alizé has strong evidence of a platform-only market segment or has secured funding to absorb the burn.*
