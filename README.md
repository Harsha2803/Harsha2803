<h1 align="center">Hi, I'm Cheella Sree Harsha 👋</h1>

<p align="center">
  <b>Backend &amp; AI Platform Engineer</b> — I work on the engineering <i>around</i> models:<br>
  authorization, validation, budgeting, observability, failure recovery.
</p>

<p align="center">
  <a href="https://www.linkedin.com/in/cheellasreeharsha/"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"></a>
  <a href="mailto:cheellasreeharsha2803@gmail.com"><img alt="Email" src="https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white"></a>
  <img alt="Location" src="https://img.shields.io/badge/Bengaluru,_India-34A853?style=for-the-badge&logo=googlemaps&logoColor=white">
</p>

<p align="center">
  <img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white">
  <img alt="FastAPI" src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white">
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white">
  <img alt="pgvector" src="https://img.shields.io/badge/pgvector-4169E1?style=flat-square&logo=postgresql&logoColor=white">
  <img alt="Redis" src="https://img.shields.io/badge/Redis-FF4438?style=flat-square&logo=redis&logoColor=white">
  <img alt="Docker" src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white">
  <img alt="Keycloak" src="https://img.shields.io/badge/Keycloak-008AAA?style=flat-square&logo=keycloak&logoColor=white">
</p>

---

Two years of building secure enterprise systems in Python and FastAPI at **JK Tech**: pluggable
identity, validated NL2SQL workflows, governed MCP tool integrations, multi-cloud service
abstractions, and event-driven content ingestion.

The through-line is treating an LLM as an **unreliable component**: every model output gets
validated, authorized, budgeted, traced, or human-gated before it can do anything. 🧯

---

## 💼 What I build at work

On an enterprise AI platform that answers natural-language questions over data warehouses and
document corpora — a containerized microservice system running on AWS, GCP, or Azure from a
single codebase.

| Area | What I built |
|---|---|
| 🔐 **Identity** | Pluggable authentication unifying OIDC, SAML 2.0, API keys and internal auth behind **one platform JWT** (Strategy + Factory). Onboarding a new enterprise IdP is a database configuration change — no code, no redeploy. JWKS validation across RSA and EC keys, hardened for the real interop failures in Azure AD, Keycloak and Google. |
| 🗄️ **NL2SQL** | A **7-step checkpointable pipeline** across **6 warehouse engines**: semantic parse → database-anchored entity validation → clarification → deterministic routing → LLM planning → layered validation → dialect-aware generation, execution and narration. A state machine with *bounded* back-edges — controlled loops, not open-ended agent wandering. |
| 🛡️ **SQL safety** | **AST-level read-only enforcement** — DML/DDL rejected anywhere in the tree, including hidden inside CTEs — plus fail-closed role-scoped table authorization, re-applied before *every* execution attempt, so even an LLM "repair" cannot smuggle in a write. |
| 🔌 **MCP & agents** | A platform capability server, a **100+ tool** SaaS aggregation server, a protocol-level SSE/streamable-HTTP gateway whose caller roles are **re-derived from the database, never trusted from headers**, and an OAuth 2.0 client for connecting remote MCP servers. |
| ☁️ **Cloud portability** | Publisher/subscriber and object-storage ports with typed per-provider implementations — GCP Pub/Sub, AWS SNS/SQS, Azure Service Bus; GCS, S3, Azure Blob. One environment variable switches the platform's entire cloud personality; no consumer contains cloud-specific code. |

<details>
<summary>📦 <b>Also — connectors, ingestion, and backend reliability</b></summary>

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

## 🧠 Mnemos — context as a compiled artifact

A self-hosted enterprise AI assistant: chat, RAG over your documents, guarded NL2SQL over your
database, and governed MCP tool calls, behind one router that shows which flow answered and why.
Multi-tenant, real OIDC, entirely local — **no API key is required for any capability**. 🔓

It carries one deep technical claim, and it is **measured rather than asserted**: the prompt
handed to the model is a *compiled, budgeted artifact you can open*, not a concatenated string.

⚙️ Six phases, modelled on a query compiler:

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

📊 Same corpus, same 23 questions, same embedder, same 800-token budget, on Postgres:

