# Quant Intelligence — P1 REUSE MANIFEST

```text
Document ID:                  QI-P1-DOC-REUSE-MANIFEST
Document Class:               Canonical P1 document — Derivation reuse decision normative owner
Phase:                        P1 — First Observable Intelligence Loop
Status:                       DESIGN REVIEW CANDIDATE / NOT FROZEN
P1 Status:                    DESIGN
Reuse Decisions:              NOT FROZEN
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
> input (Design Input §21, plus the P0 carry-over exclusions). It persists the reuse
> decisions; it does **not** redesign them.
>
> **Status note.** `NOT FROZEN`. Review candidate only. No implementation authorization.

---

# 1. Pinned Derivation Source

> Source: Design Input §21; P0 Baseline Manifest §2.

```text
Repository:  zwmhua2-lab2/MarketPulse-AI
Pinned SHA:  7a023f99ac44ac744e5a9885fd82f8a87ef51a94
```

**P1-U-01.** This is a **pinned derivation source only**. Quant Intelligence is an
independent project and MarketPulse-AI is **not modified** by P1.

**P1-U-02 — no wholesale copy (normative).** Do not copy MarketPulse wholesale, and do
not import convenience dependencies that would reintroduce excluded trading semantics.

**P1-U-03.** Reuse decisions are three-valued in intent — adapt (subset), adapt
(concept), reimplement from contract — plus an explicit exclusion class. Every item
below belongs to exactly one class, and the class is normative.

---

# 2. ADAPT-SUBSET

> Source: Design Input §21.

Files that may be adapted as a **subset** of their source behaviour:

```text
clock.py
ids.py
edge/hashing.py + g5/canonical.py   (as ONE canonical / hash safety unit)
runtime.py                          (worker supervision / health pattern ONLY)
```

**P1-U-04.** `edge/hashing.py` and `g5/canonical.py` must be treated as **one**
canonical / hash safety unit. They must not be partially adopted in a way that separates
canonicalization from hashing.

**P1-U-05.** `runtime.py` may be adapted for its **worker supervision / health pattern
only**. Its health semantics remain superseded by the P1 fail-closed health requirement
(cross-ref `P1_ARCHITECTURE.md` P1-A-23 / P1-A-24).

---

# 3. ADAPT-CONCEPT

> Source: Design Input §21.

Concepts that may be adapted, with the stated constraint:

```text
market_data/models.py
market_data/normalizer.py        — received / available time explicitly injected;
                                   NO implicit datetime.now causality
market_data/okx_live.py          — split Public WS / Business WS / REST context
edge/forward.py                  — causal due / endpoint / coverage CONCEPTS ONLY
edge/forward_service.py          — durable scheduling / restart CONCEPTS ONLY
FastAPI structure
React / TS / Vite engineering foundation
```

**P1-U-06 — no implicit causality (normative).** `market_data/normalizer.py` must have
received / available time **explicitly injected**. Implicit `datetime.now` causality is
**prohibited**. This is a hard constraint of the causal time contract (cross-ref
`P1_DATA_CONTRACT.md` §2).

**P1-U-07.** `edge/forward.py` and `edge/forward_service.py` are concept sources
**only** — causal due / endpoint / coverage concepts and durable scheduling / restart
concepts respectively. Their implementations are not reused as source files.

---

# 4. REIMPLEMENT FROM CONTRACT

> Source: Design Input §21.

Must be **reimplemented from contract**, not adapted:

```text
g2/rolling.py
g2/structure.py
g2/scanner.py
```

**P1-U-08.** These three files must be reimplemented from the P1 contract
(`P1_DATA_CONTRACT.md` §5, §4, §6). Their existing source semantics are **not** portable
as-is.

**P1-U-09.** This reimplementation requirement applies in particular to the detector
formulas: the P1 detector semantics are defined by this design, not by the source files.

---

# 5. EXCLUDE AS SOURCE FILE / DEPENDENCY

> Source: Design Input §21; P0 Baseline Manifest §5.

The following are **excluded** — neither reused as source files nor imported as
dependencies:

```text
g2/models.py
runtime_host.py
edge/attribution.py
g3/*
g4/*
PaperExecutionService
Order / Fill domain
PositionRecord
TradePlanRecord
PositionProtectionService
SystemRiskGate
Economic Recovery
Trading Ledger
```

**P1-U-10 — explicit exclusion (normative).** The exclusion list above is absolute. None
of these items may be reintroduced directly or indirectly, including through a
transitive convenience dependency.

**P1-U-11 — trading / economic exclusion mapping (normative).** The excluded items map
onto P1's forbidden capability classes as follows:

| Excluded item | Forbidden capability class |
|---|---|
| `PaperExecutionService` | paper trading / execution |
| `Order / Fill domain` | orders / fills |
| `PositionRecord` | positions |
| `TradePlanRecord` | trade plan |
| `PositionProtectionService`, `SystemRiskGate` | trading risk admission |
| `Economic Recovery`, `Trading Ledger` | trading economics / ledger |
| `g3/*`, `g4/*`, `g2/models.py`, `runtime_host.py`, `edge/attribution.py` | excluded source files |

---

# 6. Carry-over Source Limitations

> Source: P0 Baseline Manifest §5; `DERIVATION_MANIFEST.md` (KSL-01, KSL-02, M-06).

```text
FAILED_BREAKOUT                       = NOT DIRECTLY PORTABLE
old relative_volume / VOLUME_ANOMALY  = NOT DIRECTLY PORTABLE
runtime_host.py                       = EXCLUDE AS SOURCE FILE
edge/attribution.py                   = EXCLUDE
g3/*                                  = EXCLUDE
g4/*                                  = EXCLUDE
```

**P1-U-12.** These P0 carry-over limitations remain in force in P1 and are consistent
with the P1 detector set restriction (cross-ref `P1_REQUIREMENTS.md` §5).

---

# 7. Reuse Boundary Summary

```text
ADAPT-SUBSET              4 entries  (hashing + canonical counted as one unit)
ADAPT-CONCEPT             7 entries
REIMPLEMENT FROM CONTRACT 3 entries
EXCLUDE                   13 entries
```

**P1-U-13.** No reuse class may be broadened without a data-contract / requirement change
and a new review.

**P1-U-14.** MarketPulse-AI must not be modified, and no MarketPulse artifact may be
presented as the Quant Intelligence baseline.

---

# 8. Traceability Index

| Reuse block | Design Input source |
|---|---|
| §1 Pinned Derivation Source | §21 |
| §2 ADAPT-SUBSET | §21 |
| §3 ADAPT-CONCEPT | §21 |
| §4 REIMPLEMENT FROM CONTRACT | §21 |
| §5 EXCLUDE AS SOURCE FILE / DEPENDENCY | §21 |
| §6 Carry-over Source Limitations | P0 Baseline Manifest §5 / DERIVATION_MANIFEST |

---

# 9. Boundary

```text
P0                        = ACCEPTED
P1 Reuse Decisions        = NOT FROZEN
P1 Implementation         = NOT AUTHORIZED
Business Coding           = FORBIDDEN
MarketPulse-AI            = NOT MODIFIED
This document authorizes: NOTHING
```
