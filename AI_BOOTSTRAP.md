# Quant Intelligence — AI BOOTSTRAP

```text
Document ID:        QI-P0-DOC-AI-BOOTSTRAP
Status:             ACCEPTED
Phase:              P0 — Accepted Baseline
P0 Status:          ACCEPTED
Business Implementation Authorization: NONE
```

> **This is the entry point.** If you are an AI agent entering the Quant Intelligence
> repository for the first time, read this file first, then follow the reading order below.

---

# 1. Read in this order

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

Supplemental:

```text
docs/p0/     — the raw P0 source candidate artifact (byte-preserved, do not reformat)
reviews/     — independent review registry and findings-closure records
```

---

# 2. Authoritative precedence

```text
Repository Truth > Chat History > Memory > Worker Report
```

Repository Truth wins. If a chat message, a memory note or a worker self-report contradicts
what this repository actually contains, the repository wins, and the contradiction must be
reported rather than silently resolved.

---

# 3. Two boundaries that must not be misread

```text
P0 NOT ACCEPTED
Business Implementation Authorization = NONE
```

Concretely, at this state:

- `P0` is a **review candidate**, not an accepted baseline.
- The final acceptance target is a **Repository Candidate Git SHA** that has passed an
  independent **GPT-6 Final P0 Re-Review**.
- Business coding is **NOT authorized**. There is no authorized backlog to execute.
- `QI-P0-R01` returned `FIX REQUIRED`. Its five blocking findings are recorded as
  `DESIGN CLOSED / RE-REVIEW REQUIRED`, not as reviewed-closed.

---

# 4. What this repository is

- An **independent** project repository for Quant Intelligence.
- **Documentation only** during P0: no business code, no runtime, no database schema, no
  market feed, no AI provider runtime, no notification runtime, no trading capability.

## What it is not

- It is **not** a fork or copy of MarketPulse. MarketPulse is only a pinned derivation
  source; its history is not merged here and it is not modified by this repository.

```text
MarketPulse Derivation Source:
  zwmhua2-lab2/MarketPulse-AI @ 7a023f99ac44ac744e5a9885fd82f8a87ef51a94
```

---

# 5. Hard boundaries you must respect

## Trading boundary

The product is an **AI-assisted Quant Opportunity Intelligence System**. The following are
**excluded** by the Constitution: automatic trading, paper trading, live trading,
order/fill execution, position management, stop-loss / take-profit execution, position
protection, trading-risk admission, account balance management, trading ledger, execution
recovery, economic reconciliation.

They must not be reintroduced through renaming or implicit dependency. Adding them requires
`NEEDS_HUMAN_DECISION`.

## Evidence boundary

Every analysis must bind the evidence actually observable at the time. Never let later,
late, or revised data rewrite what the system saw. Never treat report-publication outcome
as if it were detection outcome, or vice versa.

## Governance boundary

```text
Worker PASS != Acceptance
max automatic Coding attempts per Problem Family = 5
```

After the budget is exhausted: `BLOCKED + ESCALATION REQUIRED`. AI must not continue coding
automatically, and must not self-approve an exception. Only C-10 Human-owned boundary
changes go to Human.

---

# 6. If you intend to review this candidate

An independent review must bind the **exact Candidate Git SHA** recorded in
`P0_BASELINE_MANIFEST.md`. A PASS is valid only for that tuple. Any semantic change to
P0 canonical content after the review invalidates the previous PASS.

Read the canonical files from the repository at that SHA — not from a chat summary, not
from a worker report.

---

# 7. Source limitations you must not forget

```text
FAILED_BREAKOUT                        = NOT DIRECTLY PORTABLE
relative_volume / VOLUME_ANOMALY       = NOT DIRECTLY PORTABLE
runtime_host.py                        = EXCLUDE AS SOURCE FILE
edge/attribution.py                    = EXCLUDE
g3/*                                   = EXCLUDE
g4/*                                   = EXCLUDE
```

Full detail in `DERIVATION_MANIFEST.md`.
