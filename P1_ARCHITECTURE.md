# Quant Intelligence — P1 ARCHITECTURE

```text
Document ID:                  QI-P1-DOC-ARCHITECTURE
Document Class:               Canonical P1 document — Implementation architecture normative owner
Phase:                        P1 — First Observable Intelligence Loop
Status:                       DESIGN REVIEW CANDIDATE / NOT FROZEN
P1 Status:                    DESIGN
P1 Architecture:              NOT FROZEN
P1 Requirement:               NOT FROZEN
Implementation Authorization: NONE
Business Coding:              FORBIDDEN
Accepted P0 Baseline:         p0-v1.0.1
Accepted P0 Commit:           d3a9ec58515d310466683a61b3e141879367879f
Source (authoritative):       docs/p1/QI-P1_Design_Input_v0.1.md
Source SHA-256:               a13966654150ae9b2264bc146afc363aaf3643135fe3a9b07835cfa5e585311e
Candidate Branch:             p1/design-candidate
Next Review:                  P1-04 Independent Requirement / Architecture Review
```

> **Persistence note.** This document is a faithful *split* of the Design Authority
> input. It persists architecture semantics; it does **not** redesign them. Every clause
> is traceable to a Design Input section (`Design Input §N`).
>
> **Status note.** `NOT FROZEN`. Review candidate only. No implementation authorization.

---

# 1. Normative Ownership

This document is the normative owner of the **P1 implementation architecture**:
process model, technology baseline, repository / module layout, SQLite architecture,
runtime worker topology, startup and warmup, health / readiness, API boundary, UI update
model, local security boundary and restart behaviour.

Product-level requirement semantics (goal, market scope, detector product semantics,
evidence-breadth semantics, decision responsibility boundary, evaluation eligibility,
report / delivery / UI product requirements, forward-outcome requirement, acceptance
boundary) are owned by `P1_REQUIREMENTS.md`.

Data shapes, identities, formulas, state machines and persistence entities are owned by
`P1_DATA_CONTRACT.md`.

---

# 2. Architectural Invariants

> Source: Design Input §15, §17, §18, §20.

**P1-A-01 — single-process architecture (normative).** P1 is a **single Backend Modular
Monolith** with a FastAPI lifecycle and bounded workers. It is **not** a distributed
system and **not** a microservice set.

**P1-A-02.** The backend runs as one process. Concurrency is achieved with bounded
in-process workers and asyncio, not with multiple services.

**P1-A-03.** P1 has **no** `Docker requirement` for its runtime.

**P1-A-04.** P1 has **no** distributed queue, **no** message broker, **no** task queue
framework, and **no** multi-agent runtime.

**P1-A-05.** P1 is `WITHOUT_AI`: no runtime LLM call, no runtime model inference, no
external AI provider participates in the architecture's decision path.

---

# 3. Technology Baseline

> Source: Design Input §17.

**Backend**

```text
Python
FastAPI
Pydantic v2
SQLAlchemy 2.x async
aiosqlite
Alembic
httpx
websockets
```

**Frontend**

```text
React
TypeScript
Vite
```

**P1-A-06.** The technology baseline above is the P1 baseline. It must not be silently
substituted (for example: a different ASGI framework, a different ORM, a different
frontend build tool, a synchronous SQLite driver).

**P1-A-07.** Explicitly **excluded** from P1 technology:

```text
PostgreSQL
Redis
Kafka
Celery
microservices
Docker requirement
distributed queue
multi-agent runtime
```

---

# 4. Repository / Module Layout

> Source: Design Input §18.

Target backend modules:

```text
backend/src/quant_intelligence/
  clock
  config
  canonical
  ids
  market_data
  features
  scanner
  evidence
  decision
  reporting
  delivery
  outcomes
  persistence
  runtime
  api
```

Frontend:

```text
frontend/
```

Non-secret configuration:

```text
config/p1.toml
```

**P1-A-08.** Module boundaries above are the planned P1 layout. Ownership mapping:

| Module | Responsibility (planned) |
|---|---|
| `clock` | time / timezone discipline, causal timestamp helpers |
| `config` | non-secret configuration loading (`config/p1.toml`) |
| `canonical` | canonical value normalization, deterministic bytes, domain-separated hashing |
| `ids` | deterministic identity construction (`qi_evt_`, `qi_opp_`, `qi_dec_`) |
| `market_data` | OKX adapters, normalization, market state snapshot |
| `features` | rolling features / market structure |
| `scanner` | detector cycle orchestration and detector implementations |
| `evidence` | Opportunity Episode and Evidence version management |
| `decision` | deterministic Decision Core |
| `reporting` | deterministic Simplified-Chinese report rendering |
| `delivery` | Telegram delivery attempts |
| `outcomes` | forward outcome scheduling and computation |
| `persistence` | SQLite / SQLAlchemy / Alembic boundary |
| `runtime` | worker supervision, health registry, lifecycle states |
| `api` | read-only FastAPI surface |

