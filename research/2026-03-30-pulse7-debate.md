# Alizé Research Pulse 7 — 2026-03-30

**Date:** 2026-03-30
**Pulse:** 7th research pulse of the day
**Agents:** Positioning Strategist, MVP Scope Strategist, Pricing Strategist

---

## Agent 1: Positioning Strategist

### Assumption Challenged
"AI as employee" framing is the right positioning for French PME/ETI decision-makers

### The Case Against
The managed-service-debate.md correctly identified that "managed service" = ESN/vendor mental model = wrong frame. But the correction went too far in the other direction. "AI employee" creates its own set of problems, and French SME reality is less forgiving than English-speaking markets on this specific axis.

**The legal weight of "employee" in French business culture is real.** France has among the world's most protective labor law (Code du travail). The word "employee" — even "AI employee" — activates a mental model that includes: obligations toward the worker, termination complexity, social charges, probation periods. A French DG who's just been told they can "hire" an AI will instinctively ask: "Hiring implies firing — what are my obligations if I want to stop this?" That question doesn't lead to a sale. It leads to a legal sidebar.

**"AI replacing employees" is an active political flashpoint in France.** The media narrative around AI in French workplaces is dominated by CGT, FO, and union-driven stories about layoffs and algorithmic control. A founder or COO who hears "AI employee" will — even if only at a subconscious level — wonder: does this product position me as someone replacing my staff? In a country where Works Councils (comités sociaux et économiques) have real power and where terminations are expensive and litigious, anything that touches the "AI vs. jobs" narrative is a headwind.

**The buyer persona prefers "tool" over "person."** French founders and COOs are not thinking about staffing when evaluating a €500-2,000/month operational tool. They're thinking about: "I have a repetitive task problem. I need it solved. I don't want to manage another thing." The brief's own stated promise is "time saved, fewer repetitive tasks, speed, control" — not "a team member." Employment framing adds psychological overhead that the outcome framing eliminates.

**The "employee" frame creates a harder commercial comparison.** When you call it an employee, buyers compare it to the cost of a real employee. A French administrative employee costs €35-45K/year all-in. At €800-1,500/month (€9,600-18,000/year), the "AI employee" is already a bargain — but the buyer may still wonder why they're paying monthly fees for something they could theoretically own. This creates a pricing legitimacy problem that "workflow automation tool" doesn't.

### Alternative Framing
**"AI-driven workflow automation" or "operational AI agents"** — with emphasis on the work done, not the worker relationship.

Specific language to test:
- "Un agent IA qui automatise vos processus métier" (An AI agent that automates your business processes)
- "L'automatisation IA qui travaille — pas de configuration, pas de maintenance" (AI automation that works — no configuration, no maintenance)
- "Vos tâches répétitives, traitées automatiquement" (Your repetitive tasks, handled automatically)

The word "workflow" is precise, unthreatening, and shifts the frame from a labor relationship to a process improvement. It sidesteps the union/political narrative entirely. It makes the ROI story cleaner: "You spend X hours/week on task Y. An agent automates that for €Z/month."

### What This Overturns
The managed-service-debate.md concluded that "AI employee" is the correct replacement for "managed service." This pulse argues the correction went too far. "AI employee" activates French labor law associations, political sensitivity around AI replacing workers, and a harder commercial comparison to real employees. The employment frame is memorable for English-speaking audiences. In French B2B, it creates cognitive dissonance.

**What it doesn't overturn:** The core insight from managed-service-debate.md (managed service = wrong frame) is correct. The replacement just needs to be "workflow automation" rather than "employee."

### Recommendation
1. Remove "AI employee" from all external-facing materials
2. Test "workflow automation" framing in first 3 sales conversations
3. Track: does "workflow" vs "employee" framing affect close rate or meeting acceptance rate?
4. Internal alignment: the team should be using "agent" as a noun (the AI agent does X) but never framing it as a hire/employee relationship

---

## Agent 2: MVP Scope Strategist

### Assumption Challenged
"The MVP must include Mastra-based agent runtime with MCP connectors before first client delivery."

