# Quant Intelligence — P1 DATA CONTRACT

```text
Document ID:                  QI-P1-DOC-DATA-CONTRACT
Document Class:               Canonical P1 document — Data / identity / formula normative owner
Phase:                        P1 — First Observable Intelligence Loop
Status:                       DESIGN REVIEW CANDIDATE / NOT FROZEN
P1 Status:                    DESIGN
P1 Data Contract:             NOT FROZEN
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
> input. It persists data-contract semantics; it does **not** redesign them. Every clause
> is traceable to a Design Input section (`Design Input §N`).
>
> **Status note.** `NOT FROZEN`. Review candidate only. No implementation authorization.

---

# 1. Normative Ownership

This document is the normative owner of P1 **data semantics**: causal timestamps,
identity construction, canonicalization and hashing, detector formulas and defaults,
event state machines, episode / evidence versioning, quality / direction / priority
derivations, evaluation eligibility, decision contract, report versioning, delivery
attempt records, BBO observation persistence, outcome scope / result semantics,
coverage and missing-data semantics, persistence entities and retention.

It is not the owner of product-level requirement semantics (see
`P1_REQUIREMENTS.md`) nor of process / technology / topology decisions (see
`P1_ARCHITECTURE.md`).

**Numeric values in this document are the Design Input's stated defaults.** Changing any
of them is a data-contract change and is outside the scope of a persistence task.

---

# 2. Causal Time Contract

> Source: Design Input §3.

**P1-D-01 — required timestamps (normative).** External facts must preserve at least:

```text
source_event_time
received_at
available_at
feature_as_of
detected_at
evidence_at
decision_input_cutoff
decision_at
report_published_at
delivery_requested_at
delivery_confirmed_at
```

**P1-D-02 — causal consumption rule (normative).**

```text
A fact may be consumed only when available_at <= decision_input_cutoff.
```

**P1-D-03 — `available_at` derivation by source (normative).**

```text
Trade / BBO          available_at = actual receive time after normalization
Confirmed Kline      legally consumable only after the confirmed frame is actually
                     received; open_time / close_time are NOT information-availability
                     timestamps
OI / Funding (REST)  available_at = successful response receive time
```

**P1-D-04 — revision, never rewrite (normative).** Late or revised data creates a **new
revision**. It **never** rewrites the original Decision's information set.

**P1-D-05 — timezone discipline.** All datetimes are UTC and timezone-aware.
Timezone-naive datetimes are invalid (see P1-D-07).

---

# 3. Canonicalization / Hashing Safety Unit

> Source: Design Input §8.

**P1-D-06 — canonicalization safety unit (normative).** The unit comprises:

```text
canonical value normalization
recursive non-finite rejection
deterministic UTF-8 JSON bytes
domain-separated SHA-256
UTC timezone-aware datetimes only
```

**P1-D-07 — invalid inputs (normative).**

```text
NaN
±Infinity
timezone-naive datetimes
unsupported arbitrary objects
```

are **invalid**. They must be rejected, not coerced, and must never reach a hash.

**P1-D-08.** The canonical / hash safety unit is a single unit: canonicalization and
hashing must not be split in a way that allows a non-canonical value to be hashed.

---

# 4. Market Event Identity

> Source: Design Input §8.

**P1-D-09 — Event ID format (normative).**

```text
qi_evt_<sha256>
Hash domain:  QI_EVENT_V1
Hash inputs:  instrument_id
              detector_type
              event_direction
              feature_snapshot_hash
              detector_config_version
```

**P1-D-10 — determinism (normative).** Same facts replayed must yield an **identical**
`event_id`.

**P1-D-11 — event state machine (normative).**

```text
Each detector / instrument pair has INACTIVE / ACTIVE state.
Emission occurs ONLY on the INACTIVE→ACTIVE transition.
```

This is the per-second duplicate-spam guard.

**P1-D-12 — detector cycle.** Detector cycle = **1 second per instrument**, computed from
**one internally consistent Market State Snapshot**.

---

# 5. Detector Formulas and Defaults

> Source: Design Input §5, §6, §7.

## 5a. STRUCTURE_BREAKOUT

```text
Structure version:   STRUCTURE_20X1M_V1
Inputs:              previous 20 confirmed 1m bars
                     latest fresh BBO midpoint
range_upper        = max(high of previous 20 confirmed bars)
range_lower        = min(low of previous 20 confirmed bars)
breakout_margin    = 0.001            (10 bps)
UP_BREAKOUT        when mid_price > range_upper * (1 + 0.001)
                     for 2 consecutive detector cycles
DOWN_BREAKOUT      when mid_price < range_lower * (1 - 0.001)
                     for 2 consecutive detector cycles
Re-arm / reset     symmetric when price returns inside the margin,
                     or when structure version changes