**P1-A-09 — forbidden directories / domains (normative).**

```text
trading
orders
positions
risk
execution
portfolio
```

These names must not appear as P1 modules, packages, namespaces or domains.

---

# 5. SQLite Architecture

> Source: Design Input §17.

**P1-A-10.** Persistence is a **local SQLite file**:

```text
data/quant_intelligence.sqlite3
```

**P1-A-11 — required pragmas (normative).**

```text
WAL mode
PRAGMA foreign_keys  = ON
PRAGMA synchronous   = FULL
PRAGMA busy_timeout  = 5000
```

**P1-A-12 — write coordination (normative).** All writes are serialized through a
**process-wide `asyncio.Lock` write coordinator** with **short transactions**.

**P1-A-13.** P1 establishes its **own Alembic baseline**:

```text
0001_p1_observable_loop
```

**No MarketPulse migrations are reused.**

**P1-A-14.** SQLite is the only durable store in P1. There is no external database, no
cache server and no broker.

Publication / suppression is a deterministic decision over persisted state; the
persistence surface used by it is owned by `P1_DATA_CONTRACT.md` §Persistence Entities.

---

# 6. Runtime Worker Topology

> Source: Design Input §15.

Bounded workers inside the single process:

```text
OKXPublicWSWorker
OKXCandleWSWorker
DerivativesContextWorker
DetectorCycleWorker
BboSamplerWorker
OutcomeSchedulerWorker
DeliveryWorker
```

**P1-A-15.** The worker set is bounded and enumerable. Every worker is supervised; no
unbounded thread / task creation is permitted.

**P1-A-16.** Worker responsibilities (planned):

| Worker | Responsibility |
|---|---|
| `OKXPublicWSWorker` | public trade + BBO stream ingestion |
| `OKXCandleWSWorker` | confirmed 1m Kline stream ingestion |
| `DerivativesContextWorker` | Open Interest (15s poll) and Funding (60s poll) REST polling |
| `DetectorCycleWorker` | 1s detector cycle per instrument over a consistent snapshot |
| `BboSamplerWorker` | 1-second durable BBO `PATH_SAMPLE` observations |
| `OutcomeSchedulerWorker` | due-scope evaluation for 5m / 15m anchors |
| `DeliveryWorker` | one automatic Telegram delivery attempt per report version |

Poll intervals, lookbacks and tolerances attached to these workers are owned by
`P1_DATA_CONTRACT.md`.

---

# 7. Startup Warmup

> Source: Design Input §16.

**P1-A-17 — Structure warmup.** Fetch **21 recent completed 1m bars via OKX REST**.
Their `available_at` is the **startup fetch response receive time**, **not** the
historical `close_time`.

**P1-A-18 — Flow warmup.** 30 seconds of live trade warmup.

**P1-A-19 — OI warmup.** 5 minutes of live OI history. **Do not fabricate a pre-start
baseline.**

**P1-A-20.** Until warmup requirements are satisfied, the corresponding detector
component is `WARMING_UP` / `INSUFFICIENT`, and readiness cannot be `READY` on account
of that component.

---

# 8. Health / Readiness

> Source: Design Input §15.

**P1-A-21 — runtime states (normative).**

```text
STARTING
WARMING_UP
READY
DEGRADED
NOT_READY
FAILED
STOPPING
STOPPED
```

**P1-A-22 — `READY` requires all of (normative).**

```text
SQLite writable
Public WS alive
Candle WS alive
default universe validated
fresh BBO
Structure / Flow / OI warmups ready
Detector / Outcome workers alive
Telegram configuration valid
no required worker unexpected exit
```

**P1-A-23 — fail-closed health (normative).**

```text
Unexpected normal return     => UNEXPECTED_EXIT, not healthy
Exception                    => FAILED
Missing / unknown / stale / failed / unexpected required component => NOT_READY
```

**P1-A-24.** Health must be **fail-closed**: an unregistered, unknown, missing or stale
required component must never be reported as healthy. (Inherits the P0-derived lesson
that a health registry defaulting unknown checks to `HEALTHY` is not acceptable.)

**P1-A-25 — default freshness bounds (normative).**

```text
Public WS heartbeat   15s
Candle WS             90s
OI                    45s
Funding              120s
Detector worker       5s
Outcome worker        5s
```

