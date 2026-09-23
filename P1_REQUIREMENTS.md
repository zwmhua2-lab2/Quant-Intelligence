# Quant Intelligence — P1 REQUIREMENTS

```text
Document ID:                  QI-P1-DOC-REQUIREMENTS
Document Class:               Canonical P1 document — Requirement normative owner
Phase:                        P1 — First Observable Intelligence Loop
Status:                       DESIGN REVIEW CANDIDATE / NOT FROZEN
P1 Status:                    DESIGN
P1 Requirement:               NOT FROZEN
P1 Architecture:              NOT FROZEN
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
> input `QI-P1_Design_Input_v0.1.md`. It persists requirement semantics; it does **not**
> redesign them. Every normative clause below is traceable to a Design Input section
> (cited inline as `Design Input §N`). No new product semantic has been introduced.
>
> **Status note.** `NOT FROZEN`. This document is a review candidate only. It does
> **not** authorize P1 implementation.

---

# 1. Normative Ownership

This document is the normative owner of the **P1 product requirement**: goal, market
scope, observable behaviour, detector product semantics, evidence-breadth semantics,
decision responsibility boundary, evaluation eligibility, report / delivery / UI
product requirements, forward-outcome requirement, out-of-scope boundary, verification
modes and the P1 acceptance boundary.

It is **not** the normative owner of implementation architecture, technology selection,
storage schema, runtime worker topology, module layout or API transport shape. Those
are owned by:

```text
P1_ARCHITECTURE.md     — implementation architecture normative owner
P1_DATA_CONTRACT.md    — data / identity / formula / state-machine normative owner
P1_REUSE_MANIFEST.md   — derivation reuse decision normative owner
P1_IMPLEMENTATION_PLAN.md — batch planning only (NOT implementation authorization)
```

Where this document mentions architecture-adjacent facts (for example the single-process
constraint or the read-only API surface), it does so only as a **cross-reference** to
the requirement-level constraint; the implementation detail is owned elsewhere.

---

# 2. P1 Goal and Product Loop

> Source: Design Input §1.

P1 proves one thing: Quant Intelligence can

```text
consume real market data
detect a small set of explicit market behaviors
persist traceable evidence
make a deterministic publish / suppress decision
render a Simplified-Chinese report
deliver it through one external channel
expose it in UI / history
evaluate forward market outcomes
```

**without any trading semantics.**

Canonical core loop:

```text
Real Market Data
→ Feature / Market Structure
→ Detector
→ Market Event
→ Opportunity Episode
→ Evidence Version
→ Deterministic Decision
→ Deterministic Chinese Report
→ Delivery / UI / History
→ 5m / 15m Forward Outcome
```

P1 is explicitly `WITHOUT_AI BASELINE`. Runtime LLM / Deep AI belongs to P2.

**P1-R-01.** P1 must be an observable intelligence loop as described above, and must
contain no trading semantics.

**P1-R-02.** P1 is a `WITHOUT_AI BASELINE` deliverable. No runtime LLM, no runtime
Deep AI, no runtime model inference participates in any P1 decision or report.

---

# 3. Market Scope

> Source: Design Input §2.

```text
Exchange:    OKX only
Market type: USDT SWAP only
```

Default configurable allowlist:

```text
BTC-USDT-SWAP
ETH-USDT-SWAP
SOL-USDT-SWAP
```

**P1-R-03.** Startup must validate each instrument for: instrument exists,
`market_type=SWAP`, `quote_asset=USDT`, `tradable=true`.

**P1-R-04.** Any default instrument validation failure **disables that instrument** and
makes required-universe health `NOT READY`.

**P1-R-05.** P1 has **no dynamic Top100 / Hot30 universe**.

---

# 4. Required Market Inputs

> Source: Design Input §2.

Required inputs:

```text
Trade
Best Bid / Offer (BBO)
confirmed 1m Kline
Open Interest
Funding Rate
```

**P1-R-06.** All five inputs above are required for the P1 observable loop.

**P1-R-07.** Funding is **Evidence Context only** and is **never** an independent P1
Detector.

---

# 5. Detector Set

> Source: Design Input §4.

P1 implements **exactly three** Detector domains:

```text
1. STRUCTURE_BREAKOUT
2. TAKER_FLOW_IMBALANCE
3. OI_EXPANSION
```

**P1-R-08.** The P1 Detector set is exactly, and only, the three domains above. No
fourth Detector domain may be added to P1.

**P1-R-09.** The following are explicitly **out of scope** and must not be implemented,
reused, ported or reintroduced as P1 Detectors:

```text
FAILED_BREAKOUT
old VOLUME_ANOMALY / old relative_volume semantics
liquidation detector
relative BTC detector
liquidity-change detector
price-acceleration detector
generic volatility-anomaly detector
```

**P1-R-10.** Liquidity / spread may be **Evidence Context** but must not be an
independent P1 trigger.

---

# 6. Detector Product Semantics

> Source: Design Input §5, §6, §7, §8.

Formula-level values, defaults and identity rules are owned by `P1_DATA_CONTRACT.md`.
This section states the **product semantics** that the formulas must express.

## 6a. STRUCTURE_BREAKOUT

**P1-R-11.** STRUCTURE_BREAKOUT expresses a **price-structure breakout** of the
immediately preceding confirmed range, confirmed against a fresh best-bid/offer
midpoint.

**P1-R-12.** The current forming candle must **not** enter the prior range. A forming
candle that trades back inside the prior range does not constitute a breakout event.

**P1-R-13.** Direction is two-valued: `UP_BREAKOUT` / `DOWN_BREAKOUT`. Re-arm / reset is
**symmetric** when price returns inside the margin or when the structure version changes.

**P1-R-14.** Required Evidence for this detector includes: structure window
identity / version, `range_upper`, `range_lower`, breakout direction and reference
level, current price, breakout distance, `feature_as_of`, `detected_at`.

**P1-R-15.** `FAILED_BREAKOUT` is **not implemented** in P1.

## 6b. TAKER_FLOW_IMBALANCE

**P1-R-16.** TAKER_FLOW_IMBALANCE expresses **taker-side order-flow imbalance** measured
on **taker-side trade notional** — not on raw quantity-only reuse.

**P1-R-17.** Direction is two-valued: `BUY_PRESSURE` / `SELL_PRESSURE`.

**P1-R-18.** `BUY_PRESSURE` / `SELL_PRESSURE` describe **market behavior**, not trade
advice. They are not, and must not be rendered as, buy/sell recommendations.

**P1-R-19.** The detector has a minimum-validity requirement (sample count, total quote
notional, and trade recency) below which no event may be emitted.

**P1-R-20.** Required Evidence includes: window start / end, `trade_count`, buy notional,
sell notional, `imbalance`, `current_price`, `feature_as_of`, `detected_at`.

## 6c. OI_EXPANSION

**P1-R-21.** OI_EXPANSION expresses **open-interest expansion** relative to a causal
baseline sample.

**P1-R-22.** If no valid baseline exists, the detector is `WARMING_UP` / `INSUFFICIENT`
and must not emit a fabricated event.

**P1-R-23.** `Direction = NONE`. OI expansion alone is **not** bullish and **not**
bearish. OI_EXPANSION must never carry or imply a direction.

**P1-R-24.** Required Evidence includes: previous and current OI with their
`available_at` values, `oi_change`, price-change context, funding context,
`feature_as_of`, `detected_at`.

## 6d. Detector Execution Semantics

**P1-R-25.** Detector cycle = 1 second per instrument, computed from **one internally
consistent Market State Snapshot**.

**P1-R-26.** Each detector / instrument pair has `INACTIVE` / `ACTIVE` state and emits
**only on the `INACTIVE→ACTIVE` transition**, to prevent per-second duplicate spam.

---

# 7. Evidence Breadth / Quality Semantics

> Source: Design Input §10.

**P1-R-27.** Quality tier is **evidence breadth**, not probability:

```text
Q1_SINGLE_DOMAIN   = 1 detector domain
Q2_CROSS_DOMAIN    = 2 domains
Q3_MULTI_DOMAIN    = Structure + Flow + Derivatives
```

**P1-R-28.** The UI label must communicate `证据广度` (**evidence breadth**) and must
**not** communicate a win rate.

**P1-R-29.** Direction coherence is three-valued: `COHERENT`, `MIXED`, `NOT_APPLICABLE`.

**P1-R-30.** Only Structure and Flow participate in directional coherence. **OI never
votes direction.** `Q3` does **not** imply coherent direction.

**P1-R-31.** `MIXED` evidence is **not** automatically suppressed. The report must
explicitly show the conflict.

**P1-R-32 — the four-way separation (normative).** The following are distinct concepts
and must never be conflated in data, UI, report or API:

```text
Event Strength
≠ Processing Priority
≠ Evidence Breadth / Quality Tier
≠ Confidence Probability
```

**P1-R-33.** `Confidence Probability` is `NOT IMPLEMENTED` in P1. P1 must not emit,
store, display or imply a probability, win rate, expected value or calibrated
confidence for any Market Event, Episode, Evidence version, Decision or Report.

---

# 8. Decision Responsibilities and Boundaries

> Source: Design Input §11.

**P1-R-34.** The Decision Core is **deterministic only**. It must contain no runtime AI,
no LLM, no stochastic component and no external model call.

**P1-R-35.** The Decision Core responsibilities are exactly:

```text
Evidence validity
causal cutoff
freshness
candidate eligibility
dedupe
quality tier
processing priority
publish / suppress
```

**P1-R-36.** Decision values are only `PUBLISH` / `SUPPRESS`.

**P1-R-37.** Decision reason codes include `PUBLISH_Q1` / `PUBLISH_Q2` / `PUBLISH_Q3`,
`PRIORITY_BELOW_GATE`, `STALE_INPUT`, `INVALID_EVIDENCE`, `DUPLICATE`. A suppression
below the publication gate is recorded as `DETERMINISTIC_REJECTED` /
`PRIORITY_BELOW_GATE`.

**P1-R-38.** The Decision Core must not produce, and this document must not require, any
of:

```text
Trade Plan
Order Intent
Position Plan
Capital Allocation
Trading Risk Admission
```

---

# 9. Evaluation Eligibility

> Source: Design Input §11.

**P1-R-39.** Evaluation eligibility is assigned **before** future outcome and **before**
the publication result is known.

**P1-R-40.** Default `eligible=true` when Evidence is structurally valid, non-duplicate,
and market-data coverage is valid.

**P1-R-41.** Publication / suppression must **not** change eligibility retrospectively.
Evaluation eligibility is immutable after assignment.

---

# 10. Report Requirements

> Source: Design Input §12.

**P1-R-42.** Report language is **Simplified Chinese**. Template version is
`P1_ZH_REPORT_V1`. There is **no LLM** in report generation.

**P1-R-43.** The report contains: symbol, time, primary behavior, corroborating behavior,
evidence breadth, priority, structure, taker flow, OI / funding, direction coherence,
data quality, known / unknown / conflict, IDs / versions.

**P1-R-44.** The report must **not** contain any buy / sell recommendation, long / short
instruction, entry, stop loss, take profit, leverage, or position sizing.

**P1-R-45.** Reports are **immutable versions**. A new publishable Evidence revision
creates a **new `report_version`**. An existing report version is never edited in place.

---

# 11. Delivery Product Requirement

> Source: Design Input §12.

**P1-R-46.** The **only** P1 external delivery channel is **Telegram Bot via HTTPS Bot
API `sendMessage`**.

**P1-R-47.** P1 has **no inbound webhook, no command bot, no inbound processing**.

**P1-R-48.** Secrets come **only** from the environment:

```text
QI_TELEGRAM_BOT_TOKEN
QI_TELEGRAM_CHAT_ID
```

Secrets must **not** enter config, database, logs or reports.

**P1-R-49.** One **automatic** delivery attempt per report version. There is **no
automatic retry**, because a timeout can leave remote acceptance unknown. Delivery
result is recorded as `CONFIRMED` / `FAILED` / `UNKNOWN`.

**P1-R-50.** Notification is sent **only** for the first published report or for a
quality-tier upgrade (`Q1→Q2`, `Q2→Q3`). Same-tier revision updates UI / history only.

> **Execution boundary for this task.** QI-P1-03-I01 must **not** connect to Telegram or
> perform any delivery. This section persists the requirement only.

---

# 12. UI Scope

> Source: Design Input §20.

**P1-R-51.** The UI has exactly three pages:

```text
/                   — 实时机会
/opportunities/:id  — 机会详情
/history            — 历史与 Outcome
```

**P1-R-52.** The UI is read-only with respect to decisions, thresholds and outcomes.
There is **no mutation UI** for trade / order / position / decision / threshold / retry.

**P1-R-53.** The UI must display evidence breadth using the `证据广度` semantics of
P1-R-28, and must not display any probability or win-rate framing.

---

# 13. Forward Outcome Requirement

> Source: Design Input §13.

**P1-R-54 — dual Outcome Anchor (normative).**

```text
Every evaluation-eligible Opportunity
  → DETECTION_ANCHOR scopes at 5m and 15m

