# Alizé — Final Assembled Architecture
**Saved:** 2026-03-25
**Source:** Louis (via Discord)

## Why Hono + Mastra (not Nuxt server routes or AdonisJS)

- **Nuxt server routes** can't hold WebSocket connections, no event bus, workers run in separate processes with no way to push events to connected clients. You'd need Redis pub/sub anyway.
- **AdonisJS** has great events (Emittery) and Transmit (SSE with Redis sync), but Mastra has no AdonisJS adapter — only Hono, Express, Fastify, Koa. You'd be fighting the framework. Lucid ORM also doesn't support pgvector.
- **Hono + Mastra** is the natural fit. Mastra runs on Hono internally. You embed Mastra into Hono with one line. Hono handles HTTP routing and SSE streaming.

## Monorepo Structure

```
alize/
├── apps/
│   ├── web/                    # Nuxt 3 — frontend only
│   │   ├── pages/
│   │   │   ├── index.vue       # Landing page
│   │   │   ├── login.vue
│   │   │   └── app/
│   │   │       ├── index.vue   # Dashboard
│   │   │       ├── chat.vue    # Chat interface
│   │   │       ├── documents.vue
│   │   │       ├── crons.vue
│   │   │       ├── skills.vue
│   │   │       └── settings.vue
│   │   ├── components/
│   │   │   ├── chat/
│   │   │   ├── documents/
│   │   │   └── crons/
│   │   ├── composables/
│   │   │   ├── useChat.ts      # SSE stream consumer
│   │   │   ├── useDocuments.ts
│   │   │   └── useAuth.ts
│   │   └── server/
│   │       └── api/[...proxy].ts  # Proxy to engine
│   │
│   └── engine/                 # Hono + Mastra + Workers
│       ├── src/
│       │   ├── index.ts        # Entry point
│       │   ├── server/
│       │   │   ├── app.ts      # Hono app
│       │   │   ├── routes/     # auth, chat, documents, crons, skills, webhooks
│       │   │   ├── middleware/ # auth, org context
│       │   │   └── sse.ts      # SSE broadcaster (Redis sub → client)
│       │   ├── agent/
│       │   │   ├── mastra.ts
│       │   │   ├── alize-agent.ts
│       │   │   ├── tools/       # search-docs, read-doc, write-doc, cron-tools, skill-tools, deep-research...
│       │   │   ├── skills/
│       │   │   │   ├── loader.ts
│       │   │   │   └── builtin/ # default skills (markdown)
│       │   │   └── memory.ts
│       │   ├── documents/       # Processing pipeline
│       │   │   ├── parser.ts
│       │   │   ├── chunker.ts
│       │   │   ├── embedder.ts
│       │   │   ├── vectorstore.ts  # pgvector
│       │   │   ├── metadata.ts
│       │   │   └── s3.ts       # OVHcloud S3
│       │   ├── channels/        # Channel adapters
│       │   │   ├── types.ts
│       │   │   ├── normalizer.ts
│       │   │   ├── formatter.ts
│       │   │   ├── slack.ts
│       │   │   ├── discord.ts
│       │   │   ├── teams.ts
│       │   │   ├── whatsapp.ts
│       │   │   └── email.ts
│       │   ├── workers/         # BullMQ jobs
│       │   │   ├── queues.ts
│       │   │   ├── doc-worker.ts
│       │   │   ├── cron-worker.ts
│       │   │   └── improvement-worker.ts
│       │   ├── events/          # Event system
│       │   │   ├── bus.ts      # Redis pub/sub
│       │   │   └── types.ts
│       │   └── db/
│       │       ├── schema.ts    # Drizzle schema
│       │       ├── migrate.ts
│       │       └── client.ts
│       └── drizzle/             # SQL migrations
│
├── packages/
│   └── shared/                  # Shared types
│       ├── types.ts
│       └── constants.ts
│
└── infra/
    ├── docker-compose.yml       # Local: pg, redis, minio
    ├── Dockerfile.web
    ├── Dockerfile.engine
    └── deploy/
        └── docker-compose.prod.yml
```

## Tech Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Frontend | Nuxt 3 | Request/response only, no SSR needed for app pages |
| Engine | Hono + Mastra | Natural fit, Mastra embeds in Hono |
| Agent runtime | Mastra | 22k stars, RAG/memory/workflows/MCP built-in |
| Vector DB | pgvector | PostgreSQL extension, OVHcloud hosted |
| File storage | OVHcloud S3 | French hosting, cheap |
| Background jobs | BullMQ | In-process with Redis, same Node process |
| Event bus | Redis pub/sub | Workers → SSE → clients |
| Real-time | SSE | Hono streaming, Redis-sub for cross-process |
| ORM | Drizzle | Lightweight, SQL-first, pgvector friendly |
| Auth | Better Auth | Organization plugin for multi-tenancy |
| Deploy | Docker Compose → K8s later | €25-40/month on OVHcloud VPS |

## Cost Estimate

- OVHcloud VPS (2 vCPU, 8GB RAM): ~€20-30/month
- PostgreSQL + Redis (self-hosted on same VPS): included
- Object Storage (S3): ~€5/month
- Mistral API: ~€200-400/month at scale
- **Total at 50 users:** ~€250-450/month
- **At 100 users × €19:** €1,900 MRR, ~€400 infra cost, ~79% gross margin

## Production Deploy

Two containers (web + engine) + two data services (postgres + redis).
Scale to Kubernetes when 50+ paying orgs.
