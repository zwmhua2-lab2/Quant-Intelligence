# Quant Intelligence — P1 IMPLEMENTATION PLAN

```text
Document ID:                  QI-P1-DOC-IMPLEMENTATION-PLAN
Document Class:               Canonical P1 document — Batch planning only
Phase:                        P1 — First Observable Intelligence Loop
Status:                       DESIGN REVIEW CANDIDATE / NOT FROZEN

PLANNED ONLY
NOT IMPLEMENTATION AUTHORIZATION

Implementation Authorization: NONE
Business Coding:              FORBIDDEN
Accepted P0 Baseline:         p0-v1.0.1
Accepted P0 Commit:           d3a9ec58515d310466683a61b3e141879367879f
Source (authoritative):       docs/p1/QI-P1_Design_Input_v0.1.md
Source SHA-256:               a13966654150ae9b2264bc146afc363aaf3643135fe3a9b07835cfa5e585311e
Candidate Branch:             p1/design-candidate
Next Review:                  P1-04 Independent Requirement / Architecture Review
```

> ## PLANNED ONLY — NOT IMPLEMENTATION AUTHORIZATION
>
> This document records an **implementation plan**. It does **not** authorize
> implementation of any batch. `P1 Implementation Authorization = NONE`.
>
> No batch below may be started. Starting `P1-I01` requires its own formal,
> separately-frozen implementation task after P1 design freeze. This task
> (`QI-P1-03-I01`) explicitly must not start `P1-I01`.

> **Persistence note.** This document is a faithful *split* of Design Input §24. Batch
> goals, scopes, out-of-scope boundaries and acceptance boundaries are transcribed from
> the Design Input. It does **not** redesign the batches and does **not** add batch scope.

---

# 1. Batch Overview

> Source: Design Input §24.

```text
P1-I01  Foundation + Market Data + Persistence
P1-I02  Detector → Evidence → Decision → Report Core
P1-I03  Delivery + Read API + UI
P1-I04  Forward Outcome + Restart + End-to-End
```

**P1-P-01.** The four batches above are the complete P1 batch set. No additional batch
may be introduced in this document.

**P1-P-02 — dependency field convention (normative for this document).** The
authoritative Design Input declares the batch **sequence** (§24 order), the batch scope
and the batch acceptance boundaries. It does **not** declare a formal inter-batch
dependency contract. Each `Dependency` field below therefore records only:

```text
(a) Sequence position
(b) Formal inter-batch dependency status
(c) Execution prerequisite
```

**No additional dependency semantic is asserted.** This is recorded explicitly so that
P1-04 can review whether an explicit dependency contract is required.

---

# 2. P1-I01｜Foundation + Market Data + Persistence

> Source: Design Input §24.

```text
Goal:      Establish the P1 foundation: project skeleton, configuration, time and
           canonical-hash discipline, SQLite persistence and Alembic baseline, real
           OKX market-data adapters with normalized contracts and market state,
           BBO sampling, and runtime supervision / fail-closed health.
Scope:     skeleton / config / clock / canonical / hash
           SQLite / Alembic
           OKX adapters / normalized contracts / market state
           BBO sampling
           runtime supervision / health
Out of Scope:
           Detector
           Report
           UI
Acceptance Boundary:
           real BTC / ETH / SOL SWAP data
           + SQLite
           + fail-closed health
           with NO Detector / Report / UI
Dependency:
           Sequence position:
             first batch in Design Input §24

           Formal inter-batch dependency:
             NOT DEFINED IN AUTHORITATIVE DESIGN INPUT

           Execution prerequisite:
             P1 design must be frozen
             and a separate Formal Implementation Task must authorize execution
Status:    PLANNED ONLY — not authorized
```

---

# 3. P1-I02｜Detector → Evidence → Decision → Report Core

> Source: Design Input §24.

