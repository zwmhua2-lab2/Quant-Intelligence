# Quant Intelligence — P1 DESIGN MANIFEST

```text
Document ID:                  QI-P1-DOC-DESIGN-MANIFEST
Document Class:               Canonical P1 document — P1 design candidate identity record
Phase:                        P1 — First Observable Intelligence Loop

P1 DESIGN REVIEW CANDIDATE
NOT FROZEN
NOT IMPLEMENTATION AUTHORIZED

P1 Status:                    DESIGN
P1 Requirement:               NOT FROZEN
P1 Architecture:              NOT FROZEN
P1 Data Contract:             NOT FROZEN
P1 Implementation:            NOT AUTHORIZED
Business Coding:              FORBIDDEN
Implementation Authorization: NONE

Accepted P0 Baseline:         p0-v1.0.1
Accepted P0 SHA:              d3a9ec58515d310466683a61b3e141879367879f
Candidate Branch:             p1/design-candidate
Next Review:                  P1-04 Independent Requirement / Architecture Review
```

> `P1 DESIGN REVIEW CANDIDATE / NOT FROZEN / NOT IMPLEMENTATION AUTHORIZED` is the only
> valid status for this candidate. This manifest does **not** declare P1 accepted,
> implemented or implementable.

---

# 1. Candidate Identification

```text
Project:
  Quant Intelligence

Repository:
  zwmhua2-lab2/Quant-Intelligence

Accepted P0 Baseline (tag):
  p0-v1.0.1   (annotated)

Accepted P0 SHA:
  d3a9ec58515d310466683a61b3e141879367879f
  = commit referenced by refs/tags/p0-v1.0.1

Candidate Branch:
  p1/design-candidate

Candidate Branch Base:
  d3a9ec58515d310466683a61b3e141879367879f   (exact accepted P0 commit — NOT
                                              p0/bootstrap-candidate)

Candidate Git SHA:
  = branch tip at handoff   (see §1a)

Next Review:
  P1-04 Independent Requirement / Architecture Review
```

## 1a. Self-reference note (read this before binding a review)

A Git object cannot contain its own hash. Therefore:

- `Candidate Git SHA` is **the tip commit of `p1/design-candidate` at handoff**, i.e. the
  commit that introduces this manifest. It cannot be written literally inside itself.
- Resolution procedure (deterministic, no chat context needed):

```bash
git rev-parse p1/design-candidate^{commit}
git ls-remote https://github.com/zwmhua2-lab2/Quant-Intelligence.git refs/heads/p1/design-candidate
```

- The literal value obtained from those commands is published in the `QI-P1-03-I01`
  completion report. It is **not** duplicated into this file, for the reason above.
- The **blob identities** in §3 *are* recorded literally, because a blob hash does not
  depend on the commit that contains it.
- `Bootstrap/design commit` = the single persistence commit on `p1/design-candidate`.
  `Final Candidate SHA` = the last pushed commit on the branch. Under a single-commit
  persistence these are the same commit.
- `P1_DESIGN_MANIFEST.md` does **not** record its own blob, for the same self-reference
  reason.

---

# 2. Authoritative Input

```text
P1 Design Input (source artifact):
  docs/p1/QI-P1_Design_Input_v0.1.md

Document ID:
  QI-P1-DESIGN-INPUT-0.1

Design Input SHA-256:
  a13966654150ae9b2264bc146afc363aaf3643135fe3a9b07835cfa5e585311e

Bytes:
  18747

Lines:
  570

Byte preservation:
  BYTE-PRESERVED. Stored exactly as delivered — not reformatted, renormalized
  or regenerated. Digest re-verified inside the Git object database
  (`git cat-file blob … | sha256sum`), not only in the working tree.

Document Class:
  SOURCE ARTIFACT (historical input) — NOT a canonical P1 document and NOT the
  review target by itself. The review target is the canonical split plus this
  manifest, as a whole, at the candidate Git SHA.
```

---

# 3. Canonical Split — Blob Identities