| Metric | naive concat | **compiled bundle** |
|---|---|---|
| ✅ Answer-bearing text retained | 91.3% | **95.7%** |
| ⚠️ Quoted a **superseded** policy revision | 91.3% | **0%** |
| ⚠️ Carried a **superseded memory** fact | 65.2% | **0%** |
| 🚨 Leaked **restricted** content | 8.7% | **0%** |
| ♻️ Duplicate token waste | 11.4% | **2.2%** |
| 🧾 Has a provenance manifest | 0% | **100%** |
| 📏 Exceeded the token budget | 0% | 0% |

**The headline is the second row.** Every single naive prompt handed the model an obsolete
policy figure alongside the current one — and the obsolete text is often the *better* lexical
match, so no amount of reranking fixes it. It is not a relevance problem.

Three mechanisms produce those numbers:

1. 🔍 **Authorization is pushed into the scan, never applied after ranking.** Post-filtering leaks
   existence through result counts and silently drops recall when the nearest neighbours happen
   to be inaccessible.
2. ⏳ **Memory claims are bitemporal and never overwritten.** Each carries world time and belief
   time, so a superseded fact is *structurally unreachable* rather than merely down-ranked, and
   "what did this system believe on date X, about date X?" is an ordinary query.
3. 🎯 **The token budget is allocated, not truncated.** Greedy on utility density with per-section
   floors, then the assembled prompt is *measured* and re-allocated if it overshoots — so
   `tokens ≤ budget` is an invariant, not an estimate. A randomised test asserts it over 300
   configurations.

<details>
<summary>🔬 <b>What the benchmark does not show</b> — stated plainly, because a benchmark that only reports its wins is marketing</summary>

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

🏗️ How it is built and verified:

| Area | Evidence |
|---|---|
| 🗃️ **Schema** | 42 Postgres tables, **41 under `FORCE` row-level security** — enforced against an unprivileged app role and proven by a test against a real Postgres, not merely declared |
| 🧪 **Tests** | 515 backend · 128 frontend · 16 Playwright end-to-end specs against the full stack |
| 🔁 **CI** | Every PR runs pytest against a real Postgres *and* a real Keycloak, plus `ruff`, `mypy --strict`, `alembic check`, and a frontend gate of lint + `tsc` + tests + a real `next build` |
| 🐳 **Stack** | 10 containers — Postgres/pgvector, Redis, MinIO, Keycloak, Ollama, API, worker, realtime, web, demo MCP server — and a one-shot migration that must exit clean before the API starts |
| 📐 **Design record** | 12 architecture decision records, a threat model, and a reproducible demo path |

---

## 🛠️ Tools

**🐍 Languages**

<img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white"> <img alt="SQL" src="https://img.shields.io/badge/SQL-CC2927?style=flat-square&logo=databricks&logoColor=white"> <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white"> <img alt="Bash" src="https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white">

**⚡ Backend**

<img alt="FastAPI" src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white"> <img alt="Pydantic" src="https://img.shields.io/badge/Pydantic-E92063?style=flat-square&logo=pydantic&logoColor=white"> <img alt="SQLAlchemy" src="https://img.shields.io/badge/SQLAlchemy-D71F00?style=flat-square&logo=sqlalchemy&logoColor=white"> <img alt="Alembic" src="https://img.shields.io/badge/Alembic-6BA81E?style=flat-square&logo=alembic&logoColor=white"> <img alt="Celery" src="https://img.shields.io/badge/Celery-37814A?style=flat-square&logo=celery&logoColor=white"> <img alt="asyncio" src="https://img.shields.io/badge/asyncio-3776AB?style=flat-square&logo=python&logoColor=white"> <img alt="WebSockets" src="https://img.shields.io/badge/WebSockets-010101?style=flat-square&logo=socketdotio&logoColor=white">

**🤖 AI systems**

