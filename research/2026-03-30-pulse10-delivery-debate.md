# Alizé Research Pulse 10 — 2026-03-30 — Delivery Model Debate

**Date:** 2026-03-30
**Pulse:** 10th research pulse
**Topic:** Delivery model — who actually delivers the first pilot

---

## Assumption Challenged

> "Louis delivers Month 1 manually + documents → contractor executes Month 2+ against playbook"

This assumes Louis is the right delivery person. It also assumes a written playbook is sufficient to transfer tacit knowledge. Both are questionable.

---

## Debate 1: Is Louis the right delivery person?

**The problem:** Louis is an M1 master student and web developer intern. He is not a business process consultant. Delivering an AI agent pilot for a client requires:

- Process mapping (understanding what the client actually does, not what they say they do)
- Stakeholder management (getting the right people to change how they work)
- Change management (getting teams to adopt new tools)
- ROI measurement (quantifying outcomes in business terms the client cares about)
- French business etiquette and communication standards

Louis has none of these. He has technical skill and enthusiasm.

**The risk:** The pilot client gets a technically working agent that doesn't change how the business operates. The agent does the task but the team doesn't adopt it. The conversion to retainer fails not because the technology is bad but because the change management was missing.

**The blunt truth:** Would you hire a freshly-minted developer to lead a数字化 transformation project at a 100-person French company? No. You'd want someone with business credibility, even if they don't write the code.

**Verdict on this sub-assumption:** Louis-as-delivery-lead is the wrong model. Louis should be the technical executor. The delivery lead should be someone with business credibility from day one.

---

## Debate 2: Does "document everything" actually transfer knowledge?

**The problem:** Delivery playbooks are written in the language of the person writing them. Business context, client-specific quirks, stakeholder sensitivities, the unwritten rules — these are tacit knowledge that lives in conversations, not documents.

The first pilot will generate 30-50 decisions that aren't obvious. Writing "call Marc at the client before day 10" doesn't capture *why* Marc needed a call, what to say, or what the warning signs were.

**The risk:** A playbook written by a developer sounds like a technical runbook. A playbook for a business process consultant looks like a engagement guide with stakeholder maps, escalation triggers, and business outcome checkpoints.

**The verdict:** A written playbook is necessary but not sufficient. The first delivery person needs to be someone whose primary skill is client-facing business consulting, not technical execution.

---

## What the minimum viable delivery team actually looks like for Month 1

| Role | Responsibility | Who |
|------|---------------|-----|
| Business lead | Client relationship, process mapping, ROI measurement, change management | Someone with PME consulting experience — NOT Louis |
| Technical executor | Agent setup, n8n/Make.com workflows, MCP integration, testing | Louis |
| Quality reviewer | Second pair of eyes on agent output, accuracy validation | Contractor or Louis |

Louis does the technical work he can do. A delivery partner (even part-time) handles the business side.

---

## 3 Concrete Recommendations

1. **Find a delivery partner before the first pilot, not after.** This person should have PME consulting experience, French business network, and be comfortable with AI as a tool (not a specialty). Even 2 days/week for Month 1 is enough. The alternative is a pilot that technically works and commercially fails.

2. **The first pilot is not a product test — it's a delivery rehearsal.** The goal is to prove the delivery model works, not that the technology works. Technical proof is assumed. Commercial proof requires business change, which Louis cannot deliver alone.

3. **Define the delivery playbook's format before writing it.** It should have: stakeholder map, process map, success metrics (stated in client language), change management checkpoints, and escalation triggers. A technical runbook is not a delivery playbook.

---

## Decisions Reached

| ID | Topic | Decision | Source |
|----|-------|----------|--------|
| D31 | Delivery model | Louis-as-delivery-lead is wrong; business lead needed from Day 1 | Delivery debate |
| D32 | Delivery timing | Find delivery partner BEFORE first pilot, not Month 2 | Delivery debate |
| D33 | Playbook format | Delivery playbook = business engagement guide, not technical runbook | Delivery debate |

## New Unresolved Items

| ID | Topic | Blocker |
|----|-------|---------|
| U32 | Delivery partner | Who? Where to find? Budget for this role? |
| U33 | Louis's role definition | What exactly does Louis own vs the delivery partner? |
| U34 | First pilot timeline | When is the first pilot starting? Delivery partner sourcing takes time |