If a report is published, additionally
  → REPORT_PUBLICATION_ANCHOR scopes at 5m and 15m
```

**P1-R-55.** Anchor types remain **separate**. Detection-anchored and
publication-anchored outcomes must never be merged into a single result.

**P1-R-56.** `due_at = anchor_time + horizon_seconds` and **never moves** due to retry or
restart.

**P1-R-57.** The reference is a **fresh legal BBO midpoint** with
`available_at <= anchor_time` and age `<= 2s`, captured durably.

**P1-R-58.** The outcome endpoint is the greatest `PATH_SAMPLE` `available_at` with
`anchor_time < available_at <= due_at`; the endpoint must be no more than 5 seconds
before `due_at`, otherwise `status = COVERAGE_GAP`.

**P1-R-59.** No data with `available_at > due_at` may backfill the original outcome.

**P1-R-60.** Missing / unavailable / immature outcomes must **not** be coerced to zero
return or to failure.

**P1-R-61.** Direction consistency is reported only when the primary event has a natural
direction. `OI_EXPANSION` primary ⇒ `NOT_APPLICABLE`.

**P1-R-62.** Outcome computations contain **no** fills, fees, slippage, leverage,
position sizing, or account / portfolio PnL.

---

# 14. P1 Out of Scope

> Source: Design Input §1, §2, §4, §11, §12, §13, §17, §18, §20.

The following are **out of scope for P1** and must not appear as P1 capabilities:

**14a. Trading and economic capability (explicit exclusion).**

```text
fills
fees
slippage
position
portfolio PnL
paper trading
execution
trading risk admission
Trade Plan / Order Intent / Position Plan / Capital Allocation
buy / sell recommendation, long / short instruction
entry, stop loss, take profit, leverage, position sizing
```

**14b. AI capability.**

```text
Runtime LLM
Runtime Deep AI
model inference in Decision or Report
```

**14c. Detector scope.**

```text
FAILED_BREAKOUT
old VOLUME_ANOMALY / old relative_volume
liquidation detector
relative BTC detector
liquidity-change detector
price-acceleration detector
generic volatility-anomaly detector
funding as an independent detector
liquidity / spread as an independent trigger
```

**14d. Universe scope.**

```text
dynamic Top100 / Hot30 universe
any exchange other than OKX
any market type other than USDT SWAP
```

**14e. Platform / infrastructure scope.**

```text
PostgreSQL
Redis
Kafka
Celery
microservices
Docker requirement
distributed queue
multi-agent runtime
frontend WebSocket
LAN / Internet exposure
```

**14f. Capability scope.**

```text
mutation API for trade / order / position / decision / threshold / retry
inbound Telegram webhook / command bot / inbound processing
Confidence Probability
```

**14g. Repository domain scope.**

```text
trading
orders
positions
risk
execution
portfolio
```

These directory / domain names are **forbidden** in the P1 implementation layout.

---

# 15. Verification Modes

> Source: Design Input §22.

**P1-R-63.** P1 must be verified in four modes:

```text
1. Unit tests
   canonicalization, IDs, detector formulas, priority, quality tier,
   episode correlation, Decision, Outcome math