```text
Goal:      Implement the deterministic core chain: the three detectors, the event state
           machine, Episode / Evidence revisions, Evaluation Eligibility,
           quality / direction / priority derivation, the deterministic Decision, and the
           Simplified-Chinese deterministic report.
Scope:     3 Detectors
           event state machine
           Episode / Evidence revisions
           Evaluation Eligibility
           quality / direction / priority
           Decision
           Chinese deterministic Report
Out of Scope:
           AI
           delivery
           UI
Acceptance Boundary:
           deterministic fixtures
           + complete core chain
           with NO AI, NO delivery, NO UI
Dependency:
           Sequence position:
             after P1-I01 in Design Input §24

           Formal inter-batch dependency:
             NOT DEFINED IN AUTHORITATIVE DESIGN INPUT

           Execution prerequisite:
             P1 design must be frozen
             and a separate Formal Implementation Task must authorize execution
Status:    PLANNED ONLY — not authorized
```

---

# 4. P1-I03｜Delivery + Read API + UI

> Source: Design Input §24.

```text
Goal:      Deliver published reports through the single external channel, expose the
           read-only REST API, and provide the three Simplified-Chinese UI pages.
Scope:     Telegram
           read-only REST API
           实时机会 / 机会详情 / 历史与 Outcome UI
Out of Scope:
           any mutation API
           any inbound Telegram processing
           any LAN / Internet exposure
Acceptance Boundary:
           real report reaches Telegram
           + UI reads DB truth and shows lineage
Dependency:
           Sequence position:
             after P1-I02 in Design Input §24

           Formal inter-batch dependency:
             NOT DEFINED IN AUTHORITATIVE DESIGN INPUT

           Execution prerequisite:
             P1 design must be frozen
             and a separate Formal Implementation Task must authorize execution
Status:    PLANNED ONLY — not authorized
```

---

# 5. P1-I04｜Forward Outcome + Restart + P1 End-to-End

> Source: Design Input §24.

```text
Goal:      Close the observable loop: dual anchor scopes, 5m / 15m forward outcomes,
           coverage / MFE / MAE, restart resume, pruning, and full live acceptance.
Scope:     Detection / Publication anchors
           5m / 15m outcome
           coverage / MFE / MAE
           restart resume
           pruning
           full live acceptance
Out of Scope:
           any trading / economic capability
           any Runtime AI
Acceptance Boundary:
           complete P1 observable intelligence loop
Dependency:
           Sequence position:
             after P1-I03 in Design Input §24

           Formal inter-batch dependency:
             NOT DEFINED IN AUTHORITATIVE DESIGN INPUT

           Execution prerequisite:
             P1 design must be frozen
             and a separate Formal Implementation Task must authorize execution
Status:    PLANNED ONLY — not authorized
```

---

# 6. Plan Boundary

**P1-P-03.** Every batch above carries `Status: PLANNED ONLY — not authorized`.

**P1-P-04 — forbidden by this plan.** This plan does **not** permit, and P1 does not
authorize:

```text
starting any batch
creating any business code
creating any database
creating any migration
creating any frontend implementation
calling OKX runtime
calling Telegram
```

**P1-P-05.** Batch acceptance boundaries must not be weakened, and batch scope must not
be silently expanded or merged.

**P1-P-06 — freeze prerequisite.** Batch execution is gated by P1 design freeze, which in
turn is gated by the P1-04 Independent Requirement / Architecture Review.

---

# 7. Traceability Index

| Plan block | Design Input source |
|---|---|
| §1 Batch Overview | §24 |
| §2 P1-I01 | §24 |
| §3 P1-I02 | §24 |
| §4 P1-I03 | §24 |
| §5 P1-I04 | §24 |

---

# 8. Boundary

```text
P0                        = ACCEPTED
P1 Implementation Plan    = PLANNED ONLY
P1 Implementation         = NOT AUTHORIZED
Business Coding           = FORBIDDEN
P1-I01 … P1-I04           = NOT STARTED, NOT AUTHORIZED
This document authorizes: NOTHING
```
