# Alizé Technical Architecture Roadmap

**Version:** 1.0  
**Last Updated:** 2026-03-30  
**Status:** Foundation Document

---

## Executive Summary

This document outlines the technical architecture required to deliver Alizé's managed AI agent service. The architecture must support: multi-tenant isolation, MCP (Model Context Protocol) integration, AI orchestration across multiple models, French language optimization, EU data sovereignty, and enterprise-grade security—all while enabling rapid agent deployment and continuous improvement.

---

## Architecture Principles

1. **Security First** — Zero-trust architecture, encryption at rest and in transit, EU-only data residency
2. **Multi-Tenant by Design** — Shared infrastructure with logical isolation, no cross-tenant data leakage
3. **MCP Native** — All AI integrations use the Model Context Protocol for standardization
4. **Observability Complete** — Full logging, tracing, and metrics for every agent interaction
5. **French-Optimized** — Language models fine-tuned/routed for French business context
6. **Scalability Horizontal** — Stateless services that scale independently
7. **Compliance Built-In** — GDPR, ISO 27001, SOC 2 Type II compliance by design

---

## System Architecture Overview

### High-Level Components

```
┌─────────────────────────────────────────────────────────────┐
│                      Client Layer                           │
│   Web Dashboard (Nuxt) │ API Clients │ Integration APIs     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      API Gateway                            │
│   Auth │ Rate Limiting │ Tenant Routing │ Load Balancing   │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│  Agent        │   │  Orchestration │   │  Integration  │
│  Runtime      │   │  Engine        │   │  Layer        │
│  (MCP)        │   │                │   │  (MCP Tools)  │
└───────────────┘   └───────────────┘   └───────────────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    AI Model Gateway                         │
│   Claude │ GPT-4 │ Mistral │ Custom French Models          │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│  PostgreSQL   │   │  Redis        │   │  Storage      │
│  (Primary DB)│   │  (Cache/Queue)│   │  (S3 EU)       │
└───────────────┘   └───────────────┘   └───────────────┘
```

---

## Component Specifications

### 1. API Gateway

**Purpose:** Single entry point for all client traffic, handles authentication, rate limiting, tenant routing.

**Tech Stack:** Kong or NGINX with Lua plugins, JWT validation, Redis for session state.

**Key Requirements:**
- Handle 10,000 concurrent connections per region
- 99.9% uptime SLA
- Sub-50ms p99 latency for API calls
- Automatic tenant isolation at routing layer

### 2. Agent Runtime (MCP-Native)

**Purpose:** Execute AI agents using the Model Context Protocol standard.

**Tech Stack:** Node.js/Python MCP server, containerized agent environments, Kubernetes orchestration.

**Key Requirements:**
- Support 50+ concurrent agents per tenant
- Agent state persistence across sessions
- Sandboxed execution environment per agent
- Real-time streaming responses

### 3. Orchestration Engine

**Purpose:** Coordinate multi-step agent workflows, handle tool calls, manage context windows.

**Tech Stack:** Custom orchestration layer, temporal.io or Conductor for workflow, Redis for state.

**Key Requirements:**
- Handle 100+ step workflows
- Sub-second tool execution
- Workflow version control and A/B testing
- Automatic retry and error recovery

### 4. Integration Layer (MCP Tools)

**Purpose:** Connect agents to external systems via MCP tool interface.

**Tool Categories:**
- **Communication:** Email (IMAP/SMTP), Calendar, Slack, Teams
- **Business:** ERP connectors (SAP, Cegid), CRM (HubSpot, Salesforce), Accounting (Pennylane, Indy)
- **Document:** Google Drive, SharePoint, PDF parsing, OCR
- **Database:** PostgreSQL, MySQL direct queries
- **Custom:** Webhooks, REST API connectors

### 5. AI Model Gateway

**Purpose:** Unified interface to multiple AI providers with routing, fallback, and French optimization.

**Tech Stack:** Custom gateway service, model adapters, prompt management, response caching.

