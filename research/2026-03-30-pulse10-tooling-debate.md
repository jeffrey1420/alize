# Alizé Research Pulse 10 — 2026-03-30 — Tooling Strategy Debate

**Date:** 2026-03-30
**Pulse:** 10th research pulse
**Topic:** n8n vs Make.com vs DIY — the interim tooling decision

---

## Assumption Challenged

> "Use n8n or Make.com for interim service delivery" — this is treated as a technical detail. It is actually a strategic business decision with 18-month consequences.

---

## The Three Options

**n8n:** Open-source, self-hostable, strong developer community, 400+ integrations. French server hosting viable. Technical ceiling: can build almost anything. Pricing: free self-hosted, cloud from ~€20/month.

**Make.com:** SaaS-only, 1,500+ apps, very visual, French PME friendly UI. Enterprise connectors (Salesforce, SAP) exist but at higher tiers. GDPR: EU data residency available. Pricing: from ~€9/month/user, enterprise plans custom.

**DIY (custom):** Build on Mastra or direct API calls. Highest flexibility, highest cost, highest time-to-first-client.

---

## The Strategic Risk Matrix

| Risk | n8n | Make.com | DIY |
|------|-----|----------|-----|
| Vendor lock-in | Medium (self-hostable mitigates) | High (SaaS, no exit) | Low |
| French PME enterprise connectors | Medium (Cegid/Sage: community-built) | High (Cegid, Sage, Pennylane at enterprise tier) | High effort |
| GDPR/compliance | High control (self-host in EU) | Medium (EU data residency available) | Full control |
| Client handover complexity | Medium (client needs technical person) | Low (visual, hand-off friendly) | Low |
| Long-term platform migration | Easy (export workflows as JSON) | Hard (proprietary format, no export) | Easy |
| Louis learning curve | Steep (developer tool) | Gentle (visual, fast to learn) | Steep |

---

## The Critical Question: Client Handover Risk

Here is the risk nobody is talking about:

**If Alizé builds 8 client workflows on Make.com and Make.com:**
- Raises prices 3x → Alizé absorbs or passes on
- Gets acquired by a US company → GDPR compliance questions
- Has an outage → Alizé clients blame Alizé
- Changes their enterprise connector pricing → Alizé has to renegotiate with clients

**With n8n self-hosted on OVHcloud:** Alizé controls the infrastructure. The client's workflows run on Alizé's servers. Alizé can demonstrate French data sovereignty. If the client wants to leave, Alizé exports the n8n workflows and migrates to a new host. Alizé owns the stack.

**The handover test:** If the client asks "can we take our workflows and go?" — with n8n, yes. With Make.com, no.

---

## The Make.com Trap for Alizé's Positioning

Make.com is excellent for SMBs who want to self-serve. Alizé's positioning is the opposite: "we manage everything, you don't touch the tool." Using Make.com (a self-service tool) to deliver a managed service creates a cognitive dissonance problem:

- Alizé: "we handle everything"
- Make.com UI: "drag and drop your own integrations"

If a client's team member discovers Make.com's UI, two things happen:
1. They realize they could do it themselves
2. They wonder what Alizé is actually adding

This doesn't happen with n8n because there's no end-user UI to discover.

---

## Verdict

**Use n8n.** Specifically: n8n self-hosted on OVHcloud, not the n8n cloud offering.

Why:
- French data sovereignty is real (OVHcloud EU hosting)
- Client handover = export/import of workflow files
- Alizé controls the infrastructure end-to-end
- GDPR: data never leaves Alizé's OVHcloud instance
- MCP integration: n8n has MCP server support in beta
- Cost: free self-hosted, Alizé just pays the OVHcloud VPS (~€20-40/month)

The learning curve is real but Louis is a developer. He can learn n8n in a weekend.

**What to do Month 1:**
1. Spin up an OVHcloud VPS with n8n self-hosted
2. Build the first client workflow in n8n
3. Document the n8n workflow structure as the beginning of the delivery playbook
4. Do NOT use Make.com for any client work until the strategic question of platform is resolved at Month 12

---

## Decisions Reached

| ID | Topic | Decision | Source |
|----|-------|----------|--------|
| D34 | Interim tooling | n8n self-hosted on OVHcloud, NOT Make.com | Tooling debate |
| D35 | Client lock-in | Zero lock-in with n8n (exportable); high lock-in with Make.com | Tooling debate |
| D36 | Positioning alignment | n8n aligns with "managed service" positioning; Make.com conflicts with it | Tooling debate |

## New Unresolved Items

| ID | Topic | Blocker |
|----|-------|---------|
| U35 | n8n learning | How fast can Louis get productive on n8n? Does Month 1 pilot allow time for this? |
| U36 | n8n MCP support | Is n8n's MCP integration stable enough for Alizé's use case? Needs technical validation |
| U37 | n8n vs Make.com cost comparison | At 8 clients, what's the total tooling cost difference? |
