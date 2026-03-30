# Alizé Pulse 36 — Delivery Capability Debate
## Agent: Delivery Economics Auditor
### Date: 2026-03-30

---

## Assumption Challenged

**D170 / D113:** "Louis delivers Pilot 1 solo" and "Louis delivers pilot 1 manually."

This assumption has never been stress-tested against the specific technical requirements of delivering a *working AI agent* to a *real web agency client*. The debate has focused on commercial capability (D90, D84), pricing, and GTM motion. The technical delivery question — whether Louis can *actually build and deploy* a functioning AI agent that a web agency would pay €2,000 for — remains unexamined.

The assumption treats "delivery" as a scoping and timing question. It is actually a **technical skills gap** question.

---

## Key Findings

### Finding 1: Web Development ≠ AI Agent Delivery

Louis's documented skill set is a **web development internship at Grinto**. This is the complete technical profile we have for him based on all existing Alizé research.

What web development at Grinto covers:
- Frontend: HTML, CSS, JavaScript, likely Vue or Nuxt (per the Alizé tech stack using Nuxt 3)
- Backend: Possibly Node.js, Hono framework
- APIs: Connecting to third-party services, building REST endpoints
- Deployment: Docker, basic server configuration

What web development does **NOT** cover:

| Technical Requirement | Web Dev Skill | Gap Severity |
|---------------------|---------------|--------------|
| **LLM orchestration / agent chaining** | None | Critical |
| **Prompt engineering for production** | None | Critical |
| **MCP client setup and tool binding** | None | Critical |
| **RAG pipeline (chunking → embedding → pgvector → retrieval)** | None | Critical |
| **Vector database administration (pgvector)** | None | Critical |
| **Error handling for autonomous agent actions** | None | Critical |
| **Production monitoring and alerting** | None | High |
| **Role-based access control for AI agents** | None | High |
| **Agent state management and memory** | None | High |
| **Fallback logic when LLM produces invalid output** | None | High |
| **Webhook reliability and idempotency** | Partial | Medium |
| **Production debugging of non-deterministic systems** | None | Critical |

**The gap is not minor.** LLM-based agents are fundamentally different from web applications. A web app behaves the same way every time. An agent can fail in ways that are hard to reproduce, produce plausible-but-wrong outputs, or take unexpected action paths. Debugging a production AI agent requires understanding token limits, temperature effects, context window management, and retrieval quality evaluation — none of which are part of a standard web dev curriculum.

**Finding 2: "Manual delivery" is not a workaround — it makes the problem worse.**

D113 says "Louis delivers pilot 1 manually." The implication is: if Louis can't build the technical system, he'll run the workflow manually and simulate what the agent would do. This was proposed as a delivery rehearsal approach.

This fails for a web agency client for two reasons:

**Reason A: The €2,000 client expects a working agent, not Louis doing the work.**

If the web agency client pays €2,000 and gets Louis manually performing the workflow they're already doing themselves, the client has paid €2,000 for nothing. The pilot fails not because the technical system isn't built — it fails because the value proposition ("an AI agent that handles X") is not demonstrated. The client can do X themselves. The pilot ends with the client asking: "why did I pay €2,000 for you to do my job?"

The "manual delivery" framing confuses *delivery rehearsal for Louis* with *delivery to the client*. Louis can use manual execution to *learn* the workflow, but the client must receive an automated agent.

**Reason B: A web agency's definition of "working" is not the same as Louis's.**

A web agency's operations staff thinks in terms of:
- Does the agent respond when a new support ticket arrives in Slack?
- Does it create the right Notion task with the right fields?
- Does it send the client an email update automatically?
- Does it escalate correctly when it doesn't know the answer?

Louis manually doing the work answers none of these questions. The client is buying an *agent that runs*, not a *consultant who performs the task*.

---

### Finding 3: The Minimum Viable Agent a Web Agency Will Pay €2,000 For

This has not been defined. U171 ("define specific €2,000 flat offer — one workflow, one sentence") is unresolved. Without this, D170 cannot be validated.

What a web agency actually needs to pay €2,000 for an AI agent:

**Minimum bar:**
- The agent must execute autonomously (not require Louis to trigger it)
- The agent must integrate with tools the agency already uses (Slack, Notion, email, HubSpot, domain registrar APIs)
- The agent must produce output that the agency's team actually uses (not just generates a report nobody reads)
- The agent must handle errors gracefully (not flood a channel with repeated failed attempts)

