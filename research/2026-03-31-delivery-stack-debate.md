# Delivery Stack Challenge — 2026-03-31

## Assumption Challenged
Zapier/Make.com is an appropriate delivery vehicle for Alizé's managed/governed positioning.

## 3 Contradictions

1. **"Managed" vs. client-side configuration burden.** Alizé's core promise is that "we deploy and manage the agents for you." Zapier/Make.com offload workflow configuration to the client — the client must build, maintain, and debug their own automation paths. This directly inverts the managed service promise: the client becomes the integration engineer, not Alizé.

2. **"Governance" vs. fragmented governance surface.** Alizé positions on security, guardrails, audit logging, and EU AI Act compliance. Zapier introduces its own permission model, execution logs, connector OAuth flows, and third-party data paths — all outside Alizé's control plane. When a regulated client (legal, healthcare, finance) asks "who approved this action and when," Zapier's log is not Alizé's log. Governance becomes a patchwork of two platforms with no unified audit trail.

3. **"Professional" positioning vs. consumer-grade tooling.** Alizé prices at €800-2,000/month+ with professional deployment, training, and monitoring. Zapier is priced for individuals and small teams doing personal automation. Using Zapier as Alizé's runtime means a client paying for a premium managed service is actually running on a €20/month consumer-tier tool with Alizé as a middle layer — a positioning contradiction visible the moment a prospect asks "what platform runs our agents?"

## Client Scenario

A 120-person accounting firm (ideal ICP: professional services, 50-250 employees, GDPR-sensitive, already uses Cegid, Outlook, SharePoint) requests a pilot. During discovery they ask: "Can you show us your security architecture? We're subject to EU AI Act and our data protection policy requires a full audit trail for any automated system touching client financial data." The sales team demos the agent workflow — but the underlying runtime is Zapier. The client's IT lead immediately asks: "So Zapier has access to our Cegid data? What's their data processing agreement? Who controls the API credentials?" Alizé cannot answer these questions without disclosing that the "managed service" runs on a consumer automation platform the client could buy themselves for €20/month. The objection is not "too expensive" — it's "your governance claim is hollowed out by your tooling choice." The sales team cannot overcome it without either lying about control or admitting the managed positioning is a veneer over Zapier.

## Verdict: KILL

Zapier/Make.com was chosen as a bootstrap shortcut (D256), but it structurally undermines the three pillars Alizé's entire positioning rests on. A client paying €1,500/month for a governed, professionally managed AI agent service cannot be handed a Zapier workflow builder as the delivery mechanism without exposing a credibility gap the sales team cannot close.

## Concrete Alternative

**Lightweight agent control plane:** A minimal server-side orchestration layer using:
- **Language:** TypeScript/Node.js on a single VPS (OVHcloud, same infra as planned)
- **Runtime:** Mastra Core (lightweight, no full stack required yet) or a simple webhook + agent loop
- **State:** Airtable or Supabase for workflow state and execution logs
- **LLM:** OpenAI API (already planned)
- **Trigger:** Webhooks from client tools (no Zapier middle layer)

This keeps integration logic server-side under Alizé's control, preserves a unified audit log Alizé owns and can present to clients, creates no consumer-tier surface for prospects to object to, and is buildable in days — not months. Defer the full Mastra + pgvector + Redis stack to post-5-clients (D254), but the delivery runtime itself must not be a consumer automation tool masquerading as a managed service.
