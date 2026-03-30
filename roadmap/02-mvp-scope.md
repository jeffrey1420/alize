# Alizé MVP Scope — First Agent Types & Pilot Design

**Version:** 1.0  
**Last Updated:** 2026-03-30  
**Status:** Foundation Document

---

## Executive Summary

This document defines the scope, design, and pilot approach for Alizé's first AI agent types. The MVP prioritizes three high-impact, low-complexity agent types that address immediate pain points for French PME/ETI: customer service (ticket handling and FAQs), lead qualification, and invoice follow-ups. Each agent type is designed for rapid deployment (14 days), measurable ROI, and impressive pilot results that drive retainer conversion.

---

## MVP Strategy

### Why These Three Agent Types?

1. **Universal Pain Point** — Every company has customer service, sales leads, and invoice chasing
2. **Clear ROI Quantification** — Hours saved and error reduction are directly measurable
3. **Low Integration Complexity** — Email and CRM integrations are standardized
4. **French Business Context** — Native French communication is a key differentiator
5. **Reusable Templates** — Each agent type becomes a template for rapid deployment

### Pilot Impressiveness Criteria

A pilot that impresses must deliver:
- **Day 1 Results** — Agent responds to queries within first 24 hours
- **Visible Time Savings** — Measurable reduction in manual task time within Week 1
- **French Fluency** — Responses indistinguishable from native French speaker
- **Zero Supervision Required** — Agent handles routine cases without escalation
- **Clear ROI Numbers** — Reduction in hours spent documented by Week 2

---

## Agent Type #1: Customer Service Agent

### Overview

The Customer Service Agent handles inbound support tickets and frequently asked questions. It integrates with email and existing helpdesk systems to provide immediate, accurate French-native responses.

### Capabilities

1. **Email Ticket Processing**
   - Read incoming support emails
   - Classify ticket type and urgency
   - Generate appropriate responses
   - Route complex tickets to human agents
   - Log all interactions

2. **FAQ Automation**
   - Answer common questions from knowledge base
   - Provide product/service information
   - Handle scheduling and reservation requests
   - Process simple changes (address update, etc.)

3. **Ticket Classification**
   - Technical issue → Engineering queue
   - Billing question → Finance queue
   - Complaint → Escalation queue
   - General inquiry → Auto-response

### French Business Context

- Formal "vous" address throughout
- French business hours response (not overnight)
- Understanding of French holiday/vacation conventions
- French legal terminology awareness
- Culturally appropriate greetings and closings

### Technical Requirements

| Component | Specification |
|-----------|---------------|
| Email Integration | IMAP/SMTP, Microsoft Graph, or Gmail API |
| Knowledge Base | Vector store with company FAQ data |
| Response Time | <5 minutes during business hours |
| Languages | French (primary), English (secondary) |
| Tone | Professional, helpful, slightly formal |

### Deployment Timeline

**Day 1-3:** Email integration, knowledge base upload, initial prompts
**Day 4-5:** Test with sample tickets, quality review
**Day 6-7:** Go-live with human-in-the-loop monitoring
**Day 8-14:** Autonomous operation, daily review, fine-tuning

### Pilot Success Metrics

| Metric | Baseline | Target | Measurement |
|--------|----------|--------|-------------|
| Ticket response time | 4 hours | 15 minutes | Email timestamp analysis |
| FAQ resolution rate | N/A | 70% auto-resolved | Ticket tagging |
| Customer satisfaction | N/A | >4/5 rating | Post-interaction survey |
| Escalation accuracy | N/A | >95% correct routing | Human audit |
| Hours saved/week | 0 | 10-15 hours | Time tracking |

### First Working Agent Spec

```
Agent Name: Assistant Clientèle v1.0
Integration: Email (IMAP) + FAQ Knowledge Base
Response Style: Formal French, empathetic, action-oriented
Escalation Triggers: Complaint keywords, refund requests, technical errors
Knowledge Cutoff: Trained on company-specific FAQ + general French business knowledge
Supervision: Daily review of 10% of responses for Week 1, weekly audit thereafter
```

---

## Agent Type #2: Lead Qualification Agent

### Overview

The Lead Qualification Agent reviews inbound leads (website forms, LinkedIn inquiries, trade show contacts) and qualifies them based on BANT criteria. It enriches lead data, assigns lead scores, and routes qualified leads to sales.

### Capabilities

1. **Lead Ingestion**
   - Website form submissions
   - LinkedIn connection requests
   - Email inquiries
   - Trade show badge scans
   - Referral introductions

2. **Lead Enrichment**
   - Company data lookup (size, industry, revenue estimate)
   - Contact information verification
   - Social profile analysis
   - News/recent activity review

3. **Qualification Scoring**
   - Budget fit (€500-3000/month)
   - Authority level (decision maker vs. influencer)
   - Need identification (clear pain point)
   - Timeline assessment (active vs. passive)

4. **Lead Routing**
   - Hot leads → Immediate sales call
   - Warm leads → Nurture sequence
   - Cold leads → Content drip
   - Bad fit → Polite decline

### French Business Context

- Recognition of French company structures (SA, SARL, SAS, etc.)
- Understanding of French business titles (PDG, Directeur, Gérant)
- Awareness of French business culture (hierarchy, decision-making patterns)
- French-language research on companies

### Technical Requirements

| Component | Specification |
|-----------|---------------|
| CRM Integration | HubSpot, Salesforce, or standalone |
| Data Enrichment | Clearbit, LinkedIn Sales Navigator API |
| Lead Scoring | Custom scoring model per ICP |
| Routing | Automatic assignment to sales rep |
| Follow-up | Email sequence triggers |

### Deployment Timeline

**Day 1-2:** CRM integration, lead data structure
**Day 3-4:** Scoring model configuration, enrichment setup
**Day 5-6:** Test with historical leads, calibration
**Day 7-10:** Go-live with sales team briefing
**Day 11-14:** Monitor scoring accuracy, adjust thresholds

### Pilot Success Metrics

| Metric | Baseline | Target | Measurement |
|--------|----------|--------|-------------|
| Lead response time | 24 hours | 2 hours | CRM timestamp |
| Qualification accuracy | 60% | >85% | Sales rep feedback |
| Meeting conversion rate | 20% | >40% | CRM stage tracking |
| Hours saved/week | 0 | 8-12 hours | Sales rep tracking |
| Lead score accuracy | N/A | >80% correlation with sales judgment | Audit |

### First Working Agent Spec