### The Case Against
Building a full agent runtime platform before first revenue is **backwards**. The Mastra + MCP architecture is infrastructure investment based on assumptions about what will need automation. But there is zero real usage data. The team doesn't know:
- Which workflow steps actually bottleneck
- What Louis does manually that clients hate most
- Which integrations matter vs. which are theoretical

The "Wizard of Oz" approach works here: **Louis manually orchestrates agents, monitors outputs, course-corrects.** This proves the workflow before investing in automation. First pilot clients pay for outcomes (team improvement), not the runtime architecture. Every sprint spent on MCP connectors is a sprint not getting to revenue.

The brief lists 9 service components. A 2-person team cannot build 9 services + a full agent runtime platform in 90 days. Choosing to build infrastructure for delivering value before delivering any value is the wrong priority.

### Proposed 90-Day MVP Scope

**Days 1-30 — Core Execution Engine (No Runtime Platform)**
- Nuxt 3 + Nuxt UI frontend (client-facing dashboard only)
- Hono backend with basic REST endpoints
- pgvector on OVHcloud (vector storage, not runtime)
- One LLM integration (OpenAI or Claude) via simple API calls
- Manual workflow: Louis triggers agent steps, reviews outputs, approves before delivery
- Database: client records, audit results, team data

**Days 31-60 — First Pilot Delivery**
- Complete one full service workflow: Audit/Scoping end-to-end
- Client portal: team members take assessments, see results
- Louis dashboard: monitor active audits, flag issues, manually intervene
- Basic logging: document every step Louis does manually → this becomes the process map
- Feedback loop: client + Louis notes on what was painful

**Days 61-90 — Automation Based on Evidence**
- Identify top 3 manual tasks Louis repeated every audit
- Build targeted automations for THOSE (not assumed features)
- Basic notification system (Slack/email when Louis needs to act)
- Expand to second pilot client
- Start documenting patterns across clients

**What to NOT build in 90 days:**
- Mastra agent runtime
- MCP server infrastructure
- Full 9-service component library
- Multi-tenant platform
- Self-service client onboarding

### What This Overturns
- **Architecture assumption:** "agent runtime is the core" — the core is working workflows that improve teams, not the runtime itself
- **Build order:** platform infrastructure before revenue is validated
- **Scope claim:** BRIEF.md's 9 service components cannot all be built by 2 devs in 90 days — prioritize ruthlessly
- **MCP integration:** MCP is valuable for production-scale multi-tool agents, but the first pilot needs ONE integration working well, not many integrations waiting for clients

### Recommendation
**Week 1: Kill the runtime spike.** Do not build Mastra infrastructure. Instead:
1. Ship a basic Nuxt+Hono app with one form, one LLM call, one stored result
2. Louis manually walks first pilot client through the Audit/Scoping workflow using this
3. Document every step Louis does manually
4. At day 30, review: what would automation actually save Louis 2+ hours per audit?

Build the runtime when there is **proof** it is the bottleneck — not before.

---

## Agent 3: Pricing Strategist

### Assumption Challenged
"The diagnostic offer's primary purpose is to qualify fit and build trust before selling."

### The Case Against
The current framing treats the diagnostic as a **pre-sales qualification tool** — a checkpoint before committing to a pilot. This is wrong. The diagnostic should be a **top-of-funnel lead generation channel** that happens to qualify. Different objective = different pricing logic.

**The €490 price point assumes Alizé has brand authority to demand payment upfront for an unknown vendor.** It does not. French SME owners booking a 30-minute diagnostic with a startup they've never heard of are already showing intent — charging €490 adds friction that filters out the *exact* prospects who need Alizé most: cost-conscious SMEs hungry for efficiency gains. The €490 "credibility signal" only works when your brand already commands respect. For an unknown brand, it signals *arrogance*, not confidence.

**The assumption that paying €490 filters for serious buyers is a survivorship bias argument.** The prospects who *don't* pay are not lost causes — they're the French SME mainstream: cautious, comparison-shopping, unwilling to commit €490 to a vendor discovered on LinkedIn or Google this quarter. A free diagnostic with a written ROI estimate still requires their time and internal data. Serious buyers self-select on engagement quality, not on willingness to pay €490.