<img alt="RAG" src="https://img.shields.io/badge/RAG-8A2BE2?style=flat-square&logo=openaigym&logoColor=white"> <img alt="NL2SQL" src="https://img.shields.io/badge/NL2SQL-FF6F00?style=flat-square&logo=databricks&logoColor=white"> <img alt="pgvector" src="https://img.shields.io/badge/pgvector-4169E1?style=flat-square&logo=postgresql&logoColor=white"> <img alt="Langfuse" src="https://img.shields.io/badge/Langfuse-0A0A0A?style=flat-square&logo=langchain&logoColor=white"> <img alt="OpenTelemetry" src="https://img.shields.io/badge/OpenTelemetry-425CC7?style=flat-square&logo=opentelemetry&logoColor=white"> <img alt="Ollama" src="https://img.shields.io/badge/Ollama-000000?style=flat-square&logo=ollama&logoColor=white">

LLM orchestration · LLM-as-judge validation · prompt versioning · human-in-the-loop checkpointing · knowledge graphs

**🔗 Agents**

<img alt="MCP" src="https://img.shields.io/badge/Model_Context_Protocol-D97757?style=flat-square&logo=anthropic&logoColor=white"> <img alt="FastMCP" src="https://img.shields.io/badge/FastMCP-009688?style=flat-square&logo=fastapi&logoColor=white">

SSE / streamable-HTTP transports · OAuth for MCP · tool registries

**🔒 Identity & security**

<img alt="OAuth2" src="https://img.shields.io/badge/OAuth_2.0-EB5424?style=flat-square&logo=auth0&logoColor=white"> <img alt="OIDC" src="https://img.shields.io/badge/OpenID_Connect-F78C40?style=flat-square&logo=openid&logoColor=white"> <img alt="SAML" src="https://img.shields.io/badge/SAML_2.0-005571?style=flat-square&logo=okta&logoColor=white"> <img alt="JWT" src="https://img.shields.io/badge/JWT-000000?style=flat-square&logo=jsonwebtokens&logoColor=white"> <img alt="Keycloak" src="https://img.shields.io/badge/Keycloak-008AAA?style=flat-square&logo=keycloak&logoColor=white"> <img alt="Entra ID" src="https://img.shields.io/badge/Microsoft_Entra_ID-0078D4?style=flat-square&logo=microsoftazure&logoColor=white">

**💾 Data**

<img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white"> <img alt="MySQL" src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white"> <img alt="Snowflake" src="https://img.shields.io/badge/Snowflake-29B5E8?style=flat-square&logo=snowflake&logoColor=white"> <img alt="BigQuery" src="https://img.shields.io/badge/BigQuery-669DF6?style=flat-square&logo=googlebigquery&logoColor=white"> <img alt="SQL Server" src="https://img.shields.io/badge/SQL_Server-CC2927?style=flat-square&logo=microsoftsqlserver&logoColor=white"> <img alt="Oracle" src="https://img.shields.io/badge/Oracle-F80000?style=flat-square&logo=oracle&logoColor=white"> <img alt="Neo4j" src="https://img.shields.io/badge/Neo4j-4581C3?style=flat-square&logo=neo4j&logoColor=white"> <img alt="Redis" src="https://img.shields.io/badge/Redis-FF4438?style=flat-square&logo=redis&logoColor=white"> <img alt="RabbitMQ" src="https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white">

**☁️ Cloud & infra**

<img alt="AWS" src="https://img.shields.io/badge/AWS-FF9900?style=flat-square&logo=amazonwebservices&logoColor=white"> <img alt="GCP" src="https://img.shields.io/badge/Google_Cloud-4285F4?style=flat-square&logo=googlecloud&logoColor=white"> <img alt="Azure" src="https://img.shields.io/badge/Azure-0078D4?style=flat-square&logo=microsoftazure&logoColor=white"> <img alt="Docker" src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white"> <img alt="Nginx" src="https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=nginx&logoColor=white"> <img alt="MinIO" src="https://img.shields.io/badge/MinIO-C72E49?style=flat-square&logo=minio&logoColor=white">

---

## 📫 Reach me

<p align="center">
  <a href="https://www.linkedin.com/in/cheellasreeharsha/"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"></a>
  <a href="mailto:cheellasreeharsha2803@gmail.com"><img alt="Email" src="https://img.shields.io/badge/cheellasreeharsha2803@gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white"></a>
</p>

<p align="center">
  <sub>🎓 B.Tech Computer Science &amp; Technology, IIEST Shibpur</sub>
</p>