2. Deterministic integration
   fixed Trade + BBO + confirmed Kline + OI stream through
   Event → Evidence → Decision → Report → Outcome

3. Persistence / restart
   real temporary SQLite DB, commit / restart / resume,
   no duplicate report, no due_at drift

4. Live smoke
   real OKX instrument metadata / Public WS / Candle WS / OI / Funding / health
```

**P1-R-64.** Live acceptance must **not** lower production thresholds merely to
manufacture an Opportunity. Detector correctness is proven by **deterministic
fixtures**.

---

# 16. P1 Acceptance Boundary

> Source: Design Input §23. Reproduced faithfully; order preserved.

P1 acceptance requires **at least**:

```text
 1. real OKX USDT SWAP feed chain
 2. BTC / ETH / SOL allowlist
 3. three Detector semantics frozen / tested
 4. no FAILED_BREAKOUT / old VOLUME_ANOMALY reuse
 5. causal timestamps enforced
 6. deterministic identity / revision
 7. Evidence reconstructs Decision input
 8. deterministic Decision Core
 9. Q1 / Q2 / Q3 evidence breadth semantics
10. no Confidence Probability
11. deterministic Chinese report
12. one real Telegram delivery channel
13. three Chinese UI pages
14. History with original immutable records
15. 5m / 15m Outcome scopes
16. Detection / Publication anchors separated
17. honest missing / coverage handling
18. restart idempotency and fixed due_at
19. fail-closed health
20. no trading / economic capability
21. no Runtime AI
22. live OKX smoke evidence
23. deterministic end-to-end verification
```

This acceptance boundary must **not** be weakened. No clause may be dropped, relaxed or
silently deferred.

---

# 17. P1 Value Evidence

> Source: Design Input §1, §13, §22, §23, §24.
>
> **Consolidation note.** This section **consolidates clauses already stated** in the
> sections cited above. It introduces **no additional** semantic requirement.

P1 must produce evidence that the observable intelligence loop works, in the following
forms — all of them derived from clauses already stated:

**P1-R-65.** Forward outcome measurement exists for evaluation-eligible Opportunities at
5m and 15m, under both anchor types, with honest `IMMATURE` / `MEASURABLE` /
`UNAVAILABLE` / `COVERAGE_GAP` status rather than coerced zeros. (Design Input §13)

**P1-R-66.** The Decision input set is reconstructible from persisted Evidence. Evidence
reconstructs the Decision input. (Design Input §23 item 7)

**P1-R-67.** The full chain is reproducible deterministically from fixed inputs, and
detector correctness is proven by deterministic fixtures rather than by manufacturing
live Opportunities. (Design Input §22)

**P1-R-68.** Live OKX smoke evidence exists for real instrument metadata, Public WS,
Candle WS, OI, Funding and health. (Design Input §22, §23 item 22)

**P1-R-69.** Restart idempotency is evidenced: no duplicate reports, no `due_at` drift,
mature pending outcome scopes resume. (Design Input §15, §23 item 18)

**P1-R-70.** Missing / coverage behaviour is evidenced as honest status, not as silent
success. (Design Input §13, §14, §23 item 17)

---

# 18. Traceability Index

| Requirement block | Design Input source |
|---|---|
| §2 Goal and Product Loop | §1 |
| §3 Market Scope | §2 |
| §4 Required Market Inputs | §2 |
| §5 Detector Set | §4 |
| §6a STRUCTURE_BREAKOUT | §5 |
| §6b TAKER_FLOW_IMBALANCE | §6 |
| §6c OI_EXPANSION | §7 |
| §6d Detector Execution | §8 |
| §7 Evidence Breadth / Quality | §10 |
| §8 Decision Responsibilities | §11 |
| §9 Evaluation Eligibility | §11 |
| §10 Report Requirements | §12 |
| §11 Delivery Product Requirement | §12 |
| §12 UI Scope | §20 |
| §13 Forward Outcome Requirement | §13 |
| §14 P1 Out of Scope | §1, §2, §4, §11, §12, §13, §17, §18, §20 |
| §15 Verification Modes | §22 |
| §16 P1 Acceptance Boundary | §23 |
| §17 P1 Value Evidence | §1, §13, §22, §23, §24 |

Formula-level values, defaults, identity rules, state machines and persistence entities
are owned by `P1_DATA_CONTRACT.md`. Runtime topology, technology baseline, module layout,
health model and security boundary are owned by `P1_ARCHITECTURE.md`.

---

# 19. Boundary

```text
P0                        = ACCEPTED
P1 Requirement            = NOT FROZEN
P1 Architecture           = NOT FROZEN
P1 Implementation         = NOT AUTHORIZED
Business Coding           = FORBIDDEN
This document authorizes: NOTHING
```