**What "working" looks like to a web agency:**

| Workflow Option | Deliverable | Value to Agency |
|----------------|-------------|----------------|
| Client request triage | Routes incoming Slack messages or emails to correct team member + creates Notion task with relevant context | Saves 30-60 min/day of manual routing |
| Monthly reporting | Pulls Google Analytics / ad platform data, generates formatted PDF report, sends to client | Saves 2-3 hours of manual data gathering per report |
| Domain/hosting monitoring | Checks uptime across client sites, alerts team via Slack when something is down | Replaces manual monitoring task |
| Content brief generation | Takes a client's brief request, generates structured creative brief for internal team | Saves 20-30 min per brief |

**The most deliverable workflow for Louis solo:**

**Client ticket triage + Notion task creation** — triggered by a Slack message to a specific channel, creates a Notion database entry with title, priority, client name, and description fields, assigns to the correct team member based on keywords.

Why this is deliverable solo:
- Single trigger (Slack webhook)
- Single output (Notion API call)
- No RAG pipeline needed (no document retrieval)
- No multi-step reasoning chain
- Testable: Louis can verify it works by sending a test Slack message

Why it's valuable enough for €2,000:
- Saves 30-60 min/day of manual triage for a 3-5 person agency
- That's 10-20 hours/month — €100-200/month value for a €2,000 one-time pilot
- Tangible: the Notion task appears, the Slack confirmation fires

**What Louis needs to build for this:**
1. Slack outgoing webhook → receives ticket notification
2. LLM call to extract: client name, issue type, priority, responsible team member
3. Notion API call to create database entry
4. Slack confirmation message in channel
5. Error handling: if Notion API fails, alert Louis (not flood the channel)
6. Logging: store every ticket processed for the pilot ROI report

This is a 2-3 day build for someone with LLM API integration experience. For Louis learning on the job, it could be 1-2 weeks of iteration.

---

### Finding 4: The Web Agency Conflict (U173) Is Not Just a Conflict — It's a Disqualifier

D181 confirms web agencies as the first vertical. D100 (web agency as internal delivery rehearsal target) was challenged in earlier debates. But the specific combination of D181 + U173 creates a problem that hasn't been fully articulated.

**The conflict has two layers:**

**Layer 1 — Competition conflict (obvious):**
Louis co-founded a web agency with Maëli and Gabin. If that agency competes with the web agencies Louis is targeting for Alizé pilots, those prospects will see him as a competitor. Even if Louis's agency doesn't actively compete today, selling AI agent services to other web agencies puts him in direct competition with his own co-founded business.

**Layer 2 — Playbook leakage (structural):**
D88 says the delivery playbook IS Alizé's technical moat. If Louis delivers the first Alizé pilot to any web agency, and that agency is at all entrepreneurial (which web agencies are, by definition — they're running a business that sells digital services), they'll reverse-engineer the playbook. They now know:
- How Alizé qualifies clients
- What workflows Alizé targets
- How Alizé prices and structures pilots
- What the delivery process looks like

If the target web agency is Maëli/Gabin's agency, this is even worse: the people who co-founded the agency with Louis have first-mover access to Alizé's playbook and could replicate it for their own clients.

**The specific contradiction:**

D181 says: "Web agencies/digital product companies confirmed as first vertical (not expert-comptables)."

D100 says: "Internal pilot target = web agency (Maëli/Gabin web agency)."

If the Maëli/Gabin web agency is simultaneously:
1. The internal delivery rehearsal target
2. A prospective Alizé client
3. A potential Alizé competitor

Then the web agency internal pilot cannot produce credible external case studies (D103 already flags this). And any other web agency targeted as a client will eventually learn that Louis co-founded a web agency — at which point the competition concern surfaces anyway.

**The assumption that's wrong here:**

The assumption that the web agency conflict can be *managed* (through ring-fencing, scope limitations, or explicit agreements) rather than *avoided*. For Year 1, while Alizé has no case studies and no differentiated product, the conflict is disqualifying, not manageable.

---

### Finding 5: The €2,000 Flat Pilot Price Doesn't Cover Louis's Learning Curve

