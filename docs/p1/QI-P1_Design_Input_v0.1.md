# Quant Intelligence｜P1 Design Input v0.1

Document ID: QI-P1-DESIGN-INPUT-0.1
Phase: P1 — First Observable Intelligence Loop
Status: DESIGN CANDIDATE / NOT FROZEN
Accepted P0 Baseline: p0-v1.0.1
Accepted Baseline Commit: d3a9ec58515d310466683a61b3e141879367879f
P1 Implementation Authorization: NONE
Runtime Deep AI: OUT OF SCOPE

---

# 1. Goal and Product Loop

P1 proves one thing: Quant Intelligence can consume real market data, detect a small set of explicit market behaviors, persist traceable evidence, make a deterministic publish/suppress decision, render a Simplified-Chinese report, deliver it through one external channel, expose it in UI/history, and evaluate forward market outcomes without any trading semantics.

Core loop:

Real Market Data → Feature / Market Structure → Detector → Market Event → Opportunity Episode → Evidence Version → Deterministic Decision → Deterministic Chinese Report → Delivery / UI / History → 5m / 15m Forward Outcome

P1 is explicitly WITHOUT_AI BASELINE. Runtime LLM / Deep AI belongs to P2.

---

# 2. Market Scope

Exchange: OKX only.
Market type: USDT SWAP only.
Default configurable allowlist:
- BTC-USDT-SWAP
- ETH-USDT-SWAP
- SOL-USDT-SWAP

Startup must validate instrument exists, market_type=SWAP, quote_asset=USDT, tradable=true. Any default instrument validation failure disables that instrument and makes required-universe health NOT READY.

No dynamic Top100 / Hot30 universe in P1.

Required inputs:
- Trade
- Best Bid / Offer
- confirmed 1m Kline
- Open Interest
- Funding Rate

Funding is Evidence Context only and never an independent P1 Detector.

---

# 3. Causal Time Contract

External facts must preserve at least:
- source_event_time
- received_at
- available_at
- feature_as_of
- detected_at
- evidence_at
- decision_input_cutoff
- decision_at
- report_published_at
- delivery_requested_at
- delivery_confirmed_at

A fact may be consumed only when available_at <= decision_input_cutoff.

Trade/BBO available_at = actual receive time after normalization.
Confirmed Kline may become legally consumable only after the confirmed frame is actually received; open_time / close_time are not information-availability timestamps.
OI/Funding REST available_at = successful response receive time.
Late or revised data creates a new revision; it never rewrites the original Decision's information set.

---

# 4. Detector Set

P1 implements exactly three Detector domains:
1. STRUCTURE_BREAKOUT
2. TAKER_FLOW_IMBALANCE
3. OI_EXPANSION

Explicitly out of scope:
- FAILED_BREAKOUT
- old VOLUME_ANOMALY / old relative_volume semantics
- liquidation detector
- relative BTC detector
- liquidity-change detector
- price-acceleration detector
- generic volatility-anomaly detector

Liquidity/spread may be Evidence Context but not an independent P1 trigger.

---

# 5. STRUCTURE_BREAKOUT Contract

Inputs:
- previous 20 confirmed 1m bars
- latest fresh BBO midpoint

Current forming candle must not enter the prior range.

Structure version: STRUCTURE_20X1M_V1
range_upper = max(high of previous 20 confirmed bars)
range_lower = min(low of previous 20 confirmed bars)
breakout_margin = 0.001 (10 bps)

UP_BREAKOUT when mid_price > range_upper * (1 + 0.001) for 2 consecutive detector cycles.
DOWN_BREAKOUT when mid_price < range_lower * (1 - 0.001) for 2 consecutive detector cycles.

Re-arm/reset is symmetric when price returns inside the margin or structure version changes.

UP distance = mid_price / range_upper - 1
DOWN distance = 1 - mid_price / range_lower
strength = clamp((distance - 0.001)/(0.005 - 0.001), 0, 1)

Required Evidence includes structure window identity/version, range_upper, range_lower, breakout direction/reference level, current price, breakout distance, feature_as_of, detected_at.