UP distance        = mid_price / range_upper - 1
DOWN distance      = 1 - mid_price / range_lower
strength           = clamp((distance - 0.001) / (0.005 - 0.001), 0, 1)
```

**P1-D-13.** The current forming candle must not enter the prior range.

**P1-D-14.** Required Evidence includes: structure window identity / version,
`range_upper`, `range_lower`, breakout direction and reference level, current price,
breakout distance, `feature_as_of`, `detected_at`.

**P1-D-15.** `FAILED_BREAKOUT` is **not implemented**.

## 5b. TAKER_FLOW_IMBALANCE

```text
Basis:               taker-side trade notional (NOT raw quantity-only reuse)
Window:              30 seconds
Minimum validity:    trade_count          >= 30
                     total_quote_notional >= 100,000 USDT
                     last_trade_age       <= 5 seconds

buy_notional       = sum(price * quantity) where taker side = BUY
sell_notional      = sum(price * quantity) where taker side = SELL
imbalance          = (buy_notional - sell_notional) / (buy_notional + sell_notional)

BUY_PRESSURE       when imbalance >= +0.60
SELL_PRESSURE      when imbalance <= -0.60
Re-arm             only after abs(imbalance) < 0.45 for 5 consecutive detector cycles
strength           = clamp((abs(imbalance) - 0.60) / 0.40, 0, 1)
```

**P1-D-16.** Required Evidence includes: window start / end, `trade_count`,
buy notional, sell notional, `imbalance`, `current_price`, `feature_as_of`,
`detected_at`.

## 5c. OI_EXPANSION

```text
OI poll interval:        15s
Funding poll interval:   60s
Baseline lookback:       300s
Baseline sample rule:    baseline sample must have
                           available_at <= current.available_at - 300s
                         nearest available sample is allowed with
                           timing error <= 30s
No baseline:             WARMING_UP / INSUFFICIENT
current OI age:          must be <= 45s
oi_change              = (current_oi - baseline_oi) / baseline_oi
Trigger:                 oi_change >= 0.01
strength               = clamp((oi_change - 0.01) / (0.05 - 0.01), 0, 1)
Direction:               NONE
```

**P1-D-17.** OI expansion alone is **not** bullish and **not** bearish. `Direction = NONE`
is normative, not a default.

**P1-D-18.** Required Evidence includes: previous and current OI with their `available_at`
values, `oi_change`, price-change context, funding context, `feature_as_of`,
`detected_at`.

---

# 6. Opportunity Episode / Evidence Revision

> Source: Design Input §9.

**P1-D-19 — episode correlation (normative).**

```text
episode_gap = 120s
Same instrument + new event within 120s  => joins the current Episode
Otherwise                                => create a new Episode
Different instruments                    => never merge
```

**P1-D-20 — Opportunity identity (normative).**

```text
Opportunity ID: qi_opp_<hash(instrument_id + first_event_id)>
```

**P1-D-21 — primary event immutability (normative).** The first accepted Market Event
remains the immutable `primary_event`. Later events are `corroborating_events`.

**P1-D-22 — Evidence versioning (normative).**

```text
Evidence starts at version 1.
A newly corroborating detector domain creates a new immutable evidence version.
Old Evidence is never overwritten.
```

**P1-D-23.** Evidence versions and report versions are immutable. Revision is always
additive.

---

# 7. Quality Tier / Direction Coherence / Processing Priority

> Source: Design Input §10.

## 7a. Quality tier — evidence breadth

```text
Q1_SINGLE_DOMAIN = 1 detector domain
Q2_CROSS_DOMAIN  = 2 domains
Q3_MULTI_DOMAIN  = Structure + Flow + Derivatives
```

**P1-D-24.** Quality tier is **evidence breadth**, not probability. The UI label must
communicate `证据广度`, not win rate.

## 7b. Direction coherence

```text
COHERENT
MIXED
NOT_APPLICABLE
```

**P1-D-25.** Only Structure and Flow participate in directional coherence. **OI never
votes direction.** `Q3` does not imply coherent direction.

## 7c. Processing priority

```text
magnitude_score = max(event_strength)
breadth_score   = Q1: 0    Q2: 0.5    Q3: 1
freshness_score = clamp(1 - latest_event_age_seconds / 10, 0, 1)
priority        = round(100 * (0.50 * magnitude + 0.35 * breadth + 0.15 * freshness))
```

**P1-D-26 — publication gate (normative).**

```text
Q1 publish if priority >= 65
Q2 publish if priority >= 50
Q3 publish if priority >= 40
otherwise DETERMINISTIC_REJECTED / PRIORITY_BELOW_GATE
```

**P1-D-27.** `Processing Priority` is **separate** from quality and from probability:

```text
Event Strength
≠ Processing Priority
≠ Evidence Breadth / Quality Tier
≠ Confidence Probability
```

**P1-D-28.** `MIXED` evidence is **not** automatically suppressed; the report must
explicitly show the conflict.

**P1-D-29.** `Confidence Probability` is `NOT IMPLEMENTED` in P1. No probability,
win rate, expected value or calibrated confidence field may exist in any P1 entity,
API payload or report.

---

# 8. Evaluation Eligibility

> Source: Design Input §11.

**P1-D-30.** Evaluation eligibility is assigned **before** future outcome and **before**
the publication result is known.

**P1-D-31.** Default `eligible = true` when Evidence is structurally valid,
non-duplicate, and market-data coverage is valid.

**P1-D-32 — immutability (normative).** Publication / suppression **must not** change
eligibility retrospectively. Evaluation eligibility is immutable after assignment.

---

# 9. Decision Contract

> Source: Design Input §11.

**P1-D-33 — Decision identity (normative).**

```text
Decision ID: qi_dec_<sha256>
Binding:     opportunity_id
             evidence identity / version / hash
             decision_input_cutoff
             decision_policy_version