D179: "€2,000 flat pilot (€1,000 at signed contract + €1,000 at delivery)."

Even if Louis had the technical skills, the economics are tight. For a web agency client:

- Louis needs to deliver a working agent
- He needs to run it for 30 days to demonstrate ROI
- He needs to produce a case study

If Louis spends 3-5 days learning MCP integration, 2-3 days learning RAG pipelines, and 2-3 days learning Mastra agent orchestration — before writing a single line of client code — the pilot economics break down.

At Louis's implied day rate (€200/day opportunity cost per D73), just the technical learning investment before client delivery starts could consume €1,400-2,200 of the €2,000 pilot fee.

**The real math:**
- Technical learning (MCP + RAG + agent runtime basics): 5-8 days
- Client-specific build (Slack integration, Notion API, LLM prompts): 3-5 days
- Testing and debugging: 2-3 days
- Client training and handoff: 1 day
- **Total: 11-17 days of Louis time**

At €200/day opportunity cost: €2,200-3,400 in Louis's time cost alone.

The €2,000 pilot fee doesn't cover Louis's learning curve. This is only viable if the first 1-2 pilots are treated as *pure training investment* — which means they should be with partners or friendly contacts who understand the learning stage, not paying clients expecting a production-ready agent.

---

## Verdict

**D170 ("Louis delivers Pilot 1 solo") is conditionally correct but technically unsubstantiated.**

The condition: Louis can deliver Pilot 1 solo *only if*:
1. The pilot workflow is narrow enough to be built without MCP/RAG complexity (single trigger, single API call, no document retrieval)
2. Louis spends 1-2 weeks on technical ramp-up before the pilot engagement starts
3. The pilot is explicitly scoped as "Phase 1: learning + proof" not "working production agent"
4. The €2,000 is treated as a learning investment, not a priced service

If any of these conditions fail, Louis cannot deliver Pilot 1 solo in its current framing.

**D113 ("Louis delivers pilot 1 manually") is confirmed wrong.** Manual execution does not produce a deliverable the client can use. It produces a learning experience for Louis. These are not the same thing.

**U173 (web agency conflict) is unresolved and likely disqualifying** for using any web agency — including Maëli/Gabin's — as either an internal rehearsal target or an external client pilot while Louis remains a co-founder of a competing web agency.

---

## What Changed

1. **U170 is now a real blocker, not just an open question.** The TODO shows U170 as "UNRESOLVED — is Louis actually able to build/deploy an AI agent for a real client?" This was listed alongside U171, U172, U173. U170 is the most critical of the four because the other three depend on it. If the answer to U170 is "no," then U171 (define the offer), U172 (find 20 web agencies), and U173 (resolve conflict) are all moot.

2. **D181 (web agencies as first vertical) creates a specific technical delivery problem that expert-comptables didn't have.** Web agencies are technically sophisticated buyers who will immediately test whether the agent actually works. They use Slack, Notion, GitHub, and API-based tools daily. They will know within 24 hours if the agent is running or if Louis is doing the work manually.

3. **The €2,000 flat pilot framing may need to be abandoned for the first 1-2 pilots.** The pricing was set without reference to Louis's learning curve cost. A better framing: the first 1-2 pilots are free internal delivery rehearsals, documented for the playbook. Pilot 3 is the first priced engagement at €2,000+.

---

## Recommendations for Louis

### Immediate (Week 1)

**1. Test your technical baseline honestly.**
Before committing to "Louis delivers Pilot 1 solo," spend 3 days actually building the simplest possible agent:
- Set up Mastra or direct LLM API integration
- Connect one MCP tool (e.g., Notion MCP)
- Write a prompt that takes a Slack message input and creates a Notion task
- Deploy it to a server
- Test it with 10 real inputs

If this takes more than 3 days, solo delivery for a client pilot is not Month 1 viable.

**2. Resolve U173 before any web agency outreach.**
The web agency conflict needs an explicit answer:
- Is the Maëli/Gabin web agency still active?
- Does Louis hold an equity stake?
- Is that agency targeting the same ICP as Alizé?

