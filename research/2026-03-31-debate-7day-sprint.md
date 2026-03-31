# 7-Day Sprint Plan for Alizé

**Date:** 2026-03-31  
**Role:** First Action Strategist  
**Context:** 75+ pulses, 290+ decisions, zero external actions. Domain unbought. No pilot client. No live product.

---

## The Cost of Continuous Research

The pulse process costs Louis **real time and real money**, and produces nothing that increments Alizé's position.

**Time cost:** Each pulse cycle burns 20-40 minutes of Louis's attention — time stolen from Grinto internship work, Kuroba co-founder duties, and actual Alizé execution. Over 75 pulses, that's **25-50 hours** of research theater. Louis is an M1 student with an internship. He does not have 25-50 hours to spend on a closed loop that talks to itself.

**Opportunity cost:** Kuroba exists because it was **built** — a landing page, a contact email, a deployment. Alizé has 162KB of TODO.md and 28 debate files and zero deployed artifacts. The pulse process has been doing the thing that feels like starting Alizé, instead of starting Alizé.

**Financial cost:** The domain is €10. Louis has spent roughly €0 on Alizé because he has not committed €10 to own the name. That single act — which takes 5 minutes — requires more commitment than any pulse has extracted.

**The specific structural failure:** The process is a closed loop. Cron fires → agents write debate files → decisions added to TODO → cron fires again. No external contact point exists. The debate log cannot stop itself. Four prior suspension decisions (D249, D258, D213, D240) were documented and ignored because a text file cannot execute a cron disable command. The loop has one participant (Louis) and zero external contact points.

**The actionable finding already in the log that has not been acted on:**
- Domain: not bought
- First pilot: not identified
- Landing page: not built
- LinkedIn message to pilot prospect: drafted (P67) but not sent

---

## Day-by-Day 7-Day Plan

### Day 0 — Today (Tuesday March 31, pre-sprint)

**Action: Buy alize.ai or alizeautomation.com right now.**

- Go to Namecheap/Gandhi/OVH
- Search for alize.ai
- If taken, check: alize-ai.com, getalize.com, alizeautomation.com
- Cost: ~€10/year
- Time required: 5 minutes
- This is the single highest-leverage action available. Everything else flows from owning the domain.

If Louis cannot do this today, he must ask himself why not. There is no blocker. This is a purchasing decision, not a technical one.

---

### Day 1 — Wednesday April 1

**Action 1: Send one email to one potential pilot client (digital agency or e-commerce, 15-30 employees)**

- Use Louis's existing network. Not cold outreach — warm.
- Template angle: "I'm building Alizé, a workflow automation tool for [agencies like yours]. Would you be open to a 20-minute call to tell me how you currently handle [a specific pain point]? No pitch, just research."
- One email. One person. Sent today.
- If no response in 48h, send one follow-up.

**Action 2: Stop the cron job.**

- Run `openclaw cron list`
- Find the Alizé pulse cron job
- Run `openclaw cron remove [job-id]`
- This is not a pause. This is a termination. The loop cannot self-terminate. Louis must do it manually.

---

### Day 2 — Thursday April 2

**Action: Build the landing page (even a rough one)**

- Take the hero copy already drafted in `/data/workspace/alize/landing-page.md`
- Deploy a static page on Kuroba's infrastructure (already available per TOOLS.md — Coolify at admin.lschvn.foo)
- One page: headline + problem statement + free diagnostic CTA + contact form
- Do not polish. Ship.
- Time budget: 2-3 hours max
- If Kuroba landing page isn't ready, use a free Carrd.co site pointing to the eventual domain

---

### Day 3 — Friday April 3

**Action: Publish and share**

- Landing page is live (on a temp domain if alize.ai not yet registered)
- Share with the person emailed on Day 1: "I built this, thoughts?"
- Share with one trusted person in Louis's network — not for feedback, for social proof that Alizé exists
- Post one LinkedIn post or Story: "Building in public — Alizé, workflow automation for [your specific angle]. First prototype live."

---

### Day 4-5 — Weekend (April 4-5)

**Rest. Grinto internship prep if needed. Kuroba work.**

If Louis has energy: draft the first pilot agreement template (Google Doc, Notion, anything). Not a legal document — a scope of work outline: what the pilot delivers, how long it runs, what happens at the end.

---

### Day 6 — Monday April 6

**Action: Pilot pipeline**

- Follow up with everyone who hasn't responded from Day 1
- Send the same email to 2 more warm contacts (network of a network)
- Target: 3 total conversations scheduled by end of day

---

### Day 7 — Tuesday April 7

**Action: First discovery call or nothing**

- If any discovery call is scheduled: conduct it. Use the discovery form in `/data/workspace/alize/docs/DISCOVERY-FORM.md`
- If no calls scheduled: the problem is the outreach, not the product. Fix the outreach.
- Either way: document what was learned. Not in a debate file. In a one-paragraph note in `/data/workspace/alize/notes.md`

---

## What to Stop Doing

**Stop running the pulse process immediately.**  
Four suspension decisions (D249, D258, D213, D240) have been documented and ignored. The cron job will fire again in hours. Louis must manually run `openclaw cron remove`.

**Stop adding decisions to the debate log.**  
290 decisions are enough. The remaining unresolved items (domain, pilot client) are not research problems — they are execution problems. No amount of debate will make the domain cheaper or the pilot client more willing.

**Stop debating what Alizé is before anyone outside Louis's head has seen it.**  
The landing page copy, pricing, positioning — all of it is hypothesis until a real human reacts to it. A/B test with reality, not with more debate.

**Stop treating "better decisions" as a precondition for action.**  
The meta-pattern auditor correctly identified the core false assumption: that more clarity would lead to execution. The barrier is not insufficient information. The barrier is commitment. Louis already knows enough to buy a domain, build a landing page, and send one email.

**Stop using "I need my co-founder / delivery partner" as a reason to delay.**  
Every criterion in this plan is executable by Louis alone. The Kuroba infrastructure exists. The domain costs €10. The pilot client lives in Louis's LinkedIn. Louis does not need a co-founder to send an email.

**Stop treating research as the work.**  
Kuroba was built. Alizé was debated. This is the operational lesson: building creates velocity; debating creates the feeling of building.

---

## What Ships This Week

### Minimum Viable External Action — Ships Friday April 3

**One-page landing page live on the web, with a free diagnostic CTA, linked from Louis's LinkedIn.**

This is the absolute minimum that creates real momentum:

1. **External signal:** Something exists on the internet that Louis can point to
2. **Social proof:** Louis can share a URL, not a PDF
3. **Lead capture:** Email address from anyone who clicks the CTA
4. **Iteration fuel:** Real human reactions replace imagined user personas

The landing page does not need to be beautiful. It needs to exist.

### What Does NOT Ship This Week (Rejected)

- A fully designed brand identity
- A pricing page with three tiers
- A self-improving AI agent stack
- A 12-month GTM roadmap
- A competitor analysis deck
- Any new research pulse

---

## One-Sentence Summary for Louis

**Alizé has 290 decisions and zero deployed artifacts — buy the domain today, build a one-page landing site tonight, send one email to a warm contact tomorrow, and kill the research cron loop before it fires again.**