FAILED_BREAKOUT is not implemented.

---

# 6. TAKER_FLOW_IMBALANCE Contract

Uses taker-side trade notional, not raw quantity-only reuse.

Window: 30 seconds.
Minimum validity:
- trade_count >= 30
- total_quote_notional >= 100,000 USDT
- last_trade_age <= 5 seconds

buy_notional = sum(price * quantity) where taker side=BUY
sell_notional = sum(price * quantity) where taker side=SELL
imbalance = (buy_notional - sell_notional)/(buy_notional + sell_notional)

BUY_PRESSURE when imbalance >= +0.60.
SELL_PRESSURE when imbalance <= -0.60.

Re-arm only after abs(imbalance) < 0.45 for 5 consecutive detector cycles.
strength = clamp((abs(imbalance)-0.60)/0.40, 0, 1)

Required Evidence includes window start/end, trade_count, buy/sell notional, imbalance, current_price, feature_as_of, detected_at.

BUY_PRESSURE / SELL_PRESSURE describe market behavior, not trade advice.

---

# 7. OI_EXPANSION Contract

Open Interest poll every 15s.
Funding poll every 60s.
Baseline lookback: 300s.
Baseline sample must be available_at <= current.available_at - 300s; nearest available sample is allowed with timing error <=30s.
No baseline => WARMING_UP / INSUFFICIENT.

current OI age must be <=45s.
oi_change = (current_oi - baseline_oi)/baseline_oi
Trigger when oi_change >= 0.01.
strength = clamp((oi_change-0.01)/(0.05-0.01), 0, 1)

Direction = NONE. OI expansion alone is not bullish/bearish.

Evidence includes previous/current OI and available_at values, oi_change, price-change context, funding context, feature_as_of, detected_at.

---

# 8. Detector Execution / Identity

Detector cycle = 1 second per instrument using one internally consistent Market State Snapshot.
Each detector/instrument has INACTIVE/ACTIVE state and emits only on INACTIVE→ACTIVE transition to prevent per-second duplicate spam.

Event ID format: qi_evt_<sha256>
Hash domain: QI_EVENT_V1
Hash inputs: instrument_id, detector_type, event_direction, feature_snapshot_hash, detector_config_version.
Same facts replayed must yield identical event_id.

Canonicalization safety unit:
- canonical value normalization
- recursive non-finite rejection
- deterministic UTF-8 JSON bytes
- domain-separated SHA-256
- UTC timezone-aware datetimes only

NaN / ±Infinity / timezone-naive datetimes / unsupported arbitrary objects are invalid.

---

# 9. Opportunity Episode / Evidence Revision

episode_gap = 120s.
Same instrument + new event within 120s joins current Episode; otherwise create new Episode. Different instruments never merge.

Opportunity ID: qi_opp_<hash(instrument_id + first_event_id)>.
First accepted Market Event remains immutable primary_event; later events are corroborating_events.

Evidence starts at version 1. A newly corroborating detector domain creates a new immutable evidence version. Old Evidence is never overwritten.

---

# 10. Quality, Direction Coherence, Priority

Quality tier is evidence breadth, not probability:
- Q1_SINGLE_DOMAIN = 1 detector domain
- Q2_CROSS_DOMAIN = 2 domains
- Q3_MULTI_DOMAIN = Structure + Flow + Derivatives

UI label should communicate “证据广度”, not win rate.

Direction coherence:
- COHERENT
- MIXED
- NOT_APPLICABLE

Only Structure + Flow participate in directional coherence. OI never votes direction. Q3 does not imply coherent direction.

Processing Priority is separate from quality and probability:
- magnitude_score = max(event_strength)
- breadth_score: Q1=0, Q2=0.5, Q3=1
- freshness_score = clamp(1 - latest_event_age_seconds/10, 0, 1)
- priority = round(100*(0.50*magnitude + 0.35*breadth + 0.15*freshness))

Publication gate:
- Q1 publish if priority >=65
- Q2 publish if priority >=50
- Q3 publish if priority >=40
otherwise DETERMINISTIC_REJECTED / PRIORITY_BELOW_GATE.