Blob identities are Git blob SHAs. They are comparable across commits and independently
reproducible via:

```bash
git ls-tree -r <commit> -- <path>
git hash-object <path>
```

## 3a. Canonical P1 documents

| # | Path | Git blob SHA | Normative ownership summary |
|---|---|---|---|
| 1 | `P1_REQUIREMENTS.md` | `b37789a27298642e5f261d35dd9eb5a9dd76e235` | P1 product requirement: goal, market scope, required inputs, detector set and product semantics, evidence breadth, decision responsibilities / boundaries, evaluation eligibility, report / delivery / UI product requirements, forward outcome, out of scope, verification modes, acceptance boundary, value evidence |
| 2 | `P1_ARCHITECTURE.md` | `5e38bc952db22520e11a495e591e9f0149ec4008` | single-process architecture, technology baseline, repository / module layout, SQLite architecture, runtime worker topology, startup warmup, health / readiness, API boundary, UI update model, local security boundary, restart behaviour |
| 3 | `P1_DATA_CONTRACT.md` | `eedb1c89e10debd46834bd3f0fe68b63a5ebb1a8` | causal timestamps, Market Event identity, canonical / hash rules, detector formulas and defaults, event state machine, Opportunity Episode, Evidence revision, Quality Tier, Direction Coherence, Processing Priority, Evaluation Eligibility, Decision contract, Report version, Delivery attempt, BBO observations, Outcome scopes / results, coverage / missing semantics, persistence entities, retention |
| 4 | `P1_REUSE_MANIFEST.md` | `7e1bdfd8ef3d022cc1eb3d4f1bf2c5d6fe96dba3` | MarketPulse reuse decisions ADAPT-SUBSET / ADAPT-CONCEPT / REIMPLEMENT FROM CONTRACT / EXCLUDE, pinned source SHA, carry-over source limitations |
| 5 | `P1_IMPLEMENTATION_PLAN.md` | `49b0f589cf95cd05c3253e2f26fc52a2741fa026` | P1-I01 … P1-I04 batch planning only (`PLANNED ONLY`, not implementation authorization) |
| 6 | `P1_DESIGN_MANIFEST.md` | *(self — see §1a)* | this design candidate's identity, status, blob identities, source digest, candidate branch, next review |

## 3b. Source artifact and P1 index

| # | Path | Git blob SHA |
|---|---|---|
| 7 | `docs/p1/QI-P1_Design_Input_v0.1.md` | `7c3fa2bc3a008ae4cea921d4f08c345f1098c9f5` |
| 8 | `docs/p1/README.md` | `80724f3a69cc0b58b5c3071b7649c667f3ca78f8` |

## 3c. State pointers and navigation updated by `QI-P1-03-I01`

| # | Path | Git blob SHA | Change class |
|---|---|---|---|
| 9 | `PROJECT_STATE.md` | `279b3e2039d60f4a880e97d86dd30cf101ec402e` | added §7 authoritative P1 state; updated stale P1 pointer |
| 10 | `ROADMAP.md` | `153e6c950b03dff1be73f3d0d86b651ea20f5164` | Part A P1 status line updated; Part G added (P1 design status only) |
| 11 | `README.md` | `329de6b9b6f3371066173ed4d6bba5a427f84e62` | appended P1 design candidate navigation section only |

## 3d. Task record

| # | Path | Git blob SHA |
|---|---|---|
| 12 | `dev_log/QI-P1-03-I01.md` | `569cfa011c1f822d58d24d17a899a5dd00a43592` |

---

# 4. P0 Integrity

The accepted P0 baseline is **unchanged**. Blob identity at the accepted tag vs the
candidate branch:

| # | Path | Blob at `p0-v1.0.1` | Blob at `p1/design-candidate` | Result |
|---|---|---|---|---|
| 1 | `PRODUCT_CONSTITUTION.md` | `14be44bb39aba6ead1f482c2bb6cd9dc75131c8a` | `14be44bb39aba6ead1f482c2bb6cd9dc75131c8a` | UNCHANGED |
| 2 | `AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md` | `89ee2ccc239b15aafdf1daa9dc340f01fd6f782d` | `89ee2ccc239b15aafdf1daa9dc340f01fd6f782d` | UNCHANGED |
| 3 | `ARCHITECTURE.md` | `9e23964528e54c171960f921a8c91224ade2e319` | `9e23964528e54c171960f921a8c91224ade2e319` | UNCHANGED |
| 4 | `DERIVATION_MANIFEST.md` | `011905580ad818000f27b302d38291366165a316` | `011905580ad818000f27b302d38291366165a316` | UNCHANGED |
| 5 | `EVALUATION_CONTRACT.md` | `18aad0a857ec0a015c5b1b7aa57628956ba055e8` | `18aad0a857ec0a015c5b1b7aa57628956ba055e8` | UNCHANGED |
| 6 | `P0_BASELINE_MANIFEST.md` | `8ce679bfe96ad128a60a50b37ff9c916ca5c18f2` | `8ce679bfe96ad128a60a50b37ff9c916ca5c18f2` | UNCHANGED |
| 7 | `AI_BOOTSTRAP.md` | `98b8ef0dad18ea23213baf34be21e5fdb7924316` | `98b8ef0dad18ea23213baf34be21e5fdb7924316` | UNCHANGED |
| 8 | `.gitignore` | `864cb918005990c43726e19db60c0857632e98d2` | `864cb918005990c43726e19db60c0857632e98d2` | UNCHANGED |
| 9 | `docs/p0/**` | — | — | UNCHANGED (`git diff --name-status` empty) |
| 10 | `reviews/**` | — | — | UNCHANGED (`git diff --name-status` empty) |

Rows 1–5 are the five P0 **semantic** canonical documents. `README.md` is a bootstrap
support file (not canonical P0 content) and is intentionally extended in §3c.

```text
Accepted Baseline tag p0-v1.0.1      = NOT MOVED, NOT RECREATED, NOT FORCE-UPDATED
Historical tag p0-v1.0.0             = NOT MOVED, NOT DELETED
main                                 = NOT MODIFIED
p0/bootstrap-candidate               = NOT MODIFIED
```

---

# 5. Candidate Content Classification

```text
Content type:      DOCUMENTATION ONLY
Business code:     NONE
Runtime:           NONE
Database:          NONE
Migration:         NONE
Frontend impl.:    NONE
Delivery runtime:  NONE
Trading capability:NONE
Runtime AI:        NONE
```

The candidate adds documents only. It creates no `backend/`, `frontend/`, `src/`,
`app/`, `services/`, `database/`, `migrations/` or `tests/` directory, and no source or
configuration file of any implementation language.

---

# 6. Review Binding Rules

1. A review verdict binds **only** the exact candidate Git SHA it names.
2. Any semantic change to a canonical P1 document after review invalidates the prior
   verdict.
3. Mechanical state-pointer updates must be recorded with an exact diff and an
   applicability judgement.
4. The accepted P0 baseline (`p0-v1.0.1`) must never be presented as the P1 candidate,
   and the P1 candidate must never be presented as accepted or frozen.
5. `P1_DESIGN_MANIFEST.md` must not be marked `FROZEN` or `ACCEPTED` before P1-05.
6. Superseded candidate SHAs must not be reused as a review target.

---

# 7. Provenance

```text
Created by:   QI-P1-03-I01 Document-only P1 Design Candidate Persistence
Date:         2026-09-23
Scope:        persist / split / organize / cross-reference / verify / commit / push only
Business code created: NO
Design semantics changed: NO
```

---

# 8. Boundary

```text
P0                        = ACCEPTED
P1 Design                 = REVIEW CANDIDATE / NOT FROZEN
P1 Requirement            = NOT FROZEN
P1 Architecture           = NOT FROZEN
P1 Implementation         = NOT AUTHORIZED
Business Coding           = FORBIDDEN
Next                      = P1-04 Independent Requirement / Architecture Review
This document authorizes: NOTHING
```