If yes to any of these, the first 2-3 pilots cannot be web agencies. Pivot to e-commerce companies (Grinto's client base), SaaS companies, or other digital product companies where Louis has relationships but no direct competitive conflict.

**3. Define U171 — the specific workflow — before scoping the pilot.**
The €2,000 offer needs to be: *"An AI agent that [specific action] for [specific role] at [web agency type], running on [specific trigger], producing [specific output]."*

Until this is written in one sentence, the pilot scope cannot be defined, and the delivery cannot be evaluated for success.

**Recommended first workflow:**
*"An AI agent that triages incoming client requests from a dedicated Slack channel, extracts the client name and issue type, creates a Notion task with the relevant details, and posts a confirmation in Slack — for a 3-10 person web agency."*

This is:
- Deliverable by Louis solo (no RAG, no MCP complexity)
- Testable in 24 hours
- Valuable to the client (30-60 min/day saved)
- Explainer in one sentence

### Short-term (Weeks 2-4)

**4. Separate delivery rehearsal from client delivery.**
Month 1 should be Louis building and testing the triage agent on his own company (Grinto, if it qualifies as a test environment) or a non-prospect friend with a web agency. Document every failure. Build the playbook as he goes.

Month 2 is the first paid pilot — with a client who understands they are getting a Month-2-level product, not a mature service.

**5. Revise the pilot fee structure.**
The €2,000 flat pilot (€1,000 at signing + €1,000 at delivery) creates a perverse incentive: Louis rushes to deliver in 2-4 weeks to get the second €1,000, potentially before the agent is actually working reliably.

Alternative structure:
- €500 at scope signed (non-refundable, covers Louis's scoping time)
- €1,500 at delivery acceptance (client confirms the agent is running as specified)
- Pilot runs for 30 days post-delivery at no additional cost (Louis monitors and debugs)

This aligns incentives: Louis gets paid for delivery, not for promising delivery.

**6. Address the pricing-to-delivery-time mismatch.**
If Louis needs 5-8 days of technical ramp-up before building any client-specific functionality, and the €2,000 pilot needs to generate enough margin to justify the effort, the only viable math is:
- Technical ramp-up happens *before* the pilot engagement starts (unpaid or cost-covered separately)
- The pilot engagement is tightly scoped (single integration, single workflow)
- The pilot price is treated as a client acquisition cost for pilots 1-2, not a profitable service

---

## Summary

| Item | Status | Recommendation |
|------|--------|----------------|
| U170: Louis solo delivery capability | **UNRESOLVED — now a hard blocker** | 3-day technical self-test before committing to solo delivery |
| U171: €2,000 flat offer definition | **UNRESOLVED** | Define as "Slack → Notion ticket triage agent" before pilot scoping |
| U172: 10-20 web agencies in warm network | **BLOCKED on U173** | Cannot identify targets until conflict is resolved |
| U173: Web agency vs Alizé conflict | **UNRESOLVED** | Answer: is Louis's co-founded web agency still active and competing? |
| D170: Louis delivers Pilot 1 solo | **CONDITIONALLY CORRECT** | Only viable if workflow is narrow (no RAG, single MCP) and ramp-up time is pre-invested |
| D113: Louis delivers pilot 1 manually | **CONFIRMED WRONG** | Client must receive working agent, not Louis doing the task |
| D179: €2,000 flat pilot | **ECONOMICALLY TIGHT** | May need restructuring to separate ramp-up from delivery payment |
| D181: Web agencies as first vertical | **CREATES SPECIFIC TECHNICAL CONSTRAINT** | Web agency clients will test the agent immediately; "working" must mean actual automation |

---

## The One Assumption That Is Definitely Wrong

**D170 ("Louis delivers Pilot 1 solo") assumes Louis can build a working AI agent for a real client. Louis has never built and deployed a production AI agent.**

His documented experience is a web development internship. Web development teaches you to build applications that behave deterministically. AI agents behave non-deterministically, require entirely different debugging approaches, and need production infrastructure (monitoring, error recovery, access control) that web development doesn't cover.

The assumption that a web dev internship = AI agent delivery capability is wrong. The two disciplines share a programming language but not a methodology.

**What this means for Alizé:**
- Louis needs a 2-4 week technical ramp-up *before* any client-facing pilot
- The first pilot should be with a forgiving client (friendly, small, understands early-stage products), not a paying client expecting working automation
- The €2,000 pilot price needs to cover actual delivery costs, not learning costs — these are different budgets