```

**P1-D-34 — Decision values (normative).**

```text
PUBLISH
SUPPRESS
```

**P1-D-35 — reason codes (normative).**

```text
PUBLISH_Q1
PUBLISH_Q2
PUBLISH_Q3
PRIORITY_BELOW_GATE
STALE_INPUT
INVALID_EVIDENCE
DUPLICATE
```

**P1-D-36 — Decision Core responsibilities (normative).**

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

**P1-D-37.** The Decision Core is **deterministic only**. No Trade Plan, Order Intent,
Position Plan, Capital Allocation or Trading Risk Admission exists in the Decision
contract.

**P1-D-38.** Evidence must reconstruct the Decision input set (cross-ref
`P1_REQUIREMENTS.md` P1-R-66).

---

# 10. Report Version Contract

> Source: Design Input §12.

**P1-D-39 — report identity (normative).**

```text
Language:          Simplified Chinese
Template version:  P1_ZH_REPORT_V1
LLM:               NONE
```

**P1-D-40 — report content contract (normative).** The report contains:

```text
symbol
time
primary behavior
corroborating behavior
evidence breadth
priority
structure
taker flow
OI / funding
direction coherence
data quality
known / unknown / conflict
IDs / versions
```

**P1-D-41 — report exclusion contract (normative).** The report contains **no**
buy / sell recommendation, long / short instruction, entry, stop loss, take profit,
leverage or position sizing.

**P1-D-42 — report versioning (normative).** Reports are immutable versions. A new
publishable Evidence revision creates a **new `report_version`**. An existing report
version is never mutated.

---

# 11. Delivery Attempt Contract

> Source: Design Input §12.

**P1-D-43 — channel (normative).**

```text
Telegram Bot via HTTPS Bot API sendMessage
No inbound webhook, no command bot, no inbound processing
```

**P1-D-44 — secrets (normative).** Secrets come only from environment:

```text
QI_TELEGRAM_BOT_TOKEN
QI_TELEGRAM_CHAT_ID
```

Secrets must not enter config, DB, logs, or reports.

**P1-D-45 — attempt policy (normative).**

```text
One automatic delivery attempt per report version.
No automatic retry (a timeout can leave remote acceptance unknown).
Recorded status: CONFIRMED / FAILED / UNKNOWN
```

**P1-D-46 — notification selection (normative).**

```text
Notify only: first published report
             or quality-tier upgrade (Q1→Q2, Q2→Q3)
Same-tier revision => UI / history only
```

**P1-D-47.** Delivery timestamps `delivery_requested_at` and `delivery_confirmed_at`
are part of the causal time contract (P1-D-01).

---

# 12. BBO Observation Contract

> Source: Design Input §14.

**P1-D-48 — live vs durable (normative).**

```text
Live BBO      = latest-value overwrite in memory
Durable BBO   = 1-second PATH_SAMPLE observations persisted for
                Outcome / restart
```

**P1-D-49.** If latest BBO age `> 2s`, **no fake price sample is written**. A gap is
recorded as a gap, never as a synthetic observation.

**P1-D-50 — retention (normative).**

```text
PATH_SAMPLE retention       = 24h
Outcome results             = remain durable
Coverage records            = observed sample ratio + max_gap_seconds
```

---

# 13. Forward Outcome Contract

> Source: Design Input §13.

**P1-D-51 — anchor scopes (normative).**

```text
Every evaluation-eligible Opportunity
  => DETECTION_ANCHOR scopes at 5m and 15m
If a report is published
  => additionally REPORT_PUBLICATION_ANCHOR scopes at 5m and 15m