MIXED evidence is not automatically suppressed; report must explicitly show the conflict.

Confidence Probability is NOT IMPLEMENTED in P1.

---

# 11. Evaluation Eligibility / Decision

Evaluation eligibility is assigned before future outcome and before publication result is known.
Default eligible=true when Evidence is structurally valid, non-duplicate, and market-data coverage is valid.
Publication/suppression must not change eligibility retrospectively.

Decision Core is deterministic only. Responsibilities:
- Evidence validity
- causal cutoff
- freshness
- candidate eligibility
- dedupe
- quality tier
- processing priority
- publish/suppress

Decision ID: qi_dec_<sha256>, binding opportunity_id, evidence identity/version/hash, decision_input_cutoff, decision_policy_version.
Decision values: PUBLISH / SUPPRESS.
Reason codes include PUBLISH_Q1/Q2/Q3, PRIORITY_BELOW_GATE, STALE_INPUT, INVALID_EVIDENCE, DUPLICATE.

No Trade Plan, Order Intent, Position Plan, Capital Allocation, or Trading Risk Admission.

---

# 12. Deterministic Report / Delivery

Report language: Simplified Chinese.
Template version: P1_ZH_REPORT_V1.
No LLM.
Report contains symbol, time, primary behavior, corroborating behavior, evidence breadth, priority, structure, taker flow, OI/funding, direction coherence, data quality, known/unknown/conflict, IDs/versions.

No buy/sell recommendation, long/short instruction, entry, stop loss, take profit, leverage, position sizing.

Reports are immutable versions; new publishable Evidence revision creates a new report_version.

P1 external delivery channel: Telegram Bot via HTTPS Bot API sendMessage.
No inbound webhook, command bot, or inbound processing.
Secrets only from environment:
- QI_TELEGRAM_BOT_TOKEN
- QI_TELEGRAM_CHAT_ID
Secrets must not enter config, DB, logs, or reports.

One automatic delivery attempt per report version. No automatic retry because timeout can leave remote acceptance unknown. Record CONFIRMED / FAILED / UNKNOWN.
Notify only first published report or quality-tier upgrade (Q1→Q2, Q2→Q3). Same-tier revision updates UI/history only.

---

# 13. Forward Outcome

Every evaluation-eligible Opportunity gets DETECTION_ANCHOR scopes at 5m and 15m.
If a report is published, also create REPORT_PUBLICATION_ANCHOR scopes at 5m and 15m.
Anchor types remain separate.

due_at = anchor_time + horizon_seconds and never moves due to retry/restart.

Reference is a fresh legal BBO midpoint available_at <= anchor_time and age <=2s, captured durably.
Outcome endpoint is the greatest PATH_SAMPLE available_at with anchor_time < available_at <= due_at; endpoint must be no more than 5s before due_at or status=COVERAGE_GAP.
No data with available_at > due_at may backfill the original outcome.

mid=(bid+ask)/2
market_return=endpoint_mid/reference_mid-1
MFE=max(path_mid/reference_mid-1)
MAE=min(path_mid/reference_mid-1)

No fills, fees, slippage, leverage, position sizing, account/portfolio PnL.

Status:
- IMMATURE
- MEASURABLE
- UNAVAILABLE
- COVERAGE_GAP

Missing/unavailable/immature must not be coerced to zero return or failure.

Direction consistency only when primary event has natural direction. OI_EXPANSION primary => NOT_APPLICABLE.

---

# 14. BBO Sampling / Retention

Live BBO is latest-value overwrite in memory.
Persist 1-second BBO PATH_SAMPLE observations for Outcome/restart.
If latest BBO age >2s, no fake price sample is written.
PATH_SAMPLE retention =24h; outcome results remain durable.
Coverage records observed sample ratio and max_gap_seconds.

Raw Trade ticks, confirmed Kline rolling history, OI/funding rolling samples remain bounded in memory and are not permanently stored. Durable facts are Market Event, Evidence, Decision, Report, Delivery, Outcome, Evaluation Eligibility, and required BBO observations.

---

