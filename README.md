# Quant Intelligence

Document-only P0 bootstrap repository for the **Quant Intelligence** project.

---

## Status

```text
Phase:                              P0 Accepted Baseline / P1 Design Next
P0 Status:                          ACCEPTED
Business Implementation Authorization: NONE
Repository Content Type:            DOCUMENTATION ONLY
Business / Runtime Code:            NONE
Trading Capability:                 NONE
```

This repository currently contains **no business implementation code**, no runtime,
no database schema/migration, no market feed runtime, no AI provider runtime, no
notification runtime and no trading capability of any kind.

---

## P0 accepted — this is not an implementation authorization

`P0` is accepted as the project constitution / governance / architecture baseline.

This does **not** authorize business implementation.
`P1` must first complete Requirement / Task Design and freeze a Formal Task.

---

## Derivation source

`MarketPulse-AI @ 7a023f99ac44ac744e5a9885fd82f8a87ef51a94`

This is a **pinned derivation source only**. Quant Intelligence is an independent
project: it has its own repository, namespace, database/schema, baseline,
architecture, governance and roadmap. MarketPulse history is **not** merged, and
MarketPulse is **not** modified by this repository.

---

## Where the canonical documentation lives

Canonical documents now live on `main`.

```text
main         — accepted P0 baseline
p0-v1.0.1    — authoritative accepted baseline tag
```

The historical candidate branch `p0/bootstrap-candidate` may remain for provenance.

Read, in order:

```text
1. AI_BOOTSTRAP.md
2. PROJECT_STATE.md
3. PRODUCT_CONSTITUTION.md
4. AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md
5. ARCHITECTURE.md
6. DERIVATION_MANIFEST.md
7. EVALUATION_CONTRACT.md
8. ROADMAP.md
9. P0_BASELINE_MANIFEST.md
```

Authoritative precedence:

```text
Repository Truth > Chat History > Memory > Worker Report
```

---

## P1 design candidate — review pending, not frozen, not authorized

The P0 accepted baseline is frozen and unchanged. The P1 design candidate lives on its
own branch and does **not** advance the accepted baseline.

```text
P1 Candidate Branch:  p1/design-candidate
P1 Design Input:      docs/p1/QI-P1_Design_Input_v0.1.md
P1 Status:            DESIGN  (design candidate persisted / review pending)
P1 Requirement:       NOT FROZEN
P1 Architecture:      NOT FROZEN
P1 Implementation:    NOT AUTHORIZED
Business Coding:      FORBIDDEN
Next:                 P1-04 Independent Requirement / Architecture Review
```

The `Status` block at the top of this file describes the P0 accepted baseline and
predates the P1 design candidate. The P1 status above is the current one; the
authoritative current-state pointers are `PROJECT_STATE.md` §7 and `ROADMAP.md` Part G.

Read the P1 design candidate in this order:

```text
1. docs/p1/README.md
2. docs/p1/QI-P1_Design_Input_v0.1.md
3. P1_REQUIREMENTS.md
4. P1_ARCHITECTURE.md
5. P1_DATA_CONTRACT.md
6. P1_REUSE_MANIFEST.md
7. P1_IMPLEMENTATION_PLAN.md
8. P1_DESIGN_MANIFEST.md
```

A persisted design candidate is **not** a frozen design and does **not** authorize
implementation.