Anchor types remain separate.
```

**P1-D-52 — due time (normative).**

```text
due_at = anchor_time + horizon_seconds
due_at never moves due to retry / restart
```

**P1-D-53 — reference (normative).** Reference is a **fresh legal BBO midpoint** with
`available_at <= anchor_time` and age `<= 2s`, captured durably.

**P1-D-54 — endpoint (normative).** Outcome endpoint is the greatest `PATH_SAMPLE`
`available_at` with `anchor_time < available_at <= due_at`; the endpoint must be no more
than **5 seconds** before `due_at`, otherwise `status = COVERAGE_GAP`.

**P1-D-55 — no retroactive backfill (normative).** No data with
`available_at > due_at` may backfill the original outcome.

**P1-D-56 — formulas (normative).**

```text
mid            = (bid + ask) / 2
market_return  = endpoint_mid / reference_mid - 1
MFE            = max(path_mid / reference_mid - 1)
MAE            = min(path_mid / reference_mid - 1)
```

**P1-D-57 — outcome exclusion contract (normative).** No fills, fees, slippage,
leverage, position sizing, account / portfolio PnL.

**P1-D-58 — status values (normative).**

```text
IMMATURE
MEASURABLE
UNAVAILABLE
COVERAGE_GAP
```

**P1-D-59 — missing semantics (normative).** Missing / unavailable / immature must
**not** be coerced to zero return or to failure.

**P1-D-60 — direction consistency (normative).** Reported only when the primary event has
a natural direction. `OI_EXPANSION` primary ⇒ `NOT_APPLICABLE`.

**P1-D-61 — scope uniqueness (normative).** Outcome scope uniqueness binds
`opportunity_id + anchor_type + horizon_seconds`.

---

# 14. Coverage / Missing Semantics

> Source: Design Input §13, §14.

**P1-D-62.** Coverage is recorded, not inferred: coverage records observed sample ratio
and `max_gap_seconds`.

**P1-D-63.** An outcome that cannot be measured honestly is reported as
`UNAVAILABLE` / `COVERAGE_GAP` / `IMMATURE`; it is never silently reported as a measured
result.

**P1-D-64.** Market-data coverage validity participates in evaluation eligibility
(P1-D-31) and must be evaluated at eligibility time, not retrospectively.

---

# 15. Persistence Entities

> Source: Design Input §19.

**P1-D-65 — minimum entity set (normative).**

```text
instruments
market_events
opportunity_episodes
evidence_versions
evaluation_eligibility
decisions
reports
delivery_attempts
market_observations
outcome_scopes
outcome_results
```

**P1-D-66.** Core identity / version / time fields must preserve the contracts in this
document.

**P1-D-67 — immutability rules (normative).**

```text
Evidence versions          — immutable
Report versions            — immutable
Evaluation eligibility     — immutable after assignment
Primary Market Event       — immutable
Outcome scope uniqueness   — opportunity_id + anchor_type + horizon_seconds
```

---

# 16. Retention

> Source: Design Input §14.

**P1-D-68 — durable facts (normative).** Durable:

```text
Market Event
Evidence
Decision
Report
Delivery
Outcome
Evaluation Eligibility
required BBO observations (PATH_SAMPLE)
```

**P1-D-69 — bounded, non-durable facts (normative).** Remaining bounded in memory,
**not** permanently stored:

```text
Raw Trade ticks
confirmed Kline rolling history
OI / funding rolling samples
live latest-value BBO
```

**P1-D-70 — retention values (normative).**

```text
PATH_SAMPLE retention = 24h
outcome results       = remain durable
```

---

# 17. Traceability Index

| Data contract block | Design Input source |
|---|---|
| §2 Causal Time Contract | §3 |
| §3 Canonicalization / Hashing | §8 |
| §4 Market Event Identity | §8 |
| §5a STRUCTURE_BREAKOUT | §5 |
| §5b TAKER_FLOW_IMBALANCE | §6 |
| §5c OI_EXPANSION | §7 |
| §6 Opportunity Episode / Evidence Revision | §9 |
| §7 Quality / Direction / Priority | §10 |
| §8 Evaluation Eligibility | §11 |
| §9 Decision Contract | §11 |
| §10 Report Version Contract | §12 |
| §11 Delivery Attempt Contract | §12 |
| §12 BBO Observation Contract | §14 |
| §13 Forward Outcome Contract | §13 |
| §14 Coverage / Missing Semantics | §13, §14 |
| §15 Persistence Entities | §19 |
| §16 Retention | §14 |

---

# 18. Boundary

```text
P0                        = ACCEPTED
P1 Data Contract          = NOT FROZEN
P1 Requirement            = NOT FROZEN
P1 Implementation         = NOT AUTHORIZED
Business Coding           = FORBIDDEN
This document authorizes: NOTHING
```