# 15. Runtime / Restart / Health

Single Backend Modular Monolith with FastAPI lifecycle and bounded workers:
- OKXPublicWSWorker
- OKXCandleWSWorker
- DerivativesContextWorker
- DetectorCycleWorker
- BboSamplerWorker
- OutcomeSchedulerWorker
- DeliveryWorker

Runtime states: STARTING, WARMING_UP, READY, DEGRADED, NOT_READY, FAILED, STOPPING, STOPPED.

READY requires SQLite writable, Public WS alive, Candle WS alive, default universe validated, fresh BBO, Structure/Flow/OI warmups ready, Detector/Outcome workers alive, Telegram configuration valid, and no required worker unexpected exit.

Unexpected normal return => UNEXPECTED_EXIT, not healthy. Exception => FAILED. Missing/unknown/stale/failed/unexpected required components => NOT_READY.

Default freshness:
- Public WS heartbeat 15s
- Candle WS 90s
- OI 45s
- Funding 120s
- Detector worker 5s
- Outcome worker 5s

Restart must not duplicate reports/outcome scopes or move due_at; mature pending outcome scopes resume; detector rolling state warms up again.

---

# 16. Startup Warmup

Structure: fetch 21 recent completed 1m bars via OKX REST; their available_at is startup fetch response receive time, not historical close_time.
Flow: 30s live trade warmup.
OI: 5m live OI history; do not fabricate pre-start baseline.

---

# 17. Technology / Persistence

Backend:
- Python
- FastAPI
- Pydantic v2
- SQLAlchemy 2.x async
- aiosqlite
- Alembic
- httpx
- websockets

Frontend:
- React
- TypeScript
- Vite

Persistence:
- SQLite local file: data/quant_intelligence.sqlite3
- WAL mode
- PRAGMA foreign_keys=ON
- PRAGMA synchronous=FULL
- PRAGMA busy_timeout=5000
- process-wide asyncio.Lock write coordinator and short transactions

No PostgreSQL, Redis, Kafka, Celery, microservices, Docker requirement, distributed queue, or multi-agent runtime in P1.

P1 establishes its own Alembic baseline: 0001_p1_observable_loop. No MarketPulse migrations are reused.

---

# 18. Repository Layout

Target modules under backend/src/quant_intelligence/: clock, config, canonical, ids, market_data, features, scanner, evidence, decision, reporting, delivery, outcomes, persistence, runtime, api.
Frontend under frontend/.
Non-secret config: config/p1.toml.

Forbidden directories/domains: trading, orders, positions, risk, execution, portfolio.

---

# 19. Persistence Entities

At minimum:
- instruments
- market_events
- opportunity_episodes
- evidence_versions
- evaluation_eligibility
- decisions
- reports
- delivery_attempts
- market_observations
- outcome_scopes
- outcome_results

Core identity/version/time fields must preserve the contracts above. Evidence and report versions are immutable. Outcome scope uniqueness binds opportunity_id + anchor_type + horizon_seconds. Evaluation eligibility is immutable after assignment.

---

# 20. API / UI / Security

Read-only API:
- GET /api/v1/opportunities
- GET /api/v1/opportunities/{opportunity_id}
- GET /api/v1/health

No mutation API for trade/order/position/decision/threshold/retry.
Configuration changes require restart with frozen config.

UI pages only:
- / — 实时机会
- /opportunities/:id — 机会详情
- /history — 历史与 Outcome

Frontend polls opportunities every 2s; no frontend WebSocket in P1.

FastAPI binds 127.0.0.1; frontend localhost. LAN/Internet exposure requires a new security requirement.

---

# 21. MarketPulse Reuse

ADAPT-SUBSET:
- clock.py
- ids.py
- edge/hashing.py + g5/canonical.py as one canonical/hash safety unit
- runtime.py worker supervision/health pattern only

ADAPT-CONCEPT:
- market_data/models.py
- market_data/normalizer.py (received/available time explicitly injected; no implicit datetime.now causality)
- market_data/okx_live.py (split Public WS / Business WS / REST context)
- edge/forward.py causal due/endpoint/coverage concepts only
- edge/forward_service.py durable scheduling/restart concepts
- FastAPI structure
- React/TS/Vite engineering foundation