**P1-A-26.** Any default instrument validation failure disables that instrument and makes
required-universe health `NOT READY` (cross-ref `P1_REQUIREMENTS.md` P1-R-04).

---

# 9. API Boundary

> Source: Design Input §20.

**P1-A-27 — read-only API surface (normative).**

```text
GET /api/v1/opportunities
GET /api/v1/opportunities/{opportunity_id}
GET /api/v1/health
```

**P1-A-28.** There is **no mutation API** for trade, order, position, decision, threshold
or retry.

**P1-A-29.** Configuration changes require **restart with frozen config**. There is no
runtime reconfiguration API and no runtime threshold mutation.

**P1-A-30.** The API reads persisted truth. It must not synthesize, recompute or
embellish decision state that is not persisted.

---

# 10. UI Update Model

> Source: Design Input §20.

**P1-A-31.** The frontend **polls** opportunities every **2 seconds**.

**P1-A-32.** There is **no frontend WebSocket** in P1.

**P1-A-33.** UI pages are exactly three (cross-ref `P1_REQUIREMENTS.md` P1-R-51):

```text
/                   — 实时机会
/opportunities/:id  — 机会详情
/history            — 历史与 Outcome
```

**P1-A-34.** History must present the original immutable records (Decision, Report
version, Evidence version, Outcome), not a recomputed view.

---

# 11. Local Security Boundary

> Source: Design Input §20, §12.

**P1-A-35.** FastAPI binds **`127.0.0.1`**; the frontend runs on **localhost**.

**P1-A-36.** **LAN / Internet exposure requires a new security requirement.** P1 does
not authorize any external network exposure of the API or UI.

**P1-A-37.** Secrets are read **only** from environment variables
(`QI_TELEGRAM_BOT_TOKEN`, `QI_TELEGRAM_CHAT_ID`) and must not enter config, database,
logs or reports (cross-ref `P1_REQUIREMENTS.md` P1-R-48).

**P1-A-38.** P1 has no inbound network surface other than the loopback-bound read-only
API. There is no inbound webhook and no inbound command path from the notification
channel.

---

# 12. Restart Behaviour

> Source: Design Input §15.

**P1-A-39 — restart invariants (normative).**

```text
Restart must NOT duplicate reports
Restart must NOT duplicate outcome scopes
Restart must NOT move due_at
Mature pending outcome scopes resume
Detector rolling state warms up again
```

**P1-A-40.** Durable facts survive restart; in-memory bounded structures (raw trade
ticks, confirmed-Kline rolling history, OI / funding rolling samples, live BBO
latest-value) do not, and are rebuilt through warmup. Durable-vs-bounded classification
is owned by `P1_DATA_CONTRACT.md`.

**P1-A-41.** Restart must resolve against the same persisted identity and version
contracts, so that replaying the same facts after restart cannot create a new identity
for an already-persisted fact.

---

# 13. Architecture Exclusions Summary

> Source: Design Input §17, §18, §20.

```text
PostgreSQL                       — EXCLUDED
Redis                            — EXCLUDED
Kafka                            — EXCLUDED
Celery                           — EXCLUDED
microservices                    — EXCLUDED
Docker requirement               — EXCLUDED
distributed queue                — EXCLUDED
multi-agent runtime              — EXCLUDED
frontend WebSocket               — EXCLUDED
LAN / Internet exposure          — EXCLUDED (requires new security requirement)
mutation API                     — EXCLUDED
inbound webhook / command bot    — EXCLUDED
trading / orders / positions /
  risk / execution / portfolio   — EXCLUDED as directories and domains
Runtime AI / LLM                 — EXCLUDED
```

---

# 14. Traceability Index

| Architecture block | Design Input source |
|---|---|
| §2 Architectural Invariants | §15, §17, §18, §20 |
| §3 Technology Baseline | §17 |
| §4 Repository / Module Layout | §18 |
| §5 SQLite Architecture | §17 |
| §6 Runtime Worker Topology | §15 |
| §7 Startup Warmup | §16 |
| §8 Health / Readiness | §15 |
| §9 API Boundary | §20 |
| §10 UI Update Model | §20 |
| §11 Local Security Boundary | §12, §20 |
| §12 Restart Behaviour | §15 |
| §13 Architecture Exclusions | §17, §18, §20 |

---

# 15. Boundary

```text
P0                        = ACCEPTED
P1 Architecture           = NOT FROZEN
P1 Requirement            = NOT FROZEN
P1 Implementation         = NOT AUTHORIZED
Business Coding           = FORBIDDEN
This document authorizes: NOTHING
```