```
Agent Name: Agent Qualification Leads v1.0
Integration: CRM (HubSpot) + Email + Enrichment API
Response Style: Professional French, consultative
Routing Rules: Score >70 = immediate call, 40-70 = nurture, <40 = content
Data Enrichment: Company size, industry, technology stack, recent news
Follow-up: Personalized French email within 2 hours of lead capture
Supervision: Sales rep reviews first 20 leads, provides feedback for calibration
```

---

## Agent Type #3: Invoice Follow-Up Agent

### Overview

The Invoice Follow-Up Agent monitors accounts receivable, tracks overdue invoices, and executes collection workflows. It sends reminders, escalates to human collectors, and maintains professional customer relationships.

### Capabilities

1. **Invoice Monitoring**
   - Track invoice status in accounting system
   - Identify overdue invoices (30/60/90 days)
   - Calculate late fees per contract terms
   - Generate aging report

2. **Reminder Workflow**
   - Friendly first reminder at 7 days overdue
   - Firm second reminder at 14 days overdue
   - Urgent third reminder at 30 days overdue
   - Final notice at 60 days overdue

3. **Collection Actions**
   - Send payment links
   - Propose payment plans for large overdue amounts
   - Flag for human intervention when needed
   - Update accounting system with payment status

4. **Cash Flow Reporting**
   - Daily aged receivables summary
   - Payment collection rate trend
   - Customer payment history analysis

### French Business Context