**If the diagnostic is lead gen, the correct price is €0.** The ROI estimate (a written deliverable) is what distinguishes a free Alizé diagnostic from a commodity free call. The diagnostic should be an *investment* in pipeline, not a revenue line.

### Free vs. Paid Diagnostic Comparison

| Dimension | Free Diagnostic | €490 Paid Diagnostic |
|-----------|----------------|----------------------|
| **Conversion Rate** | Higher volume; more total conversations | Lower volume; higher % of booked who show |
| **Lead Quality** | Broader — includes curious & serious | Filtered for payment-committed buyers |
| **Avg Deal Velocity** | Faster to pilot (frictionless entry) | Slower (already "bought in" but smaller pool) |
| **Brand Perception** | Accessible, confident, consultative | Risks seeming arrogant for unknown brand |
| **Competitive Positioning** | Aligns with French B2B norm (free first meeting) | Backfires if competitors offer free |
| **Pipeline Risk** | Low — mass entry; qualify downstream | High — small pool; wrong filtering criteria |
| **Revenue Per Lead** | €0 upfront; upsell to pilot | €490 upfront; smaller funnel |
| **French SME Norm** | Standard practice (experts, coaches, consultants) | Unusual for cold/early-stage outreach |

### What This Overturns
The entire BRIEF.md framing that treats diagnostic pricing as a **pricing architecture question** (where to anchor the fee) rather than a **GTM channel question** (how to generate the most qualified pipeline efficiently). The €490 framing also contradicts Pulse 6's finding that the pilot should be "at-cost with strict deliverables" — you cannot be at-cost on the pilot if you're also extracting €490 on the diagnostic. The diagnostic should be an investment in pipeline, not a revenue line.

### Recommendation
**Make the diagnostic free. Reframe it explicitly as a lead generation channel.** Structure: 30-min call + written ROI estimate delivered within 48 hours. Drop the €490 entirely — it will not generate enough signal or revenue to justify the pipeline friction. The goal is maximum qualified conversations, not maximum diagnostic revenue.

---

## Cross-Cutting Findings from Pulse 7

### Three Assumptions That Survived Scrutiny
1. Per-workflow pricing direction (confirmed from Pulse 6) — still the right model
2. Vertical focus on digital agencies/e-commerce (confirmed from Pulse 6) — confirmed again
3. At-cost pilot with strict deliverables (confirmed from Pulse 6) — pricing logic aligns

### Three Assumptions That Were Challenged
1. "AI employee" framing → should be "workflow automation" for French B2B market
2. "Mastras runtime needed before pilot" → manual orchestration first, build when proven
3. "Diagnostic at €490" → should be free, treated as lead generation not qualification

### New Findings Not Previously Debated
1. French labor law + political narrative makes "employee" framing a liability
2. MVP should be Wizard-of-Oz (manual orchestration) until runtime bottleneck is proven
3. Diagnostic pricing is a GTM channel decision, not a pricing architecture decision

---

## Decisions Reached This Pulse

| ID | Topic | Decision | Recommendation |
|----|-------|----------|----------------|
| D20 | "AI employee" framing | REMOVE — replace with "workflow automation" / "AI agent" | Test "workflow automation" language in first sales conversations |
| D21 | 90-day MVP scope | Kill runtime spike; manual orchestration first | Build Nuxt+Hono+LLM; Louis manually orchestrates first pilot |
| D22 | Diagnostic pricing | FREE — lead generation channel | 30-min call + written ROI estimate; no charge |

---

## Recommendations for Louis

1. **Update BRIEF.md** — replace "AI employee" references with "workflow automation" or "operational AI agent"
2. **Kill the runtime spike** — do not start Month 1 with Mastra infrastructure. Start with a basic Nuxt+Hono app and manual orchestration
3. **Free diagnostic** — remove €490 pricing; treat diagnostic as top-of-funnel lead gen
4. **Sales language** — never say "hire" or "employee" in external materials; say "agent automates your workflow"

---

*Pulse 7 complete — 2026-03-30 11:42 UTC*
