# Quant Intelligence — ARCHITECTURE

```text
Document ID:        QI-P0-DOC-ARCHITECTURE
Status:             ACCEPTED
Phase:              P0 — Accepted Baseline
P0 Status:          ACCEPTED
Business Implementation Authorization: NONE
Source of truth:    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md (§3, §4, §8)
Clause range:       A-01 … A-04, T-01 … T-07, H-01 … H-03
```

> Faithful split of `QI-P0-RC-0.3`. Architectural boundary and invariants are preserved
> exactly. This document is boundary-level only: it deliberately does **not** design a
> full database schema.

---

# Part A — Architecture Boundary

## A-01｜V1 最小形态

默认：

- 单 Backend Modular Monolith；
- 单持久数据库；
- 单 Frontend；
- 有界 worker；
- 单一清晰 Runtime composition。

V1 不预建：

- 微服务集群；
- Kafka；
- Redis 分布式任务平台；
- Multi-Agent voting；
- Runtime 自修改平台；
- 为“架构先进”而增加的新编排框架。

## A-02｜模块

1. Market Data / Universe
2. Feature / Market Structure
3. Scanner
4. Opportunity Evidence
5. Decision Core
6. Deep AI Analysis
7. Report / Delivery
8. History
9. Forward Outcome
10. Evaluation / Review
11. Runtime Foundation
12. API / Frontend

## A-03｜Decision Core

负责：

- candidate eligibility；
- deterministic data-quality gate；
- opportunity quality rule；
- processing priority；
- dedupe；
- analysis budget；
- whether-to-request-AI；
- publication decision。

不负责：

- account；
- capital；
- order；
- position；
- execution；
- trading risk。

## A-04｜逻辑数据链

P0 冻结逻辑关系，不提前设计完整 Schema：

`Observation`
→ `Feature Snapshot`
→ `Evidence`
→ `Candidate`
→ `Decision`
→ `AI Analysis Attempt`
→ `Report Version`
→ `Delivery Attempt`
→ `Outcome Scope`
→ `Evaluation Record`

修订必须新增版本，不覆盖当时版本。

---

# Part B — Causal Time Contract

> 本节是 P0 级不变量，不是 P1 才临时决定的实现细节。
> （关闭 `QI-P0-R01` / `P1-02`）

## T-01｜时间词汇

### `source_event_time`

上游市场源声称事件发生的时间。它不能单独证明本系统当时已经知道该事件。

### `received_at`

本系统实际收到输入的时间。

### `available_at`

该输入在本系统中成为合法可消费事实的最早时间。对于外部数据，`available_at >= received_at`。若解析、确认、聚合完成后才可合法使用，则 `available_at` 必须反映这一点。

### `feature_as_of`

一个 Feature Snapshot 使用的所有贡献输入中：

`feature_as_of = max(contributing_input.available_at)`

Feature 不得声称早于其最后一个必要输入已经可用。

### `detected_at`

Scanner 对该 Feature 形成事件的时间，必须满足：

`detected_at >= feature_as_of`

### `evidence_at`

Evidence 冻结时间，必须满足：

`evidence_at >= feature_as_of`

### `decision_input_cutoff`

Decision / AI Analysis 被允许看到的最新信息边界。不得消费：

`available_at > decision_input_cutoff`

的事实。

### `decision_at`

Decision 完成时间，必须满足：

`decision_at >= decision_input_cutoff`

### `report_published_at`

报告成为产品可展示对象的时间。

### `delivery_requested_at`

系统向外部通知通道发出请求的时间。

### `delivery_confirmed_at`

通道确认接收/投递的时间（如果通道提供）。它不等于“用户已经阅读”。

## T-02｜Kline 可用性

若上游 Kline 有 open_time、close_time、confirmed，则不能把 `open_time` 当成“整根确认 Kline 已经可用”的时间。

任何需要 confirmed bar 的 Feature，只有在确认帧实际到达并合法可用后才允许消费。

`bar source time` 与 `bar information availability` 分离。

## T-03｜滚动 Feature 因果性

滚动 Feature 使用 N 个输入时：

1. 记录/可重建贡献窗口边界；
2. 所有贡献输入 `available_at <= feature_as_of`；
3. 不允许事后迟到数据静默重写原 Feature；
4. 若迟到/修订数据需要重新计算，只能产生新的 Feature/Evidence revision；
5. 原 Decision / Report 仍绑定原 revision。

## T-04｜Outcome Anchor 必须声明类型

QI 至少区分两个概念，禁止混为一个指标。

### Detection Outcome

衡量：“系统检测到这个机会以后，市场发生了什么？”

Anchor：`DETECTION_ANCHOR`

用于评价 Scanner / Opportunity detection。

### Post-Publication Outcome

衡量：“报告已经发布以后，市场发生了什么？”

Anchor：`REPORT_PUBLICATION_ANCHOR`

用于评价考虑系统分析延迟后的产品可行动信息窗口。

它不得被描述成“用户实际看到后的收益”，因为系统通常不知道用户阅读时间。

一个 Outcome Scope 必须持久化自己的：

- anchor_type；
- anchor_time；
- reference observation；
- horizon；
- policy version。

不同 anchor_type 不得混在同一统计分母中。

## T-05｜固定 Horizon

`due_at = frozen_anchor_time + frozen_horizon`

重试、重启、补算不得移动 due_at。

## T-06｜Outcome 数据截止

用于某 horizon 的数据必须满足：

`available_at <= due_at`

不能因为 source_event_time 看起来落在窗口里，就使用 due_at 后才到达的数据。

## T-07｜迟到和修订

迟到、缺失、修订、不可用分别记录。

不得用后来完整的数据重写“当时系统看见了什么”。

---

# Part C — Runtime Health Contract

> QI 不继承旧 HealthRegistry 的 `missing check => HEALTHY` 语义。
> （吸收非阻断 Finding `P2-03`）

## H-01｜Required Health Checks Fail Closed

被声明为 required 的 component：

- 未注册；
- 未初始化；
- 状态未知；
- heartbeat 缺失；
- worker 不存在；

均不得推导为 READY。

## H-02｜Unexpected Worker Exit

长期 worker 若在未收到合法 stop/cancel 的情况下正常 return，必须记录为 `UNEXPECTED_EXIT / UNAVAILABLE`，不能因为“没有抛 exception”保持健康。

## H-03｜Readiness

READY 必须是所需检查的显式 conjunction，不是默认状态。

P1 Runtime Formal Requirement 必须包含 required component registry、unknown/missing state、worker exit、stale heartbeat、provider stream down、DB unavailable 的 fail-closed acceptance fixtures。

---

## Cross-references

```text
A-03 Decision Core exclusions  → constitutionally owned by PRODUCT_CONSTITUTION.md (C-03, C-04)
A-04 logical data chain        → outcome/evaluation semantics in EVALUATION_CONTRACT.md
T-01 … T-07 causal invariants  → enforced at evaluation time by EVALUATION_CONTRACT.md (E-04, E-07)
H-01 … H-03 runtime health     → mandatory Formal Task AC preconditions recorded in
                                 DERIVATION_MANIFEST.md §Migration Acceptance Preconditions (MAP-03)
A-01 "no unnecessary microservices / no Kafka / no Redis platform by default /
      no multi-agent voting / no runtime self-modification"
                               → derived-source EXCLUDE rules in DERIVATION_MANIFEST.md (M-06)
```