- French payment terms awareness (30 days net, end of month)
- French late payment regulations (Taux d'intérêt légal)
- Appropriate tone for French business relationships
- Understanding of French company financial stress signals
- French invoicing standards (TVA, mentions légales)

### Technical Requirements

| Component | Specification |
|-----------|---------------|
| Accounting Integration | Pennylane, Indy, Cegid, or CSV import |
| Email Integration | SMTP with tracking |
| Reminder Templates | Customizable per client |
| Escalation Rules | Configurable per invoice amount |
| Reporting | Daily aged receivables export |

### Deployment Timeline

**Day 1-2:** Accounting system connection, invoice data import
**Day 3-4:** Reminder workflow configuration, template customization
**Day 5-7:** Test with historical overdue invoices
**Day 8-10:** Go-live with human oversight
**Day 11-14:** Monitor payment collection, fine-tune timing

### Pilot Success Metrics

| Metric | Baseline | Target | Measurement |
|--------|----------|--------|-------------|
| Days sales outstanding | 45 days | <35 days | Accounting report |
| Collection rate | 85% | >95% | Payment tracking |
| Invoice follow-up time | 2 hours/day | 15 minutes/day | Time tracking |
| Bad debt ratio | 5% | <2% | Write-off tracking |
| Customer satisfaction | N/A | >4.5/5 (no resentment) | Survey |

### First Working Agent Spec

```
Agent Name: Agent Relance Factures v1.0
Integration: Accounting software (Pennylane/CSV) + Email
Reminder Schedule: Day 7, 14, 30, 60 from due date
Tone: Polite but firm, culturally appropriate French
Escalation Triggers: >€10K overdue, >90 days overdue, customer dispute
Human Flags: Legal threats, payment plan requests, disputes
Reporting: Daily aged receivables summary to finance contact
Supervision: Finance reviews all escalations, weekly full audit
```

---

## Cross-Cutting MVP Infrastructure

### Shared Components Required

1. **French Language Model Routing**
   - Route French customer-facing tasks to Mistral/French-optimized model
   - Route complex reasoning to Claude
   - Cost optimization for routine French tasks

2. **Template Management System**
   - Pre-built prompt templates per agent type
   - Client-specific customization interface
   - Version control and A/B testing

3. **Human-in-the-Loop Interface**
   - Review queue for flagged items
   - One-click approval/rejection
   - Feedback capture for fine-tuning

4. **Analytics Dashboard**
   - Per-agent performance metrics
   - Client-specific dashboards
   - ROI calculation automation

---

## Pilot Design Framework

### Pilot Scoping Principles

1. **Single Department** — Choose one department with a clear, contained problem
2. **Measurable Baseline** — Quantify current state before agent deployment
3. **Limited Scope** — One agent type, one integration, one workflow
4. **Fast Feedback** — Daily check-ins during Week 1, weekly thereafter
5. **Clear Success Criteria** — Pre-agreed KPIs with measurement method

### Pilot Duration: 30 Days

| Week | Focus | Key Milestone |
|------|-------|---------------|
| Week 1 | Integration & Training | Agent reads emails, observes patterns |
| Week 2 | Supervised Operation | Agent responds with human review |
| Week 3 | Autonomous Operation | Agent handles routine cases alone |
| Week 4 | Full Autonomy & Measurement | Agent runs independently, results measured |

### Pilot Pricing

- **Fee:** €200 (credited toward Professional plan)
- **Commitment:** 4 hours/month of client time for check-ins
- **Success Guarantee:** If KPIs not met by Day 21, extend pilot 14 days at no cost

---

## Agent Customization Workflow

### Phase 1: Discovery (Week 1 of pilot prep)

1. **Process Documentation**
   - Map current workflow step-by-step
   - Identify pain points and bottlenecks
   - Document exception handling

2. **Data Collection**
   - Sample documents (tickets, leads, invoices)
   - Knowledge base content
   - Customer communication history

3. **Stakeholder Interviews**
   - End users who will interact with agent
   - Managers who will oversee agent
   - Champions who will advocate for agent

### Phase 2: Configuration (Week 2)

4. **Template Selection**
   - Choose base template (Customer Service / Lead Qualification / Invoice Follow-Up)
   - Customize prompt for client context
   - Add client-specific knowledge

5. **Integration Setup**
   - Connect email/CRM/accounting system
   - Test data flow
   - Verify authentication

6. **Workflow Configuration**
   - Define escalation rules
   - Set response timing
   - Configure routing logic

### Phase 3: Training (Week 3)

7. **Agent Training**
   - Upload client-specific documents
   - Fine-tune response patterns
   - Calibrate tone and style

8. **Testing & Validation**
   - Run test scenarios
   - Human review of responses
   - Refine based on feedback

### Phase 4: Go-Live (Week 4)

9. **Soft Launch**
   - Start with supervised operation
   - Daily review of agent outputs
   - Immediate feedback incorporation

10. **Full Launch**
    - Transition to autonomous operation
    - Weekly performance reviews
    - Continuous improvement

---

# Implementation Tasks

## Phase 1: Agent Type Research & Definition (Month 1)

### Customer Service Agent Research

#### Task: MVP-001
- **title**: Research French PME customer service pain points
- **description**: Conduct interviews with 5 PME owners to understand current customer service challenges: response times, common issues, outsourcing vs. in-house, tools used. Document findings in structured report.
- **inputs**: Interview guide, target company list, CRM of current state
- **outputs**: Research report with 10 key pain points ranked by frequency
- **dependencies**: [PV-021]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Pain points validated by >80% of interviewees

#### Task: MVP-002
- **title**: Analyze customer service agent best practices
- **description**: Research best-in-class customer service AI agents: Zendesk AI, Freshdesk Freddy, Intercom Fin. Document capabilities, pricing, integration patterns, and French language support.
- **inputs**: Competitor research, industry reports
- **outputs**: Best practices document with 15 techniques to adopt
- **dependencies**: []
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Document informs agent design decisions

#### Task: MVP-003
- **title**: Define customer service agent response quality rubric
- **description**: Create a quality scoring rubric for customer service responses: accuracy (0-5), tone (0-5), completeness (0-5), French fluency (0-5), actionability (0-5). Define minimum passing score.
- **inputs**: Industry quality standards, client expectations
- **outputs**: Quality rubric with scoring guidelines and examples
- **dependencies**: [MVP-001]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Rubric used for agent evaluation in all pilots

#### Task: MVP-004
- **title**: Design customer service agent escalation matrix
- **description**: Define escalation triggers for customer service agent: complaint keywords, refund requests, legal threats, technical errors, emotional distress. Map each trigger to human handler and response SLA.
- **inputs**: Support ticket analysis, client input
- **outputs**: Escalation matrix with 20+ triggers and handling procedures
- **dependencies**: [MVP-001]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Matrix reviewed by legal and operations

#### Task: MVP-005
- **title**: Create customer service agent knowledge base structure
- **description**: Design knowledge base architecture for customer service agent: document types, indexing strategy, update workflows, version control. Include French-specific formatting requirements.
- **inputs**: Knowledge management best practices, client FAQ examples
- **outputs**: Knowledge base schema and management guide
- **dependencies**: [MVP-001]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Knowledge base deployed in first pilot

#### Task: MVP-006
- **title**: Document customer service agent business hours policy
- **description**: Define French-appropriate business hours for customer service agent: working hours (8am-7pm), lunch break handling, weekend policy, French holiday calendar. Include response time SLAs per time period.
- **inputs**: French business norms, client preferences
- **outputs**: Business hours policy document with SLA matrix
- **dependencies**: []
- **priority**: medium
- **estimated_complexity**: low
- **agent_type**: product
- **validation**: Policy implemented in agent configuration

#### Task: MVP-007
- **title**: Build customer service agent tone guidelines
- **description**: Create detailed tone guidelines for French customer service: formal "vous" usage, greeting formulas, empathy expressions, closing formulas. Include 20+ examples of on-brand responses.
- **inputs**: French business communication standards, client brand voice
- **outputs**: Tone guidelines document with examples
- **dependencies**: [PV-008]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Tone consistent across all agent responses

#### Task: MVP-008
- **title**: Research French helpdesk and ticketing systems
- **description**: Research popular French helpdesk systems: Jira Service Management, Freshdesk, Zendesk, Support Panda. Document API capabilities, French language support, pricing tiers.
- **inputs**: Helpdesk vendor research, French market analysis
- **outputs**: Integration compatibility report for top 3 systems
- **dependencies**: []
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Top 2 integrations implemented by Month 3

### Lead Qualification Agent Research

#### Task: MVP-009
- **title**: Research lead qualification best practices for French B2B
- **description**: Research B2B lead qualification frameworks adapted for French market: MEDDIC, BANT, ANUM. Document French-specific nuances and cultural considerations in qualification conversations.
- **inputs**: Sales methodology research, French business culture research
- **outputs**: Qualification framework adapted for French B2B with 10 key questions
- **dependencies**: []
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Framework used in first 5 pilot qualifications

#### Task: MVP-010
- **title**: Define lead scoring model for French PME/ETI
- **description**: Design lead scoring model specific to Alizé ICP: budget fit (0-30), authority (0-25), need (0-25), timeline (0-20). Define thresholds for hot/warm/cold leads per score range.
- **inputs**: ICP definition, sales team input, historical data
- **outputs**: Scoring model with point allocation and threshold definitions
- **dependencies**: [GTM-002]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: sales
- **validation**: Scoring accuracy validated by sales team

#### Task: MVP-011
- **title**: Research lead enrichment data sources for French companies
- **description**: Research data enrichment providers with French company data: Clearbit, LinkedIn Sales Navigator, Modèle R, Société. Document data accuracy, coverage, API capabilities, GDPR compliance.
- **inputs**: Data provider research, GDPR requirements
- **outputs**: Enrichment provider comparison with French data coverage analysis
- **dependencies**: []
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Primary enrichment provider integrated by Month 2

#### Task: MVP-012
- **title**: Create lead routing workflow for sales team
- **description**: Design lead routing logic: territory-based routing, industry specialist routing, workload balancing. Define rules for round-robin, highest-rep-capacity, and territory-based routing.
- **inputs**: Sales team structure, territory map, capacity data
- **outputs**: Routing workflow document with decision tree
- **dependencies**: [MVP-010]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Routing implemented in CRM automation

#### Task: MVP-013
- **title**: Define lead qualification agent follow-up templates
- **description**: Create 10 follow-up email templates for lead qualification: initial acknowledgment, additional questions, meeting proposal, nurture sequence, disqualification message. All in French with personalization tokens.
- **inputs**: ICP personas, French business email conventions
- **outputs**: 10 email templates with subject lines and timing recommendations
- **dependencies**: [GTM-006]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Templates used in first pilot lead follow-up

#### Task: MVP-014
- **title**: Build lead quality prediction model
- **description**: Train model to predict which leads will convert to meetings based on: company size, industry, source, enrichment data completeness, response speed. Use historical data from sales team.
- **inputs**: Historical lead data, conversion outcomes
- **outputs**: Prediction model with >70% accuracy on historical data
- **dependencies**: [MVP-011]
- **priority**: medium
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Model accuracy validated against held-out test set

### Invoice Follow-Up Agent Research

#### Task: MVP-015
- **title**: Research French invoice payment norms and late payment regulations
- **description**: Research French late payment regulations: legal interest rate, penalty provisions, invoice payment terms (30 days net). Document French business norms for payment follow-up etiquette.
- **inputs**: French commercial law, business customs
- **outputs**: Regulatory compliance guide and culturally appropriate follow-up tone
- **dependencies**: []
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Agent follows all regulatory requirements

#### Task: MVP-016
- **title**: Define invoice aging categories and escalation triggers
- **description**: Define invoice aging categories: current (0-7 days), overdue-1 (8-14 days), overdue-2 (15-30 days), overdue-3 (31-60 days), write-off (>60 days). Map escalation triggers per category and amount threshold.
- **inputs**: Accounting best practices, client input
- **outputs**: Aging matrix with escalation procedures per category
- **dependencies**: [MVP-015]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Matrix approved by finance team for first pilot

#### Task: MVP-017
- **title**: Research French accounting software integrations
- **description**: Research integration capabilities for French accounting software: Pennylane, Indy, Cegid, Sage, FiBox. Document API availability, data export formats, webhook support.
- **inputs**: Accounting software vendor research
- **outputs**: Integration compatibility report for top 5 systems
- **dependencies**: []
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Top 3 integrations implemented by Month 3

#### Task: MVP-018
- **title**: Create invoice follow-up email template library
- **description**: Create 8 invoice follow-up templates in French: friendly reminder (Day 7), firm reminder (Day 14), urgent notice (Day 30), final warning (Day 60), payment confirmation, payment plan offer, dispute acknowledgment, collection agency notice. Each with appropriate tone escalation.
- **inputs**: French business correspondence standards, client brand voice
- **outputs**: 8 templates with timing rules and tone guidelines
- **dependencies**: [MVP-015]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Templates used in first pilot

#### Task: MVP-019
- **title**: Design payment plan negotiation workflow
- **description**: Design workflow for handling customer requests for payment plans: evaluation criteria (amount, duration, customer history), approval thresholds, contract terms, monitoring requirements.
- **inputs**: Finance team input, risk management guidelines
- **outputs**: Payment plan workflow with decision tree and approval matrix
- **dependencies**: [MVP-018]
- **priority**: medium
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Workflow approved by finance and legal

#### Task: MVP-020
- **title**: Build invoice follow-up agent cash flow reporting
- **description**: Design daily cash flow reporting for invoice follow-up agent: aged receivables summary, collection rate trend, daily collection forecast, customer payment pattern analysis.
- **inputs**: Finance team reporting requirements
- **outputs**: Daily report template and automation specification
- **dependencies**: [MVP-016]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Report generated and reviewed daily in pilot

---

## Phase 2: Agent Template Development (Month 1-2)

### Customer Service Agent Templates

#### Task: MVP-021
- **title**: Create customer service agent base prompt template
- **description**: Build the foundational prompt template for customer service agent: role definition, capabilities, tone guidelines, escalation rules, response structure. Include 5-shot examples for each response type.
- **inputs**: Quality rubric, tone guidelines, escalation matrix
- **outputs**: Base prompt template with placeholders for client customization
- **dependencies**: [MVP-003, MVP-004, MVP-007]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Template produces quality responses in test scenarios

#### Task: MVP-022
- **title**: Build FAQ response prompt library (30 scenarios)
- **description**: Create 30 pre-built prompt templates for common FAQ scenarios: product information, pricing questions, scheduling, account changes, troubleshooting, refunds. Each with classification criteria and response template.
- **inputs**: FAQ analysis, common question patterns
- **outputs**: 30-scenario prompt library with response templates
- **dependencies**: [MVP-021]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Library covers >80% of incoming questions

#### Task: MVP-023
- **title**: Create complaint handling prompt templates (15 scenarios)
- **description**: Build 15 prompt templates for handling customer complaints: product defect, service failure, billing error, shipping issue, delivery damage, rude staff, website problem, delivery delay, wrong item, refund request, cancellation request, account access, data privacy, contract dispute, general dissatisfaction. Include de-escalation techniques.
- **inputs**: Complaint analysis, de-escalation best practices
- **outputs**: 15 complaint handling templates with empathy techniques
- **dependencies**: [MVP-021]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Complaints handled without escalation in test scenarios

#### Task: MVP-024
- **title**: Develop ticket classification prompt
- **description**: Create prompt for automatic ticket classification: categories (technical, billing, general, complaint, feedback), subcategories, urgency levels, department routing. Include classification confidence threshold.
- **inputs**: Classification requirements, existing ticket data
- **outputs**: Classification prompt with accuracy target >90%
- **dependencies**: [MVP-021]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Classification accuracy >90% on test set

#### Task: MVP-025
- **title**: Build knowledge base retrieval prompt
- **description**: Create prompt for knowledge base retrieval and answer synthesis: query interpretation, document search, answer extraction, source citation, confidence scoring. Include handling for no-match scenarios.
- **inputs**: Knowledge base structure, retrieval requirements
- **outputs**: Retrieval prompt with citation and confidence handling
- **dependencies**: [MVP-005]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Retrieval accuracy >85% on test queries

#### Task: MVP-026
- **title**: Create customer service agent monitoring prompts
- **description**: Build prompts for ongoing agent monitoring: daily performance summary, weekly trend analysis, anomaly detection, customer feedback synthesis, improvement recommendations.
- **inputs**: Quality rubric, monitoring requirements
- **outputs**: Monitoring prompts for daily and weekly reporting
- **dependencies**: [MVP-003]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Monitoring reports generated automatically

### Lead Qualification Agent Templates

#### Task: MVP-027
- **title**: Create lead qualification interview prompt
- **description**: Build prompt for conducting lead qualification "interview" via email: opening, discovery questions, qualification criteria, next step proposal, closing. Follow BANT/ MEDDIC frameworks adapted for French context.
- **inputs**: Qualification framework, French conversation norms
- **outputs**: Qualification interview prompt with question sequence
- **dependencies**: [MVP-009, MVP-010]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: sales
- **validation**: Qualified leads match sales team judgment >80%

#### Task: MVP-028
- **title**: Build lead enrichment prompt library
- **description**: Create prompts for lead data enrichment: company information lookup, contact verification, social profile analysis, news search, technology stack detection. Include data validation rules.
- **inputs**: Enrichment data sources, data quality requirements
- **outputs**: Enrichment prompt library with validation logic
- **dependencies**: [MVP-011]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Enrichment coverage >80% for French companies

#### Task: MVP-029
- **title**: Develop lead scoring calculation prompt
- **description**: Create prompt for calculating lead score from enrichment data: point allocation logic, threshold determination, score explanation generation, confidence level. Include score update triggers.
- **inputs**: Scoring model, enrichment data structure
- **outputs**: Scoring prompt with explanation generation
- **dependencies**: [MVP-010]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Score correlation with conversion >0.7

#### Task: MVP-030
- **title**: Build lead routing decision prompt
- **description**: Create prompt for lead routing decisions: territory matching, rep capacity check, specialist matching, workload balancing. Include override conditions and manual handoff triggers.
- **inputs**: Routing workflow, rep capacity data
- **outputs**: Routing prompt with decision logic
- **dependencies**: [MVP-012]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Routing decisions match manual assignment >90%

#### Task: MVP-031
- **title**: Create lead nurture sequence prompt
- **description**: Build prompt for generating lead nurture content: content selection, personalization, send timing, follow-up triggers. Include 5-email nurture sequence for warm leads.
- **inputs**: Content library, nurture best practices
- **outputs**: Nurture sequence prompt with content selection logic
- **dependencies**: [MVP-013]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Nurture engagement rate >15%

#### Task: MVP-032
- **title**: Develop meeting booking confirmation prompt
- **description**: Create prompt for meeting booking confirmation: calendar availability check, meeting proposal, confirmation handling, reminder sequence. Include cancellation and reschedule logic.
- **inputs**: Calendar integration, meeting booking best practices
- **outputs**: Booking confirmation prompt with full workflow
- **dependencies**: [MVP-027]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Booking confirmation rate >80%

### Invoice Follow-Up Agent Templates

#### Task: MVP-033
- **title**: Create invoice status check prompt
- **description**: Build prompt for checking invoice payment status: aging calculation, payment matching, status update logic, flags for unusual patterns. Connect to accounting data structure.
- **inputs**: Accounting data schema, aging rules
- **outputs**: Status check prompt with aging logic
- **dependencies**: [MVP-016]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Status accuracy >99% vs. accounting system

#### Task: MVP-034
- **title**: Build reminder timing optimization prompt
- **description**: Create prompt for optimizing reminder timing: customer payment history, preferred contact time, historical response rates, optimal send time calculation. Include learning mechanism.
- **inputs**: Payment history data, email engagement data
- **outputs**: Timing optimization prompt with A/B testing support
- **dependencies**: [MVP-018]
- **priority**: medium
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Response rate improved >20% vs. fixed timing

#### Task: MVP-035
- **title**: Develop payment link generation prompt
- **description**: Create prompt for generating payment links: payment amount calculation, link generation, email embedding, confirmation tracking. Include partial payment handling.
- **inputs**: Payment gateway API, invoice data
- **outputs**: Payment link prompt with gateway integration
- **dependencies**: [MVP-018]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Payment link generation success rate >95%

#### Task: MVP-036
- **title**: Build dispute handling prompt
- **description**: Create prompt for handling invoice disputes: dispute acknowledgment, evidence collection, escalation to human, status tracking. Include dispute resolution timeline.
- **inputs**: Dispute handling procedures, customer communication standards
- **outputs**: Dispute handling prompt with escalation workflow
- **dependencies**: [MVP-019]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Disputes resolved within SLA >90%

#### Task: MVP-037
- **title**: Create collection escalation prompt
- **description**: Build prompt for collection escalation decisions: amount thresholds, aging thresholds, customer history, dispute status. Include human handoff preparation and documentation.
- **inputs**: Escalation matrix, collection procedures
- **outputs**: Escalation prompt with full documentation generation
- **dependencies**: [MVP-016, MVP-019]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Escalation decisions match manual review >95%

#### Task: MVP-038
- **title**: Develop aged receivables summary prompt
- **description**: Create prompt for generating daily aged receivables summary: invoice groupings, aging bands, amount totals, trend comparison, customer-level drill-down, action recommendations.
- **inputs**: Reporting requirements, finance team needs
- **outputs**: Daily summary prompt with formatting
- **dependencies**: [MVP-020]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Summary reviewed daily by finance in pilot

---

## Phase 3: Pilot Design & Scoping (Month 2)

### Pilot Framework Development

#### Task: MVP-039
- **title**: Design pilot evaluation criteria framework
- **description**: Create standardized pilot evaluation framework: pre-pilot baseline metrics, weekly check-in points, Week 4 success criteria, ROI calculation methodology. Make it client-agnostic and reusable.
- **inputs**: Client expectations, ROI calculation best practices
- **outputs**: Evaluation framework with templates and calculation sheets
- **dependencies**: [PV-042]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: ops
- **validation**: Framework used in all pilot evaluations

#### Task: MVP-040
- **title**: Create pilot kickoff deck template
- **description**: Build pilot kickoff presentation: company intro, pilot scope, timeline, success metrics, roles and responsibilities, communication plan, escalation contacts. 15 slides.
- **inputs**: Sales presentation template, pilot program design
- **outputs**: Kickoff deck template ready for client customization
- **dependencies**: [GTM-034]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Deck used in first 3 pilot kickoffs

#### Task: MVP-041
- **title**: Design pilot weekly check-in format
- **description**: Create weekly check-in structure: agenda template, metrics review, blockers identification, feedback collection, next week planning. 30-minute call format.
- **inputs**: Client management best practices, pilot timeline
- **outputs**: Check-in template with 10-point agenda
- **dependencies**: [MVP-039]
- **priority**: high
- **estimated_complexity**: low
- **agent_type**: ops
- **validation**: Check-ins conducted on schedule for all pilots

#### Task: MVP-042
- **title**: Create pilot success report template
- **description**: Build pilot final report: executive summary, baseline vs. results comparison, ROI calculation, lessons learned, retainer proposal. Include French and English versions.
- **inputs**: Evaluation framework, case study template
- **outputs**: Report template with data visualization placeholders
- **dependencies**: [MVP-039, PV-017]
- **priority**: high
- **estimated_complexity**:medium
- **agent_type**: ops
- **validation**: Report presented to first 3 pilot clients

#### Task: MVP-043
- **title**: Build pilot ROI calculation spreadsheet
- **description**: Create Excel/Google Sheets template for pilot ROI calculation: baseline hours, agent hours, cost savings, productivity gains, ROI percentage. Pre-fill formulas and charts.
- **inputs**: Evaluation framework, client finance data
- **outputs**: ROI calculation template with charts and graphs
- **dependencies**: [MVP-039]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Spreadsheet used in first 3 pilot calculations

#### Task: MVP-044
- **title**: Create pilot termination criteria document
- **description**: Define clear criteria for early pilot termination: performance below threshold, client non-engagement, technical blockers, contractual issues. Include offboarding process.
- **inputs**: Legal requirements, risk management
- **outputs**: Termination criteria document with checklist
- **dependencies**: [MVP-039]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Document reviewed by legal counsel

#### Task: MVP-045
- **title**: Design retainer transition checklist
- **description**: Create checklist for transitioning successful pilot to retainer: contract finalization, agent scaling, additional integrations, team training, ongoing SLA definition.
- **inputs**: Pilot results, retainer pricing tiers
- **outputs**: Transition checklist with 30+ items
- **dependencies**: [MVP-042]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Checklist used for first 3 retainer transitions

### Pilot Candidate Assessment

#### Task: MVP-046
- **title**: Develop pilot candidate scoring rubric
- **description**: Create rubric for evaluating pilot candidates: company size fit, problem clarity, decision-maker engagement, technical readiness, budget availability. Score 1-5 per criterion.
- **inputs**: ICP definition, pilot success factors
- **outputs**: Scoring rubric with threshold for pilot acceptance
- **dependencies**: [GTM-002]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Rubric used to evaluate all pilot candidates

#### Task: MVP-047
- **title**: Create pilot application form
- **description**: Design pilot application form for prospects: company info, current challenge, expected outcomes, technical environment, decision-maker commitment. 10 questions max.
- **inputs**: Pilot scope, qualification criteria
- **outputs**: Application form in French with submission flow
- **dependencies**: [MVP-046]
- **priority**: medium
- **estimated_complexity**: low
- **agent_type**: sales
- **validation**: Form used for first 10 pilot applications

#### Task: MVP-048
- **title**: Build pilot capacity planning model
- **description**: Create model for planning pilot capacity: max concurrent pilots, resource requirements per pilot, seasonal variations, team capacity growth. Track utilization rates.
- **inputs**: Team capacity data, pilot complexity estimates
- **outputs**: Capacity model with monthly utilization targets
- **dependencies**: []
- **priority**: medium
- **estimated_complexity**: high
- **agent_type**: ops
- **validation**: Model accurate within 10% for 3 months

### Pilot Technical Setup

#### Task: MVP-049
- **title**: Create shared pilot infrastructure template
- **description**: Design reusable infrastructure for pilots: tenant provisioning script, template database schema, standard integrations, monitoring dashboards. Enable rapid pilot setup.
- **inputs**: Infrastructure requirements, multi-tenant design
- **outputs**: Infrastructure-as-code templates for pilot provisioning
- **dependencies**: [TA-013]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: New pilot provisioned in <4 hours

#### Task: MVP-050
- **title**: Build agent template versioning system
- **description**: Create versioning system for agent templates: version numbers, changelog, rollback capability, A/B testing framework, template marketplace structure.
- **inputs**: Template library, version control best practices
- **outputs**: Versioning system with API and UI
- **dependencies**: [MVP-021, MVP-027, MVP-033]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Templates versioned and rollback tested

#### Task: MVP-051
- **title**: Create pilot data isolation testing procedure
- **description**: Design testing procedure to verify data isolation between pilots: cross-tenant access attempts, data leak detection, audit log verification. Automate with CI/CD.
- **inputs**: Multi-tenant security requirements
- **outputs**: Test suite with >20 test cases
- **dependencies**: [MVP-049]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: All tests pass, no data leaks detected

#### Task: MVP-052
- **title**: Build pilot monitoring dashboard template
- **description**: Create Grafana/BI dashboard template for pilot monitoring: agent performance metrics, task completion rates, error rates, customer satisfaction, cost per task.
- **inputs**: Monitoring requirements, pilot success metrics
- **outputs**: Dashboard template deployable for new pilots
- **dependencies**: [MVP-039]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Dashboard deployed within 24 hours of pilot start

#### Task: MVP-053
- **title**: Create pilot integration testing checklist
- **description**: Build checklist for testing agent integrations: email connectivity, CRM sync, accounting data flow, webhook delivery, error handling. Include test data sets.
- **inputs**: Integration requirements per agent type
- **outputs**: Integration checklist with test scripts
- **dependencies**: [MVP-049]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: All integrations tested before pilot go-live

---

## Phase 4: First Pilot Execution (Month 2-3)

### Pilot #1: Customer Service Agent

#### Task: MVP-054
- **title**: Onboard Pilot #1 customer service client
- **description**: Execute pilot onboarding for first customer service agent client: discovery call, contract signing, technical integration, knowledge base upload, agent configuration.
- **inputs**: Pilot candidate, scope definition
- **outputs**: Signed pilot contract, technical integration complete
- **dependencies**: [MVP-039, MVP-040, MVP-049]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: sales
- **validation**: Agent responding to tickets within 48 hours

#### Task: MVP-055
- **title**: Configure customer service agent for Pilot #1
- **description**: Customize customer service agent base template for Pilot #1: upload company FAQ, configure escalation rules, set business hours, calibrate tone, test with sample tickets.
- **inputs**: Client knowledge base, escalation matrix
- **outputs**: Configured agent ready for supervised testing
- **dependencies**: [MVP-054, MVP-021]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Agent passes quality rubric evaluation

#### Task: MVP-056
- **title**: Conduct Week 1 supervised operation for Pilot #1
- **description**: Run customer service agent in supervised mode for Week 1: review every response, provide immediate feedback, log issues, adjust prompts in real-time.
- **inputs**: Agent outputs, quality rubric
- **outputs**: Week 1 supervision log with adjustments made
- **dependencies**: [MVP-055]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Agent accuracy >80% by end of Week 1

#### Task: MVP-057
- **title**: Measure Week 2 autonomous operation for Pilot #1
- **description**: Transition customer service agent to autonomous operation for Week 2: reduce human review to 10% sample, track escalation rate, measure response time, collect customer feedback.
- **inputs**: Week 2 metrics, customer feedback
- **outputs**: Week 2 performance report with trends
- **dependencies**: [MVP-056]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Escalation rate <15%, response time <5 minutes

#### Task: MVP-058
- **title**: Document Pilot #1 lessons learned
- **description**: Capture lessons learned from first customer service pilot: what worked, what failed, prompt adjustments made, integration issues, client feedback themes.
- **inputs**: Pilot execution notes, metrics
- **outputs**: Lessons learned document with 15+ actionable insights
- **dependencies**: [MVP-057]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Insights applied to Pilot #2

#### Task: MVP-059
- **title**: Calculate Pilot #1 ROI and results
- **description**: Calculate ROI for first customer service pilot: hours saved, tickets resolved, response time improvement, customer satisfaction. Compare to baseline.
- **inputs**: Pilot metrics, baseline data
- **outputs**: ROI calculation with >30% productivity gain documented
- **dependencies**: [MVP-057, MVP-043]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: ROI >30% validated by client sign-off

#### Task: MVP-060
- **title**: Execute Pilot #1 retainer conversion
- **description**: Convert successful Pilot #1 to retainer contract: present ROI report, propose retainer tier, negotiate terms, finalize contract, plan scaling.
- **inputs**: Pilot results, ROI calculation
- **outputs**: Signed retainer contract
- **dependencies**: [MVP-059]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Retainer signed within 1 week of pilot end

### Pilot #2: Lead Qualification Agent

#### Task: MVP-061
- **title**: Onboard Pilot #2 lead qualification client
- **description**: Execute pilot onboarding for first lead qualification agent client: discovery call, contract signing, CRM integration, scoring model calibration, enrichment setup.
- **inputs**: Pilot candidate, scope definition
- **outputs**: Signed pilot contract, CRM integration complete
- **dependencies**: [MVP-039, MVP-040, MVP-049]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: sales
- **validation**: Agent processing leads within 48 hours

#### Task: MVP-062
- **title**: Configure lead qualification agent for Pilot #2
- **description**: Customize lead qualification agent for Pilot #2: configure scoring model weights, set routing rules, upload enrichment data sources, calibrate qualification questions.
- **inputs**: Client CRM data, scoring model
- **outputs**: Configured agent with calibrated scoring
- **dependencies**: [MVP-061, MVP-027]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: sales
- **validation**: Scoring accuracy >80% by end of calibration

#### Task: MVP-063
- **title**: Conduct calibration testing for Pilot #2
- **description**: Test lead qualification agent with historical leads: run 50 past leads through agent, compare to sales team judgment, adjust scoring weights, validate routing accuracy.
- **inputs**: Historical lead data, sales team input
- **outputs**: Calibration report with accuracy metrics
- **dependencies**: [MVP-062]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Accuracy >85% on calibration set

#### Task: MVP-064
- **title**: Monitor Week 1 live operation for Pilot #2
- **description**: Monitor lead qualification agent with live leads for Week 1: track scoring distribution, measure response time, audit routing decisions, collect sales rep feedback.
- **inputs**: Live lead data, sales feedback
- **outputs**: Week 1 monitoring report with adjustments
- **dependencies**: [MVP-063]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Meeting conversion rate >35% for qualified leads

#### Task: MVP-065
- **title**: Document Pilot #2 lessons learned
- **description**: Capture lessons learned from first lead qualification pilot: scoring adjustments, routing improvements, enrichment gaps, sales team adoption issues.
- **inputs**: Pilot execution notes, metrics
- **outputs**: Lessons learned document with 15+ actionable insights
- **dependencies**: [MVP-064]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Insights applied to template improvements

#### Task: MVP-066
- **title**: Calculate Pilot #2 ROI and results
- **description**: Calculate ROI for lead qualification pilot: hours saved on qualification, meeting conversion improvement, lead-to-revenue impact, cost per qualified lead.
- **inputs**: Pilot metrics, baseline data
- **outputs**: ROI calculation with >25% productivity gain documented
- **dependencies**: [MVP-064, MVP-043]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: ROI >25% validated by sales team

#### Task: MVP-067
- **title**: Execute Pilot #2 retainer conversion
- **description**: Convert successful Pilot #2 to retainer contract: present ROI report, propose retainer tier, negotiate terms, finalize contract.
- **inputs**: Pilot results, ROI calculation
- **outputs**: Signed retainer contract
- **dependencies**: [MVP-066]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Retainer signed within 1 week of pilot end

### Pilot #3: Invoice Follow-Up Agent

#### Task: MVP-068
- **title**: Onboard Pilot #3 invoice follow-up client
- **description**: Execute pilot onboarding for first invoice follow-up agent client: discovery call, contract signing, accounting integration, reminder template customization, aging rules configuration.
- **inputs**: Pilot candidate, scope definition
- **outputs**: Signed pilot contract, accounting integration complete
- **dependencies**: [MVP-039, MVP-040, MVP-049]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: ops
- **validation**: Agent monitoring invoices within 48 hours

#### Task: MVP-069
- **title**: Configure invoice follow-up agent for Pilot #3
- **description**: Customize invoice follow-up agent for Pilot #3: connect accounting system, configure aging thresholds, customize reminder templates, set escalation rules.
- **inputs**: Client accounting data, escalation matrix
- **outputs**: Configured agent with active monitoring
- **dependencies**: [MVP-068, MVP-033]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Agent sending reminders correctly by Day 3

#### Task: MVP-070
- **title**: Validate invoice aging calculation for Pilot #3
- **description**: Validate invoice aging calculation accuracy: run agent on 100 historical invoices, compare aging status to accounting system, verify day calculations.
- **inputs**: Historical invoice data, accounting system
- **outputs**: Validation report with accuracy metrics
- **dependencies**: [MVP-069]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Aging accuracy >99% vs. accounting system

#### Task: MVP-071
- **title**: Monitor Week 1 operation for Pilot #3
- **description**: Monitor invoice follow-up agent for Week 1: track reminder delivery, measure response rates, audit escalation decisions, review finance team feedback.
- **inputs**: Week 1 metrics, finance feedback
- **outputs**: Week 1 monitoring report with adjustments
- **dependencies**: [MVP-070]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Collection rate improvement >10% by Week 1

#### Task: MVP-072
- **title**: Document Pilot #3 lessons learned
- **description**: Capture lessons learned from first invoice follow-up pilot: template adjustments, timing optimizations, escalation improvements, customer response patterns.
- **inputs**: Pilot execution notes, metrics
- **outputs**: Lessons learned document with 15+ actionable insights
- **dependencies**: [MVP-071]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Insights applied to template improvements

#### Task: MVP-073
- **title**: Calculate Pilot #3 ROI and results
- **description**: Calculate ROI for invoice follow-up pilot: hours saved on collections, collection rate improvement, bad debt reduction, days sales outstanding improvement.
- **inputs**: Pilot metrics, baseline data
- **outputs**: ROI calculation with >20% collection improvement documented
- **dependencies**: [MVP-071, MVP-043]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: ROI >20% validated by finance team

#### Task: MVP-074
- **title**: Execute Pilot #3 retainer conversion
- **description**: Convert successful Pilot #3 to retainer contract: present ROI report, propose retainer tier, negotiate terms, finalize contract.
- **inputs**: Pilot results, ROI calculation
- **outputs**: Signed retainer contract
- **dependencies**: [MVP-073]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Retainer signed within 1 week of pilot end

---

## Phase 5: Template Refinement (Month 3-4)

### Template Optimization

#### Task: MVP-075
- **title**: Aggregate lessons learned across all pilots
- **description**: Consolidate lessons learned from all 3 pilots into unified improvement list: prompt refinements, integration fixes, workflow improvements, new edge cases.
- **inputs**: Lessons learned documents from MVP-058, MVP-065, MVP-072
- **outputs**: Prioritized improvement list with 30+ items
- **dependencies**: [MVP-058, MVP-065, MVP-072]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: All high-priority items addressed

#### Task: MVP-076
- **title**: Update customer service agent template based on pilots
- **description**: Refine customer service agent template with pilot learnings: improved escalation triggers, better FAQ retrieval, enhanced tone calibration, new response patterns.
- **inputs**: Lessons learned, quality rubric analysis
- **outputs**: Updated template with version bump
- **dependencies**: [MVP-075]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: New template passes test scenarios

#### Task: MVP-077
- **title**: Update lead qualification agent template based on pilots
- **description**: Refine lead qualification agent template with pilot learnings: adjusted scoring weights, improved routing logic, better enrichment prompts, new qualification questions.
- **inputs**: Lessons learned, accuracy analysis
- **outputs**: Updated template with version bump
- **dependencies**: [MVP-075]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: New template passes calibration test

#### Task: MVP-078
- **title**: Update invoice follow-up agent template based on pilots
- **description**: Refine invoice follow-up agent template with pilot learnings: optimized reminder timing, improved escalation thresholds, better dispute handling, new templates.
- **inputs**: Lessons learned, collection rate analysis
- **outputs**: Updated template with version bump
- **dependencies**: [MVP-075]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: New template passes integration test

#### Task: MVP-079
- **title**: Create agent template documentation
- **description**: Document all agent templates: usage instructions, customization guide, best practices, common issues, troubleshooting tips. Make it self-service for sales team.
- **inputs**: All agent templates
- **outputs**: Documentation portal with 50+ pages
- **dependencies**: [MVP-076, MVP-077, MVP-078]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: Sales team can configure agents without engineering help

#### Task: MVP-080
- **title**: Build agent template testing suite
- **description**: Create automated test suite for agent templates: 20 test cases per agent type, regression testing, performance benchmarks, quality scoring.
- **inputs**: Agent templates, quality rubric
- **outputs**: Test suite with >90% coverage
- **dependencies**: [MVP-076, MVP-077, MVP-078]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: All tests pass before template release

---

## Phase 6: Scale Preparation (Month 4-6)

### Scaling Infrastructure

#### Task: MVP-081
- **title**: Design pilot replication playbook
- **description**: Create playbook for rapid pilot replication: checklist of 50+ steps, template configurations, common pitfalls, success criteria. Enable non-expert execution.
- **inputs**: All pilot documentation, lessons learned
- **outputs**: Playbook with step-by-step instructions
- **dependencies**: [MVP-079]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: ops
- **validation**: New pilot launched by non-expert in <1 week

#### Task: MVP-082
- **title**: Build pilot launch automation
- **description**: Automate pilot launch process: tenant provisioning, template deployment, integration setup, monitoring dashboard deployment. Reduce launch time to <1 day.
- **inputs**: Replication playbook, infrastructure templates
- **outputs**: Automated launch scripts with approval gates
- **dependencies**: [MVP-049, MVP-081]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: New pilot launched in <4 hours

#### Task: MVP-083
- **title**: Create pilot success benchmarking database
- **description**: Build database of pilot success metrics across all clients: baseline metrics, achieved metrics, time to results, success factors, failure patterns.
- **inputs**: Pilot results, performance data
- **outputs**: Benchmarking database with 10+ pilot data points
- **dependencies**: [MVP-059, MVP-066, MVP-073]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Benchmarks used in pilot proposals

#### Task: MVP-084
- **title**: Develop new agent type roadmap
- **description**: Plan next wave of agent types based on client demand: HR onboarding agent, contract review agent, inventory management agent, social media agent. Prioritize by demand.
- **inputs**: Client feedback, market research
- **outputs**: Agent type roadmap with 5 candidates ranked
- **dependencies**: []
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Roadmap approved by leadership

#### Task: MVP-085
- **title**: Build agent type selection wizard
- **description**: Create decision tool for prospects: questionnaire about their challenges, recommended agent type(s), expected ROI range, pilot timeline. Help with agent type selection.
- **inputs**: Agent type specs, ROI data
- **outputs**: Interactive selection wizard (web or PDF)
- **dependencies**: [MVP-001, MVP-009, MVP-015]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: product
- **validation**: Wizard used by first 10 prospects

#### Task: MVP-086
- **title**: Create multi-agent deployment framework
- **description**: Design framework for deploying multiple agents to single client: agent coordination, shared knowledge base, cross-agent learning, unified reporting.
- **inputs**: Single-agent templates, client demand data
- **outputs**: Multi-agent architecture and deployment guide
- **dependencies**: [MVP-076, MVP-077, MVP-078]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: product
- **validation**: First multi-agent client onboarded successfully

#### Task: MVP-087
- **title**: Build client self-service configuration portal
- **description**: Create web portal for client self-service: knowledge base updates, response template edits, escalation rule changes, performance dashboard. Reduce dependency on ops team.
- **inputs**: Client requirements, agent templates
- **outputs**: Self-service portal with 20+ configuration options
- **dependencies**: [MVP-082]
- **priority**: medium
- **estimated_complexity**: high
- **agent_type**: frontend
- **validation**: Clients make 50%+ of changes without support

#### Task: MVP-088
- **title**: Create agent performance comparison report
- **description**: Build automated report comparing agent performance across clients: response time, accuracy, satisfaction, ROI. Enable benchmarking and improvement identification.
- **inputs**: Performance data from all agents
- **outputs**: Monthly comparison report with rankings
- **dependencies**: [MVP-052]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: ops
- **validation**: Report reviewed monthly by ops team

#### Task: MVP-089
- **title**: Develop agent fine-tuning pipeline documentation
- **description**: Document the process for fine-tuning agents on client data: data collection, annotation guidelines, training configuration, evaluation metrics, deployment.
- **inputs**: Fine-tuning best practices, client data
- **outputs**: Fine-tuning playbook with 30+ steps
- **dependencies**: [MVP-080]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Fine-tuning completed in <1 week per client

#### Task: MVP-090
- **title**: Create pilot-to-retainer conversion playbook
- **description**: Document the process for converting pilots to retainers: timing, presentation structure, negotiation tactics, objection handling, contract templates.
- **inputs**: Conversion experiences, sales playbooks
- **outputs**: Conversion playbook with scripts and templates
- **dependencies**: [MVP-060, MVP-067, MVP-074]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: sales
- **validation**: Conversion rate >80% using playbook
