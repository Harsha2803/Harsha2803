## Cheella Sree Harsha

**Backend & AI Platform Engineer.** I work on the engineering *around* models — authorization,
validation, budgeting, observability, failure recovery — rather than on the models themselves.

Two years of building secure enterprise systems in Python and FastAPI at **JK Tech**: pluggable
identity, validated NL2SQL workflows, governed MCP tool integrations, multi-cloud service
abstractions, and event-driven content ingestion.

The through-line is treating an LLM as an **unreliable component**: every model output gets
validated, authorized, budgeted, traced, or human-gated before it can do anything.

[LinkedIn](https://www.linkedin.com/in/cheellasreeharsha/) · cheellasreeharsha2803@gmail.com

---

### What I build at work

On an enterprise AI platform that answers natural-language questions over data warehouses and
document corpora — a containerized microservice system running on AWS, GCP, or Azure from a
single codebase.

| | |
|---|---|
| **Identity** | Pluggable authentication unifying OIDC, SAML 2.0, API keys and internal auth behind **one platform JWT** (Strategy + Factory). Onboarding a new enterprise IdP is a database configuration change — no code, no redeploy. JWKS validation across RSA and EC keys, hardened for the real interop failures in Azure AD, Keycloak and Google. |
| **NL2SQL** | A **7-step checkpointable pipeline** across **6 warehouse engines**: semantic parse → database-anchored entity validation → clarification → deterministic routing → LLM planning → layered validation → dialect-aware generation, execution and narration. A state machine with *bounded* back-edges — controlled loops, not open-ended agent wandering. |
| **SQL safety** | **AST-level read-only enforcement** — DML/DDL rejected anywhere in the tree, including hidden inside CTEs — plus fail-closed role-scoped table authorization, re-applied before *every* execution attempt, so even an LLM "repair" cannot smuggle in a write. |
| **MCP & agents** | A platform capability server, a **100+ tool** SaaS aggregation server, a protocol-level SSE/streamable-HTTP gateway whose caller roles are **re-derived from the database, never trusted from headers**, and an OAuth 2.0 client for connecting remote MCP servers. |
| **Cloud portability** | Publisher/subscriber and object-storage ports with typed per-provider implementations — GCP Pub/Sub, AWS SNS/SQS, Azure Service Bus; GCS, S3, Azure Blob. One environment variable switches the platform's entire cloud personality; no consumer contains cloud-specific code. |

<details>
<summary>Also — connectors, ingestion, and backend reliability</summary>

<br>

- **Enterprise connectors** behind one contract — SharePoint (Microsoft Graph, OAuth 2.0
  client-credentials), S3, GCS, SMB network shares, and Oracle — with pre-save connectivity
  validation and encrypted credential storage.
- **Event-driven ingestion**: metadata-carrying signed-URL upload → bucket notification →
  pub/sub → workers → OCR, chunking, and knowledge-graph construction. Cloud-portable by
  construction, because it is built on the ports above.
- **Bulk ingestion workers** with heartbeat tracking, stuck-job detection, status-history
  auditing, and lifecycle hooks wired into service startup and shutdown.
- **FastAPI backend** across dozens of routers: correlation-ID middleware, structured logging
  that binds per-request identity context, dependency-injected auth and role guards, a
  JWT-authenticated WebSocket service bridging pub/sub to connected clients, and **70+ Alembic
  migrations** of schema stewardship.
- **Reliability patterns for LLM-heavy backends**: a single-owner retry policy (Tenacity wraps
  every model call, SDK retries disabled to prevent compounding retry storms), bounded budgets
  on every LLM loop, and graceful degradation when optional dependencies are down.

</details>

---

### Mnemos — context as a compiled artifact

A self-hosted enterprise AI assistant: chat, RAG over your documents, guarded NL2SQL over your
database, and governed MCP tool calls, behind one router that shows which flow answered and why.
Multi-tenant, real OIDC, entirely local — **no API key is required for any capability**.

It carries one deep technical claim, and it is **measured rather than asserted**: the prompt
handed to the model is a *compiled, budgeted artifact you can open*, not a concatenated string.

Six phases, modelled on a query compiler:

```
ContextRequest
   ├─ 1 BIND        intent + entity binding + authorization predicate
   ├─ 2 PLAN        logical operator DAG  (memory_scan · vector · lexical)
   ├─ 3 OPTIMISE    utility calibration → budget allocation w/ section floors
   ├─ 4 EXECUTE     deadline-bounded operators, degradation recorded not thrown
   ├─ 5 REFINE      RRF fusion → dedup → conflict resolution
   └─ 6 ASSEMBLE    trust fencing → canonical digest → manifest
   ▼
ContextBundle { digest, prompt, manifest, budget_report, explain }
```

Same corpus, same 23 questions, same embedder, same 800-token budget, on Postgres:

| | naive concat | **compiled bundle** |
|---|---|---|
| Answer-bearing text retained | 91.3% | **95.7%** |
| Quoted a **superseded** policy revision | 91.3% | **0%** |
| Carried a **superseded memory** fact | 65.2% | **0%** |
| Leaked **restricted** content | 8.7% | **0%** |
| Duplicate token waste | 11.4% | **2.2%** |
| Has a provenance manifest | 0% | **100%** |
| Exceeded the token budget | 0% | 0% |

**The headline is the second row.** Every single naive prompt handed the model an obsolete
policy figure alongside the current one — and the obsolete text is often the *better* lexical
match, so no amount of reranking fixes it. It is not a relevance problem.

Three mechanisms produce those numbers:

1. **Authorization is pushed into the scan, never applied after ranking.** Post-filtering leaks
   existence through result counts and silently drops recall when the nearest neighbours happen
   to be inaccessible.
2. **Memory claims are bitemporal and never overwritten.** Each carries world time and belief
   time, so a superseded fact is *structurally unreachable* rather than merely down-ranked, and
   "what did this system believe on date X, about date X?" is an ordinary query.
3. **The token budget is allocated, not truncated.** Greedy on utility density with per-section
   floors, then the assembled prompt is *measured* and re-allocated if it overshoots — so
   `tokens ≤ budget` is an invariant, not an estimate. A randomised test asserts it over 300
   configurations.

<details>
<summary><b>What the benchmark does not show</b> — stated plainly, because a benchmark that only reports its wins is marketing</summary>

<br>

- **The answer-retention delta is small.** On 23 synthetic questions it is one additional
  retained answer at each budget. The strongest result is governance, not a claim of general
  retrieval superiority.
- **The compiler is slower** — roughly 1.3–1.6×. It does strictly more work. The table is an
  argument about what that cost buys, not a claim of a free lunch.
- **The corpus is synthetic.** It is built to have the properties real corpora have — repeated
  boilerplate, chunk overlap, superseded revisions, a restricted document — but it is not a
  public benchmark and these numbers are not comparable to one.
- **There is no LLM in the loop.** The benchmark measures *what reaches the model*, not answer
  correctness.
- **It is portfolio-grade, not production-scale**: single-node, dev-mode services, one trust
  domain. Production *practices*, not production *scale*.

</details>

How it is built and verified:

| | |
|---|---|
| **Schema** | 42 Postgres tables, **41 under `FORCE` row-level security** — enforced against an unprivileged app role and proven by a test against a real Postgres, not merely declared |
| **Tests** | 515 backend · 128 frontend · 16 Playwright end-to-end specs against the full stack |
| **CI** | Every PR runs pytest against a real Postgres *and* a real Keycloak, plus `ruff`, `mypy --strict`, `alembic check`, and a frontend gate of lint + `tsc` + tests + a real `next build` |
| **Stack** | 10 containers — Postgres/pgvector, Redis, MinIO, Keycloak, Ollama, API, worker, realtime, web, demo MCP server — and a one-shot migration that must exit clean before the API starts |
| **Design record** | 12 architecture decision records, a threat model, and a reproducible demo path |

---

### Tools

**Languages** · Python · SQL · TypeScript · Bash

**Backend** · FastAPI · Pydantic · SQLAlchemy · Alembic · Celery · asyncio · WebSockets · REST · structlog · Tenacity

**AI systems** · RAG · NL2SQL · LLM orchestration · LLM-as-judge validation · prompt versioning · human-in-the-loop checkpointing · Langfuse · OpenTelemetry · pgvector · knowledge graphs

**Agents** · Model Context Protocol (MCP) · FastMCP · SSE / streamable-HTTP transports · OAuth for MCP · tool registries

**Identity & security** · OAuth 2.0 · OpenID Connect · SAML 2.0 · JWT · JWKS · Keycloak · Azure AD / Entra ID · RBAC · row-level security · bcrypt · AES

**Data** · PostgreSQL · MySQL · Snowflake · BigQuery · SQL Server · Oracle · Neo4j · Redis · RabbitMQ

**Cloud & infra** · GCP (Pub/Sub, GCS) · AWS (SNS/SQS, S3) · Azure (Service Bus, Blob) · Docker · Docker Compose · Nginx · signed URLs · event-driven architecture

---

<sub>B.Tech Computer Science & Technology, IIEST Shibpur · Bengaluru, India</sub>
