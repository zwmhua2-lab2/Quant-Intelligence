# docs/p1 — Quant Intelligence P1 Design Candidate

```text
Directory purpose:  P1 phase source artifact + P1 phase document index
Phase:              P1 — First Observable Intelligence Loop
Status:             DESIGN REVIEW CANDIDATE / NOT FROZEN
P1 Status:          DESIGN
Implementation Authorization: NONE
Business Coding:    FORBIDDEN
Accepted P0 Baseline: p0-v1.0.1
```

> **This directory does not declare P1 Frozen, Accepted, or Implementable.**
> P1 Requirement / Architecture are `NOT FROZEN`, and P1 implementation is
> `NOT AUTHORIZED`.

---

# 1. P1 Design Candidate History

```text
P1-01  Requirement Design
       = DESIGN COMPLETE

P1-02  Architecture + Data Contract + Reuse Design
       = DESIGN COMPLETE

P1-03  Document-only P1 Design Candidate Persistence
       = DESIGN CANDIDATE PERSISTED / REVIEW PENDING

P1-04  Independent Requirement / Architecture Review
       = NOT STARTED  (next)

P1-05  Freeze + P1-I01 Formal Implementation Task
       = NOT STARTED
```

`P1-01` and `P1-02` were design work performed by the Design Authority. Their combined
output was delivered as a single authoritative design artifact and persisted
document-by-document by `P1-03` (`QI-P1-03-I01`).

No design semantic was changed during persistence. The persistence task only split,
organized, cross-referenced, verified, committed and pushed.

---

# 2. Source Artifact

The authoritative P1 design input for this candidate:

```text
Path:      docs/p1/QI-P1_Design_Input_v0.1.md
Document:  QI-P1-DESIGN-INPUT-0.1
SHA-256:   a13966654150ae9b2264bc146afc363aaf3643135fe3a9b07835cfa5e585311e
Bytes:     18747
Lines:     570
```

It is stored **byte-for-byte** as delivered. It is not reformatted, renormalized or
regenerated. Its SHA-256 is re-verified inside the Git object database (not only in the
working tree) — see `dev_log/QI-P1-03-I01.md`.

This artifact is a **source artifact** (historical input). It is **not** a canonical P1
document and it is not the review target.

---

# 3. Canonical Split

The Design Input was faithfully split into canonical P1 documents at the repository root,
mirroring the P0 convention (canonical documents at root; phase source artifacts under
`docs/<phase>/`):

| Canonical P1 document | Normative ownership |
|---|---|
| `P1_REQUIREMENTS.md` | P1 product requirement: goal, market scope, required inputs, detector set and product semantics, evidence-breadth semantics, decision responsibilities / boundaries, evaluation eligibility, report / delivery UI product requirements, forward outcome requirement, out-of-scope boundary, verification modes, acceptance boundary, value evidence |
| `P1_ARCHITECTURE.md` | process model, technology baseline, repository / module layout, SQLite architecture, runtime worker topology, startup warmup, health / readiness, API boundary, UI update model, local security boundary, restart behaviour |
| `P1_DATA_CONTRACT.md` | causal timestamps, identity construction, canonicalization / hashing, detector formulas and defaults, event state machine, episode / evidence versioning, quality / direction / priority, eligibility, decision contract, report version, delivery attempt, BBO observations, outcome scopes / results, coverage / missing semantics, persistence entities, retention |
| `P1_REUSE_MANIFEST.md` | MarketPulse-AI reuse decisions: ADAPT-SUBSET / ADAPT-CONCEPT / REIMPLEMENT FROM CONTRACT / EXCLUDE, pinned source SHA, carry-over source limitations |
| `P1_IMPLEMENTATION_PLAN.md` | P1-I01 … P1-I04 batch planning only — `PLANNED ONLY`, not implementation authorization |
| `P1_DESIGN_MANIFEST.md` | this design candidate's identity, status, blob identities, source digest, candidate branch and next review |

State pointers updated by the same task:

```text
PROJECT_STATE.md   — P1 current-state pointer
ROADMAP.md         — P1 design status only (P0 history and P1–P4 scope unchanged)
README.md          — P1 design candidate navigation only
```

P0 semantic canonical documents and the accepted baseline are **unchanged** — see
`P1_DESIGN_MANIFEST.md` §P0 Integrity.

---

# 4. Review Target

```text
Next Review:  P1-04 Independent Requirement / Architecture Review
Candidate Branch: p1/design-candidate
Review Basis: Repository Truth at the exact candidate Git SHA
              (NOT a chat summary, NOT a worker report)
```

`P1-04` must review the candidate branch tip. The candidate Git SHA is resolved by:

```bash
git rev-parse p1/design-candidate^{commit}
git ls-remote https://github.com/zwmhua2-lab2/Quant-Intelligence.git refs/heads/p1/design-candidate
```

A review verdict binds only the exact candidate Git SHA it names.

---

# 5. Current Status

```text
P0                        = ACCEPTED
P1 Status                 = DESIGN
P1-01                     = DESIGN COMPLETE
P1-02                     = DESIGN COMPLETE
P1-03                     = DESIGN CANDIDATE PERSISTED / REVIEW PENDING
P1 Requirement            = NOT FROZEN
P1 Architecture           = NOT FROZEN
P1 Implementation         = NOT AUTHORIZED
Business Coding           = FORBIDDEN
```

**Next = P1-04 Independent Requirement / Architecture Review.**

After a `PASS` verdict, the next step would be P1-05 Freeze plus the P1-I01 formal
implementation task. Neither has started.

---

# 6. Boundary

```text
This directory declares:   a review candidate
This directory does NOT declare:  Frozen, Accepted, or Implementable
P1 implementation started:  NO
Business code created:      NO
```