REIMPLEMENT FROM CONTRACT:
- g2/rolling.py
- g2/structure.py
- g2/scanner.py

EXCLUDE AS SOURCE FILE / DEPENDENCY:
- g2/models.py
- runtime_host.py
- edge/attribution.py
- g3/*
- g4/*
- PaperExecutionService
- Order/Fill domain
- PositionRecord
- TradePlanRecord
- PositionProtectionService
- SystemRiskGate
- Economic Recovery
- Trading Ledger

Do not copy MarketPulse wholesale or import convenience dependencies that reintroduce excluded trading semantics.

---

# 22. Verification

Unit tests: canonicalization, IDs, detector formulas, priority, quality tier, episode correlation, Decision, Outcome math.
Deterministic integration: fixed Trade+BBO+confirmed Kline+OI stream through Event→Evidence→Decision→Report→Outcome.
Persistence/restart: real temporary SQLite DB, commit/restart/resume, no duplicate report, no due_at drift.
Live smoke: real OKX instrument metadata/Public WS/Candle WS/OI/Funding/health.

Live acceptance must not lower production thresholds merely to manufacture an Opportunity. Detector correctness is proven by deterministic fixtures.

---

# 23. P1 Acceptance Boundary

P1 acceptance requires at least:
1. real OKX USDT SWAP feed chain
2. BTC/ETH/SOL allowlist
3. three Detector semantics frozen/tested
4. no FAILED_BREAKOUT / old VOLUME_ANOMALY reuse
5. causal timestamps enforced
6. deterministic identity/revision
7. Evidence reconstructs Decision input
8. deterministic Decision Core
9. Q1/Q2/Q3 evidence breadth semantics
10. no Confidence Probability
11. deterministic Chinese report
12. one real Telegram delivery channel
13. three Chinese UI pages
14. History with original immutable records
15. 5m/15m Outcome scopes
16. Detection/Publication anchors separated
17. honest missing/coverage handling
18. restart idempotency and fixed due_at
19. fail-closed health
20. no trading/economic capability
21. no Runtime AI
22. live OKX smoke evidence
23. deterministic end-to-end verification

---

# 24. Implementation Batches

P1-I01｜Foundation + Market Data + Persistence
- skeleton/config/clock/canonical/hash
- SQLite/Alembic
- OKX adapters/normalized contracts/market state
- BBO sampling
- runtime supervision/health
Acceptance: real BTC/ETH/SOL SWAP data + SQLite + fail-closed health; no Detector/Report/UI.

P1-I02｜Detector → Evidence → Decision → Report Core
- 3 Detectors
- event state machine
- Episode/Evidence revisions
- Evaluation Eligibility
- quality/direction/priority
- Decision
- Chinese deterministic Report
Acceptance: deterministic fixtures and complete core chain; no AI, no delivery, no UI.

P1-I03｜Delivery + Read API + UI
- Telegram
- read-only REST API
- 实时机会 / 机会详情 / 历史与 Outcome UI
Acceptance: real report reaches Telegram; UI reads DB truth and shows lineage.

P1-I04｜Forward Outcome + Restart + P1 End-to-End
- Detection/Publication anchors
- 5m/15m outcome
- coverage/MFE/MAE
- restart resume
- pruning
- full live acceptance
Acceptance: complete P1 observable intelligence loop.

---

# 25. Current State / Next

P0 = ACCEPTED.
P1-01 Requirement Design = DESIGN COMPLETE.
P1-02 Architecture + Data Contract + Reuse Design = DESIGN COMPLETE.
P1 Requirement / Architecture = NOT FROZEN.
P1 Implementation Authorization = NONE.
Business Coding = FORBIDDEN.

Next: P1-03 Document-only P1 Design Candidate Persistence → exact Candidate Git SHA → P1-04 Independent Requirement / Architecture Review → if PASS, P1-05 Freeze + P1-I01 Formal Implementation Task.

[END OF P1 DESIGN INPUT]