**Supported Models:**
- Anthropic Claude (primary for complex reasoning)
- OpenAI GPT-4 (fallback, specific use cases)
- Mistral (French-optimized, cost efficiency)
- Fine-tuned French models (future)

### 6. Data Layer

**PostgreSQL (Primary):**
- Multi-tenant schema with row-level security
- Connection pooling via PgBouncer
- Read replicas for reporting queries
- Point-in-time recovery enabled

**Redis:**
- Session management
- Agent context caching
- Rate limiting counters
- Workflow state machine

**Object Storage (S3 EU):**
- Document storage (GDPR-compliant regions only)
- Agent training data
- Audit logs
- Backup storage

---

## Multi-Tenant Architecture

### Isolation Strategy

1. **Database Level:** Row-level security with tenant_id column, enforced at PostgreSQL level
2. **Cache Level:** Redis keys prefixed with tenant_id, no shared state
3. **Compute Level:** Kubernetes namespaces per tenant or tenant groups
4. **Network Level:** VPC isolation, security groups, private networking
5. **Storage Level:** S3 bucket per tenant OR prefixed keys with IAM policies

### Tenant Onboarding Flow

```
1. Contract signed → Create tenant record in master DB
2. Provision tenant-specific resources (schema, S3 prefix, Redis keys)
3. Create tenant admin user with role-based permissions
4. Generate API keys with tenant scope
5. Configure tenant-specific integrations
6. Run onboarding workflow → assign initial agent templates
```

### Resource Allocation

| Plan | Concurrent Agents | Storage | API Calls/mo | Support |
|------|-------------------|---------|---------------|---------|
| Starter €500 | 3 | 10GB | 10,000 | Email |
| Professional €1000 | 10 | 50GB | 50,000 | Priority |
| Enterprise €2000 | 30 | 200GB | Unlimited | Dedicated |

---

## MCP Integration Specification

### MCP Overview

The Model Context Protocol (MCP) provides a standardized way for AI models to interact with tools and data sources. Alizé adopts MCP as its core integration framework.

