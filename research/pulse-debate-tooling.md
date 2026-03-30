# Pulse Debate: n8n vs Make.com — The Strategic Reality

## The Assumption Under Challenge

> "Use n8n or Make.com for interim service delivery."

This framing treats the tooling decision as an implementation detail. It's not. It's a strategic positioning choice that determines Alizé's dependency structure, client lock-in dynamics, and platform migration optionality for the next 2-3 years.

---

## The Verdict: n8n. Not Close.

**Make.com is a consumer SaaS product dressed as enterprise tooling.** For a French managed service positioning on data sovereignty, EU AI Act compliance, and professional governance — Make.com is the wrong foundation.

Here's why:

### 1. Make.com vs n8n for French PME/ETI

| Dimension | Make.com | n8n |
|-----------|----------|-----|
| **HQ** | US (California) | Germany (Berlin) |
| **Data hosting** | Make.com servers | Self-host on OVHcloud |
| **French market** | English-first, limited FR support | Active FR community, European |
| **Sage/Cegid/Pennylane** | Better connectors (mature) | Functional but less polished |
| **GDPR compliance** | US data processing addendum required | Full control, French infra |
| **EU AI Act narrative** | Doesn't exist | French-hosted = defensible |
| **Pricing trajectory** | Raised prices 3x since 2022 | Open-source core, stable pricing |
| **Acquisition risk** | High (backed by PE, profitable SaaS) | Independent, VC-backed but self-host option |

**The Frenchinfra argument is not theoretical.** Alizé's positioning is "AI agents on French servers, under your access rules, EU AI Act-native." Make.com breaks this narrative the moment a French DPO (Délégué à la Protection des Données) asks where client data flows. n8n on OVHcloud answers that question in one word: France.

### 2. The Lock-In Risk Is Asymmetric

Build 6 clients on Make.com. Make.com raises prices 3x at renewal. What happens?

- **Alizé absorbs the increase** → erodes margin on every client
- **Alizé passes it through** → clients question why their "AI service" runs on a consumer automation tool
- **Alizé migrates away** → 6 client workflows, each 20-40 hours to rebuild

With n8n, the equivalent scenario: n8n Cloud raises prices. Alizé spins up the open-source version on the already-provisioned OVHcloud instance. Migration cost: hours, not weeks.

**Make.com also has acquisition risk.** It was acquired by Celonis in 2024. Integrify was acquired by UiPath. The no-code automation space is consolidating. When Make.com gets folded into a larger ecosystem, Alizé's workflows become legacy artifacts.

### 3. Platform Migration at Month 12

The "we'll decide at Month 12" question is actually a question about **workflow portability**.

Make.com workflows are visual scenarios with proprietary logic. They cannot be exported as structured code. Migrating from Make.com to a custom platform at Month 12 = rebuilding everything from screenshots and memory.

n8n workflows are JSON with a structured schema. They're not perfect, but they're portable. If Alizé decides at Month 12 to build its MCP-first platform, n8n workflows are at least a referenceable starting point.

**n8n makes the platform decision richer. Make.com eliminates it.**

### 4. Who Actually Owns the Client Relationship?

This is the most underappreciated point.

With Make.com: The client receives workflows running inside Make.com's ecosystem. If Alizé disappears tomorrow, the client can theoretically continue with Make.com (and hire someone else to maintain the workflows). Alizé has delivered value but created no structural dependency.

With n8n (especially self-hosted): The workflows are deployed in the client's infrastructure environment (or Alizé-controlled infrastructure). The client **needs Alizé** to maintain and evolve the system. This is the foundation of the recurring revenue model.

**Make.com is a client extraction tool. n8n is a client retention tool.** Given Alizé's managed service positioning, this matters enormously.

---

## Strategic Risk of Each Option

**Make.com risks:**
- Data sovereignty narrative collapses on first client DPO review
- Price increase at renewal destroys margin predictability
- Acquisition by competitor creates forced migration
- Competitor Agentova also uses Make.com — no differentiation
- Client can leave Alizé and find another Make.com consultant

**n8n risks:**
- Connector quality for Sage/Cegid less polished than Make.com (solvable with custom MCP)
- Steeper learning curve for Alizé team initially
- Open-source maintenance overhead (mitigated by OVHcloud-hosted cloud version)

---

## What Alizé Does RIGHT NOW (Month 1)

**Step 1:** Install n8n on OVHcloud (already provisioned). Use the cloud version for Month 1-2 while learning.

**Step 2:** Pick the 3 highest-value client workflows from the first pilot. Build them in n8n. Document everything in a shared ops runbook.

**Step 3:** For any French enterprise connector (Sage, Cegid, Pennylane) where n8n's connector is lacking — build a custom MCP server on OVHcloud. This is the actual differentiator anyway: not which automation tool, but whether Alizé can connect to any tool via MCP.

**Step 4:** At Month 12, the platform decision is now informed by real workflow pain points from n8n — not abstract architecture planning.

---

## Bottom Line

The "n8n or Make.com" debate is a false equivalence. Make.com is fine for solopreneurs automating their own inbox. For a French managed AI service whose entire value proposition is data sovereignty, governance, and professional delivery — **n8n is the only coherent choice.**

Make.com now: comfortable, fast setup, familiar.
n8n now: aligned with positioning, future-proof, client-retentive.

The question isn't which tool is easier. It's which tool makes Alizé's business model actually work.