### MCP Server Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    MCP Client (Agent Runtime)               │
└─────────────────────────────────────────────────────────────┘
                              │
                    MCP Protocol (JSON-RPC)
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│  MCP Server:  │   │  MCP Server:  │   │  MCP Server:  │
│  Communication│   │  Business Apps│   │  Document     │
│  (Email/Cal)  │   │  (ERP/CRM)    │   │  Processing   │
└───────────────┘   └───────────────┘   └───────────────┘
```

### MCP Tool Definitions

#### Email Tools
- `email_read(folder, limit, since)` — Read emails from inbox/sent/etc.
- `email_send(to, subject, body, attachments)` — Send emails
- `email_search(query, folder)` — Search emails
- `email_mark_as_read(message_id)` — Update read status

#### Calendar Tools
- `calendar_list_events(start_date, end_date)` — List calendar events
- `calendar_create_event(title, start, end, attendees, location)` — Create event
- `calendar_update_event(event_id, changes)` — Update event
- `calendar_delete_event(event_id)` — Delete event

#### CRM Tools
- `crm_get_contacts(limit, filters)` — Retrieve contacts
- `crm_create_contact(data)` — Create new contact
- `crm_update_contact(contact_id, data)` — Update contact
- `crm_search_contacts(query)` — Search contacts
- `crm_get_deals(stage)` — Retrieve deals/pipeline

#### Document Tools
- `document_upload(file, folder)` — Upload document
- `document_download(document_id)` — Download document
- `document_search(query)` — Full-text search
- `document_parse_pdf(file)` — Extract text from PDF
- `document_create_summary(document_id)` — Generate summary

#### Database Tools
- `db_query(sql, params)` — Execute read-only SQL query
- `db_insert(table, data)` — Insert record
- `db_update(table, id, data)` — Update record

### MCP Security Model

1. **Tool Permissions:** Each tenant configures which tools their agents can access
2. **OAuth Connections:** Per-tenant OAuth tokens for external services (never shared)
3. **Rate Limiting:** Per-tool call limits to prevent abuse
4. **Audit Logging:** Every tool call logged with tenant, user, timestamp, input hash
5. **Sandboxing:** Tool execution in isolated containers with network restrictions

---

## French Language Optimization

### Strategy

1. **Prompt Engineering:** All system prompts in French with French business context
2. **Model Routing:** Route French-specific tasks to Mistral or French-fine-tuned models
3. **French Fine-tuning:** Fine-tune models on French business correspondence corpus
4. **French Validation:** LLM-as-judge to verify French output quality

### French Business Corpus

Collect and use for training/fine-tuning:
- French commercial law documents
- French business correspondence examples
- French accounting standards (PCG)
- French HR documentation
- French customer service interactions

### Localization Requirements

- All agent responses in French unless client specifies otherwise
- Date/time formats: DD/MM/YYYY, 24-hour time
- Currency: Euro (€) with French formatting (1 234,56 €)
- French formal "vous" address in all communications
- Understanding of French business culture (meal breaks, vacation conventions, etc.)

---

## Security Architecture

### Zero-Trust Model

1. **Verify Explicitly:** Always authenticate and authorize based on all available data
2. **Least Privilege:** Just-in-time access, just-enough permissions
3. **Assume Breach:** Verify end-to-end encryption, log everything, monitor actively

### Authentication & Authorization

**Client Authentication:**
- API keys for programmatic access (rotatable, tenant-scoped)
- OAuth 2.0 for user-facing applications
- MFA enforced for admin accounts

**Internal Service Authentication:**
- mTLS between microservices
- Service accounts with short-lived tokens
- Kubernetes service account tokens

### Data Encryption

| Data State | Encryption | Key Management |
|------------|------------|-----------------|
| In Transit | TLS 1.3 | Let's Encrypt / managed certs |
| At Rest | AES-256 | AWS KMS / Vault |
| Backups | AES-256 | Separate KMS keys |
| API Keys | Hashed | HashiCorp Vault |

### EU Data Residency

**Guaranteed Regions:** 
- Primary: eu-west-3 (Paris, France)
- DR: eu-central-1 (Frankfurt, Germany)
- Backup: eu-west-1 (Ireland) — only for EU-to-EU replication

**Data Classification:**
- **Tier 1 (PII):** Names, emails, phone numbers — encrypted, EU-only
- **Tier 2 (Business Data):** Documents, transactions — encrypted, EU-only
- **Tier 3 (Logs):** Audit logs, metrics — encrypted, EU-only
- **Tier 4 (Analytics):** Aggregated, anonymized — can use EU analytics services

### Compliance Requirements

| Compliance | Status | Implementation |
|------------|--------|-----------------|
| GDPR | Required | Data processing agreements, right to deletion, EU residency |
| ISO 27001 | Target Q4 | Security controls, audit logging, access management |
| SOC 2 Type II | Target Q4 | Security, availability, confidentiality controls |
| French DORA | Required | Operational resilience for financial sector clients |

---

## Observability Stack

### Logging

**Stack:** ELK (Elasticsearch, Logstash, Kibana) or Loki + Grafana

**Requirements:**
- All logs in structured JSON format
- Logs retained 90 days hot, 1 year cold storage
- Cross-tenant log isolation
- Real-time log search and alerting

**Log Schema:**
```json
{
  "timestamp": "2026-03-30T10:00:00Z",
  "tenant_id": "uuid",
  "agent_id": "uuid",
  "user_id": "uuid",
  "action": "tool_call",
  "tool": "email_send",
  "request_id": "uuid",
  "latency_ms": 150,
  "status": "success|error",
  "metadata": {}
}
```

### Metrics

**Stack:** Prometheus + Grafana

**Key Metrics:**
- Agent execution latency (p50, p95, p99)
- Agent success/failure rate by type
- API request volume by tenant
- AI token consumption by model
- Error rates by endpoint
- Database connection pool utilization
- Cache hit rates

**Dashboards:**
- Executive: Revenue, MRR, churn, NPS
- Sales: Pipeline, conversion rates, CAC
- Operations: Uptime, incident count, response times
- Engineering: Error rates, latency, deployment frequency

### Tracing

**Stack:** Jaeger or Tempo (Grafana)

**Requirements:**
- Distributed tracing for all API calls
- Trace every agent execution flow
- Propagate request context across services
- Sample 100% of errors, 5% of successful traces

---

## Infrastructure Architecture

### Cloud Provider

**Primary:** AWS eu-west-3 (Paris) — France region, meets data residency

**Secondary:** AWS eu-central-1 (Frankfurt) — Germany region for DR

### Kubernetes Architecture

```
Cluster: alize-prod-eu-west-3
├── Namespace: ingress
│   └── NGINX Ingress Controller
├── Namespace: api-gateway
│   └── Kong API Gateway (3 replicas)
├── Namespace: agent-runtime
│   ├── Agent Runtime Service (5 replicas)
│   ├── Agent Worker Pods (HPA, 2-20 replicas)
│   └── MCP Server Pods (per tool category)
├── Namespace: orchestration
│   └── Orchestration Engine (3 replicas)
├── Namespace: ai-gateway
│   └── AI Model Gateway (3 replicas)
├── Namespace: data
│   ├── PostgreSQL (Primary + 2 Replicas)
│   ├── Redis Cluster (3 nodes)
│   └── MinIO/S3 (Object storage)
├── Namespace: monitoring
│   ├── Prometheus
│   ├── Grafana
│   └── Alertmanager
└── Namespace: security
    ├── Vault (Secrets)
    ├── Certificate Manager
    └── Policy Controller
```

### Deployment Strategy

1. **Blue-Green Deployments:** Zero-downtime deployments with instant rollback
2. **Canary Releases:** New agent features rolled to 5% of traffic first
3. **Infrastructure as Code:** Terraform for all cloud resources
4. **GitOps:** ArgoCD or Flux for Kubernetes deployments

---

# Implementation Tasks

## Phase 1: Core Infrastructure (Month 1-2)

### Cloud & Networking

#### Task: TA-001
- **title**: Set up AWS account structure for Alizé
- **description**: Configure AWS organization with master and member accounts: production, staging, development, security tooling. Set up SCPs and OU structure.
- **inputs**: AWS account credentials, org structure design
- **outputs**: Production AWS account with security baselines applied
- **dependencies**: []
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: All resources deployed in eu-west-3 region only

#### Task: TA-002
- **title**: Configure VPC with private subnets for production
- **description**: Design and deploy VPC architecture: public subnets for load balancers, private subnets for application tier, isolated subnets for databases. Configure NAT gateways and VPC endpoints.
- **inputs**: VPC architecture diagram, IP CIDR plan
- **outputs**: VPC with 3-tier subnet architecture, routing tables, security groups
- **dependencies**: [TA-001]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: All EC2/RDS resources launch in private subnets

#### Task: TA-003
- **title**: Set up Kubernetes cluster (EKS) in EU region
- **description**: Deploy EKS cluster in eu-west-3: managed node groups, cluster autoscaler, EBS CSI driver, VPC CNI plugin, CoreDNS. Configure cluster access via kubeconfig.
- **inputs**: EKS requirements, node sizing plan
- **outputs**: Running EKS cluster with 3 node groups (system, app, data)
- **dependencies**: [TA-002]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: kubectl commands work, cluster autoscaler responds to load

#### Task: TA-004
- **title**: Configure kubectl access and RBAC for team
- **description**: Set up kubectl access for engineering team with role-based access control: developer namespace access, ops full access, read-only for others. Integrate with GitHub SSO.
- **inputs**: Team member list, namespace design
- **outputs**: kubectl configs for 5 team members, RBAC policies applied
- **dependencies**: [TA-003]
- **priority**: high
- **estimated_complexity**: low
- **agent_type**: backend
- **validation**: Each team member can access their namespace only

#### Task: TA-005
- **title**: Set up Kubernetes namespaces for microservices
- **description**: Create namespaces: ingress, api-gateway, agent-runtime, orchestration, ai-gateway, data, monitoring, security. Apply resource quotas and limit ranges per namespace.
- **inputs**: Namespace design from architecture diagram
- **outputs**: All namespaces created with quotas applied
- **dependencies**: [TA-003]
- **priority**: high
- **estimated_complexity**: low
- **agent_type**: backend
- **validation**: Namespaces visible in kubectl, resource quotas enforced

#### Task: TA-006
- **title**: Configure ingress controller (NGINX)
- **description**: Deploy NGINX Ingress Controller in ingress namespace with TLS termination, rate limiting, and basic auth options. Configure external DNS for domain routing.
- **inputs**: Domain names, TLS certificate requirements
- **outputs**: Ingress controller running, test endpoint accessible via HTTPS
- **dependencies**: [TA-005]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: HTTPS routes work for test services

#### Task: TA-007
- **title**: Set up secrets management (HashiCorp Vault)
- **description**: Deploy HashiCorp Vault in HA mode in security namespace. Configure AWS KMS for unseal keys, enable Kubernetes auth method, create policies for each service.
- **inputs**: Vault architecture design, AWS KMS configuration
- **outputs**: Vault cluster running with dynamic secrets for databases
- **dependencies**: [TA-005]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Services can retrieve secrets from Vault, no plaintext secrets in repo

#### Task: TA-008
- **title**: Configure certificate management
- **description**: Set up cert-manager with Let's Encrypt ClusterIssuer for automatic TLS certificate provisioning. Configure certificate rotation.
- **inputs**: Domain names, email for Let's Encrypt
- **outputs**: Automatic TLS for all ingress routes, certificates auto-renew
- **dependencies**: [TA-006, TA-007]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Certificates provisioned automatically for new routes

#### Task: TA-009
- **title**: Set up S3 bucket with EU residency
- **description**: Create S3 buckets for document storage: alize-documents-{env}, alize-audit-logs-{env}, alize-backups-{env}. Enable versioning, encryption, and lifecycle policies. Force EU region.
- **inputs**: Bucket naming convention, retention policies
- **outputs**: S3 buckets with proper policies, tested uploads from application
- **dependencies**: [TA-001]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Files uploaded only to eu-west-3 region, encryption verified

#### Task: TA-010
- **title**: Configure AWS backup for RDS and S3
- **description**: Set up automated backup strategy: RDS daily snapshots with 30-day retention, S3 replication to Frankfurt, cross-region backup verification.
- **inputs**: RPO/RTO requirements, backup schedule
- **outputs**: Backup jobs running, restore tested quarterly
- **dependencies**: [TA-009]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Backup completes daily, restore tested successfully

---

## Phase 2: Database & Cache Layer (Month 2)

### Database Setup

#### Task: TA-011
- **title**: Deploy PostgreSQL cluster (RDS or self-managed)
- **description**: Deploy PostgreSQL 15 in high availability: primary + 2 standbys, automated failover, read replicas for reporting. Enable pgaudit for security logging.
- **inputs**: Database sizing, HA requirements
- **outputs**: PostgreSQL cluster with 99.9% uptime, automated failover tested
- **dependencies**: [TA-002]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Failover tested, pgaudit logs visible

#### Task: TA-012
- **title**: Configure connection pooling (PgBouncer)
- **description**: Deploy PgBouncer in front of PostgreSQL: transaction pooling mode, pool size configuration, tenant-based connection limits.
- **inputs**: Database connection requirements, pool sizing
- **outputs**: PgBouncer running, connection pooling verified under load
- **dependencies**: [TA-011]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: 1000 concurrent connections handled without exhaustion

#### Task: TA-013
- **title**: Design multi-tenant database schema
- **description**: Design PostgreSQL schema with row-level security: tenants table, tenant_id on all tables, RLS policies, indexes per query pattern.
- **inputs**: Data model requirements, query patterns
- **outputs**: Schema with RLS policies, tested tenant isolation
- **dependencies**: [TA-011]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Cross-tenant queries return zero results

#### Task: TA-014
- **title**: Create database migration workflow
- **description**: Set up database migration tooling (Flyway or Liquibase): migration scripts versioning, rollback support, migration pipeline in CI/CD.
- **inputs**: Schema design, CI/CD pipeline
- **outputs**: Migration workflow operational, tested on staging
- **dependencies**: [TA-013]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Migrations run automatically on deployment

#### Task: TA-015
- **title**: Configure Redis cluster for caching
- **description**: Deploy Redis Cluster (3 masters + 3 replicas) in data namespace. Configure eviction policies, persistence, and TLS.
- **inputs**: Redis architecture, memory requirements
- **outputs**: Redis cluster with 3 masters, automatic failover tested
- **dependencies**: [TA-005]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Redis cluster handles 10K ops/sec, failover tested

#### Task: TA-016
- **title**: Set up Redis for session management
- **description**: Configure Redis as session store: session key format, TTL policies, session validation endpoints. Integrate with authentication service.
- **inputs**: Session requirements, Redis config
- **outputs**: Sessions working, Redis keys prefixed by tenant_id
- **dependencies**: [TA-015]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Sessions persist across service restarts

#### Task: TA-017
- **title**: Configure Redis for rate limiting
- **description**: Implement rate limiting with Redis: sliding window algorithm, per-tenant and per-endpoint limits, quota tracking.
- **inputs**: Rate limit requirements per plan tier
- **outputs**: Rate limiting working, limits enforced correctly
- **dependencies**: [TA-015]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Rate limits trigger at configured thresholds

#### Task: TA-018
- **title**: Configure Redis for workflow state
- **description**: Set up Redis for orchestration workflow state: state machine keys, expiration policies, recovery mechanism for in-flight workflows.
- **inputs**: Workflow state requirements
- **outputs**: Workflow state persisted and recoverable
- **dependencies**: [TA-015]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Workflow state survives service restart

---

## Phase 3: API Gateway (Month 2)

### Gateway Configuration

#### Task: TA-019
- **title**: Deploy Kong API Gateway
- **description**: Deploy Kong Gateway (or NGINX with Kong) in api-gateway namespace: 3 replicas, PostgreSQL for config storage, TLS termination.
- **inputs**: Gateway sizing, plugin requirements
- **outputs**: Kong running, admin API accessible, test route working
- **dependencies**: [TA-005, TA-011]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Kong handles 1000 req/sec, admin API responsive

#### Task: TA-020
- **title**: Configure JWT authentication plugin
- **description**: Set up JWT validation in Kong: RS256 algorithm, tenant-scoped claims, token validation endpoint, claim mapping.
- **inputs**: JWT structure, tenant claims
- **outputs**: Kong validates JWTs, rejects invalid tokens
- **dependencies**: [TA-019]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Valid JWTs pass, expired/invalid JWTs rejected with 401

#### Task: TA-021
- **title**: Configure rate limiting per tenant
- **description**: Implement rate limiting plugins: per-tenant limits based on plan tier, endpoint-specific limits, Redis-backed counters.
- **inputs**: Rate limit table per plan
- **outputs**: Rate limits enforced, HTTP 429 returned when exceeded
- **dependencies**: [TA-017, TA-019]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Tenant at 1000 req/min gets 429 at 1001st request

#### Task: TA-022
- **title**: Configure tenant routing rules
- **description**: Set up Kong routes for multi-tenant routing: /api/v1/{tenant_id} pattern, tenant-specific upstream services, header-based routing options.
- **inputs**: Routing requirements, service endpoints
- **outputs**: Requests routed to correct tenant-scoped services
- **dependencies**: [TA-019]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Tenant ID extracted from URL, routed correctly

#### Task: TA-023
- **title**: Set up request transformation plugins
- **description**: Configure request/response transformations: add tenant_id header, remove sensitive headers, add request ID for tracing.
- **inputs**: Transformation requirements
- **outputs**: Requests transformed before hitting upstream
- **dependencies**: [TA-019]
- **priority**: medium
- **estimated_complexity**: low
- **agent_type**: backend
- **validation**: Headers added correctly, request IDs correlate logs

#### Task: TA-024
- **title**: Configure CORS for client applications
- **description**: Set up CORS plugin: allowed origins from whitelist, allowed methods, credential support, preflight caching.
- **inputs**: Client domain list
- **outputs**: CORS configured for web dashboard and mobile clients
- **dependencies**: [TA-019]
- **priority**: medium
- **estimated_complexity**: low
- **agent_type**: backend
- **validation**: Browser preflight requests succeed, credentials work

#### Task: TA-025
- **title**: Set up request logging plugin
- **description**: Configure Kong to log all requests: format matching observability schema, log masking for sensitive fields, forwarding to Loki/ELK.
- **inputs**: Log format specification
- **outputs**: All requests logged with required fields
- **dependencies**: [TA-019]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Logs appear in observability stack with correct format

#### Task: TA-026
- **title**: Configure circuit breaker for AI services
- **description**: Set up circuit breaker plugin: detect AI service failures, open circuit after 5 failures in 10 seconds, half-open after 30 seconds.
- **inputs**: Circuit breaker configuration
- **outputs**: Circuit breaker protecting AI gateway from cascade failures
- **dependencies**: [TA-019]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Circuit opens when AI service fails, recovers automatically

---

## Phase 4: Agent Runtime (Month 2-3)

### MCP Server Implementation

#### Task: TA-027
- **title**: Set up MCP server project structure
- **description**: Create Node.js project for MCP server: monorepo structure, TypeScript configuration, testing framework (Jest), CI/CD pipeline.
- **inputs**: Project structure requirements
- **outputs**: MCP server repo with build and test pipeline
- **dependencies**: []
- **priority**: high
- **estimated_complexity**: low
- **agent_type**: backend
- **validation**: Build passes, tests run in CI

#### Task: TA-028
- **title**: Implement MCP protocol handler
- **description**: Implement MCP JSON-RPC protocol handler: initialize, tools/list, tools/call, resources/* endpoints. Handle streaming responses.
- **inputs**: MCP protocol specification
- **outputs**: MCP server responding to protocol-compliant requests
- **dependencies**: [TA-027]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Passes MCP compliance tests

#### Task: TA-029
- **title**: Implement email MCP tools (read, send, search)
- **description**: Build email integration tools: IMAP/SMTP connection pooling, OAuth2 for Microsoft/Google, inbox/search operations.
- **inputs**: Email provider APIs (Microsoft Graph, Gmail API)
- **outputs**: email_read, email_send, email_search tools working
- **dependencies**: [TA-028]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Can read and send emails with OAuth credentials

#### Task: TA-030
- **title**: Implement calendar MCP tools
- **description**: Build calendar integration tools: OAuth2 calendar access, event CRUD operations, attendee management.
- **inputs**: Microsoft Graph API, Google Calendar API
- **outputs**: calendar_list_events, calendar_create_event tools working
- **dependencies**: [TA-028]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Can list and create calendar events

#### Task: TA-031
- **title**: Implement document MCP tools
- **description**: Build document processing tools: S3 upload/download, PDF parsing with OCR, text extraction, summary generation.
- **inputs**: S3 configuration, PDF parsing library
- **outputs**: document_upload, document_download, document_parse_pdf tools
- **dependencies**: [TA-009, TA-028]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: PDF text extraction accuracy >95%

#### Task: TA-032
- **title**: Implement CRM MCP tools (HubSpot focus)
- **description**: Build HubSpot integration tools: contact CRUD, deal management, search functionality, custom objects.
- **inputs**: HubSpot API documentation
- **outputs**: crm_get_contacts, crm_create_contact, crm_search tools
- **dependencies**: [TA-028]
- **priority**: medium
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Can CRUD contacts in HubSpot test account

#### Task: TA-033
- **title**: Implement database MCP tools
- **description**: Build database access tools: read-only SQL queries, parameterized queries, result formatting. Security: no DDL, query validation.
- **inputs**: Database schema, security requirements
- **outputs**: db_query tool with SQL injection prevention
- **dependencies**: [TA-013]
- **priority**: medium
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Queries execute, invalid SQL rejected, no data leakage

#### Task: TA-034
- **title**: Implement webhook MCP tools
- **description**: Build webhook integration tools: outbound webhook delivery with retry, signature verification, delivery status tracking.
- **inputs**: Webhook configuration requirements
- **outputs**: webhook_send tool with retry logic and status tracking
- **dependencies**: [TA-028]
- **priority**: medium
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Webhooks deliver with retry on failure

#### Task: TA-035
- **title**: Configure MCP server authentication
- **description**: Implement MCP server auth: per-tenant API keys, tool-level permissions, audit logging of all tool calls.
- **inputs**: Authentication requirements, permission matrix
- **outputs**: MCP tools validate permissions before execution
- **dependencies**: [TA-028]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Unauthorized tool calls rejected with 403

#### Task: TA-036
- **title**: Containerize MCP server
- **description**: Create Docker image for MCP server: multi-stage build, non-root user, health check, resource limits.
- **inputs**: Dockerfile best practices
- **outputs**: MCP server Docker image in registry
- **dependencies**: [TA-027]
- **priority**: high
- **estimated_complexity**: low
- **agent_type**: backend
- **validation**: Image builds, runs locally, passes health check

#### Task: TA-037
- **title**: Deploy MCP servers to Kubernetes
- **description**: Deploy MCP server pods: 3 replicas per tool category, HPA for scaling, PodDisruptionBudget for availability.
- **inputs**: MCP server images, scaling requirements
- **outputs**: MCP servers running in Kubernetes, accessible from agent runtime
- **dependencies**: [TA-036, TA-005]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: MCP servers respond to requests under load

#### Task: TA-038
- **title**: Implement tool call rate limiting
- **description**: Add rate limiting per tool: per-tenant limits, per-tool limits, Redis-backed counters. Return 429 when exceeded.
- **inputs**: Rate limit configuration
- **outputs**: Tool calls rate limited, 429 returned with retry-after header
- **dependencies**: [TA-017, TA-037]
- **priority**: high
- **estimated_complexity**: medium
- **agent_type**: backend
- **validation**: Rate limits trigger at configured thresholds

---

## Phase 5: AI Gateway (Month 3)

### Model Orchestration

#### Task: TA-039
- **title**: Design AI gateway architecture
- **description**: Create architecture for AI model gateway: multi-provider support (Anthropic, OpenAI, Mistral), request routing, fallback logic, cost tracking.
- **inputs**: Model requirements, provider pricing
- **outputs**: AI gateway architecture document
- **dependencies**: []
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Architecture approved by engineering lead

#### Task: TA-040
- **title**: Implement AI gateway service
- **description**: Build AI gateway service: provider abstraction layer, request/response handling, streaming support, error handling.
- **inputs**: AI gateway architecture
- **outputs**: AI gateway service handling requests to multiple providers
- **dependencies**: [TA-039]
- **priority**: high
- **estimated_complexity**: high
- **agent_type**: backend
- **validation**: Service handles requests to Claude and GPT-4

#### Task: TA-041
- **title**: Implement Anthropic Claude integration
- **description**: Integrate Anthropic Claude API: streaming responses, token counting, cost calculation, system prompt management.
- **inputs**: Anthropic API documentation
- **outputs**: Claude integration working with token tracking
