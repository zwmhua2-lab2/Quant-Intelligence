# Quant Intelligence｜P0 Review Candidate v0.3

日期：2026-09-23  
Project Control Chat Window：Quant Intelligence｜总控  
Working Project Name：Quant Intelligence  
Review Candidate ID：QI-P0-RC-0.3  
Predecessor：QI-P0-RC-0.2  
Predecessor SHA-256：`32833aa3a3f0ef18b3b2b6220aabb32a44f304cb76f0ffe5ff95339c39fbcdb3`  
Trigger Review：`QI-P0-R01`  
Trigger Verdict：`FIX REQUIRED`  
Status：FINDINGS-CLOSED DESIGN CANDIDATE / NOT ACCEPTED / NOT IMPLEMENTATION AUTHORIZATION  
Project Repository：PROJECT REPOSITORY NOT ESTABLISHED  
MarketPulse Derivation Source：`7a023f99ac44ac744e5a9885fd82f8a87ef51a94`  
Business Implementation Authorization：NONE

> 本文件是 `QI-P0-R01` Findings Closure 后的完整 P0 设计候选。
> 它不是最终 Accepted Baseline，不授权 DeepSeek 开始业务 Coding。
> v0.3 的目的，是关闭 v0.2 的五项阻断设计问题，并把三项非阻断迁移注意项转成未来 Formal Task 的强制验收前置条件。

---

# 1. P0-04 Result

本轮结果：

`P0-04｜Sol Findings Closure = DESIGN COMPLETE`

但：

`P0 = NOT ACCEPTED`

本轮没有：

- 修改 MarketPulse；
- 修改 Global Protocol；
- 创建业务代码；
- 创建数据库 Schema；
- 启动行情 Runtime；
- 调用产品 AI Provider；
- 发送通知；
- 授权 DeepSeek 业务实现；
- 声称任何源码问题已经被运行测试修复。

本轮针对固定来源提交重新核验了关键源码，确认：

1. `g2/rolling.py` 是 Scanner 前的实际 Feature producer；
2. 旧 `relative_volume` 计算确实把整窗总量除以历史单笔数量均值；
3. `g2/runtime.py` 的调用链确实包含 RollingFeatureEngine → OpportunityScanner；
4. 旧 Structure / Scanner 组合下的 `FAILED_BREAKOUT` 默认条件存在静态不可达问题；
5. `edge/hashing.py` 的非有限值递归拒绝位于 hashing wrapper，而不是单独的 `canonicalize_value`；
6. 旧 `runtime.py` 的 HealthRegistry 对未登记检查默认 `HEALTHY`。

这些事实只用于新项目派生边界，不修改原项目。

---

# 2. Product Constitution — v0.3

## C-01｜产品身份

Quant Intelligence 是：

**AI-assisted Quant Opportunity Intelligence System**

核心目标：

持续观察市场  
→ 发现值得关注的市场行为  
→ 形成可追溯 Evidence  
→ 判断机会质量与优先级  
→ 深度分析  
→ 报告 / 推送 / UI  
→ History  
→ Forward Outcome  
→ 长期 Review 系统质量。

系统不承诺盈利。

市场异常不是天然机会，Detector strength 不是机会质量，AI 自报 confidence 不是胜率。

## C-02｜Core Product Loop

以下链路属于产品核心：

`Market Data`
→ `Feature / Market Structure`
→ `Scanner`
→ `Opportunity Evidence`
→ `Decision Core`
→ `Deep AI Analysis`
→ `Opportunity Report`
→ `Delivery / UI`
→ `History`
→ `Forward Outcome / Review`

阶段实施可以分批，但普通 Task 不得永久删除 Forward Outcome / Review。

## C-03｜明确禁止的经济能力

当前 Constitution 排除：

- Automatic Trading
- Paper Trading
- Live Trading
- Order / Fill Execution
- Position Management
- Stop-Loss Execution
- Take-Profit Execution
- Position Protection
- Trading Risk Admission
- Account Balance Management
- Trading Ledger
- Execution Recovery
- Economic Reconciliation

普通 Requirement / Refactor / Reuse / Migration / Fix 均不得通过改名或隐式依赖重新带回这些能力。

未来若加入，必须进入：

`NEEDS_HUMAN_DECISION`

## C-04｜方向性分析允许范围

系统允许输出：

- bullish / bearish / neutral bias；
- 支持因素；
- 反证；
- 关键区域；
- 机会成立条件；
- 失效条件；
- 不确定性。

这些属于 Intelligence。

不得生成：

- order intent；
- trade plan；
- position plan；
- capital allocation；
- trading risk admission。

## C-05｜独立项目

MarketPulse 仅是 pinned derivation source。

Quant Intelligence 必须拥有独立：

- Repository；
- namespace；
- database/schema；
- Product Baseline；
- Architecture；
- Governance；
- Roadmap；
- Runtime composition。

禁止：

`复制 MarketPulse 整仓 → 关闭交易开关 → 当成新产品`

## C-06｜Evidence First

所有分析必须绑定当时实际可观察的 Evidence。

必须保留足以重建当时信息边界的：

- source/exchange；
- instrument identity；
- market type；
- source event time；
- received time；
- available time；
- Feature as-of；
- decision input cutoff；
- data quality；
- rule/config version；
- evidence identity/version。

未知保持未知。

## C-07｜Forward Outcome 不是 Paper Trading

允许的 Outcome：

- 固定 horizon 的市场价格变化；
- 方向一致性；
- MFE / MAE 类路径统计；
- 条件后来是否成立/失效；
- opportunity type 后续表现；
- priority / confidence 区分度。

禁止：

- 假想下单；
- 假想成交；
- 虚拟仓位；
- 虚拟账户；
- 模拟资金曲线；
- 交易成本后的账户 PnL。

## C-08｜Confidence

概率型 Confidence 只有在预测对象被明确冻结后才合法：

`P(定义事件在固定 horizon 内发生 | 当时 Evidence)`

文字语气不等于概率。

未完成校准前，不得把模型自报 `0.8` 写成“80% 胜率”。

## C-09｜Runtime AI Authority

产品 Runtime AI 只分析输入 Evidence。

不得：

- 修改代码；
- 修改 Scanner；
- 修改策略阈值；
- 修改 Schema；
- 修改部署；
- 修改 Prompt governance 自身；
- 获得未授权 secret；
- 执行市场文本、网页、日志里的工具指令；
- 创建经济动作。

## C-10｜Human-owned Boundary

仅下列变化必须 Human 决定：

- Product Goal change；
- Core Product Loop change；
- Trading boundary change；
- Major Architecture invariant change；
- Autonomous Governance rule change；
- Major destructive-data decision；
- Serious security / secret boundary decision；
- AI 无法可靠裁决的产品选择。

---

# 3. Architecture Boundary — v0.3

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

# 4. Causal Time Contract — v0.3
## （关闭 QI-P0-R01 / P1-02）

本节是 P0 级不变量，不是 P1 才临时决定的实现细节。

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

# 5. MarketPulse Derivation Manifest — v0.3
## （关闭 QI-P0-R01 / P1-01）

KEEP / ADAPT / EXCLUDE 表示迁移设计，不表示目标代码已经存在。

## M-01｜低耦合候选

| Source | Decision | QI Rule |
|---|---|---|
| `clock.py` | KEEP | 独立迁移 Clock/SystemClock/ReplayClock 语义 |
| `ids.py` | KEEP-SUBSET | 只保留实际需要的 UUID/UTC helpers |
| `market_data/base.py` | ADAPT | 新 feed contract |
| `market_data/models.py` | ADAPT | 重新定义 availability / market status 语义 |
| `market_data/normalizer.py` | ADAPT | 注入 Clock/received_at，不直接 `datetime.now()` 形成不可控时间 |
| `market_data/okx_live.py` | ADAPT | 只使用公开行情路径 |
| `market_data/universe_provider.py` | ADAPT | 新 Universe policy |
| `universe/*` | ADAPT | 不继承交易准入含义 |

## M-02｜Feature / Scanner

| Source | Decision | QI Rule |
|---|---|---|
| `g2/rolling.py` | **ADAPT-CONCEPT / NO DIRECT PORT** | Feature formulas 必须重新冻结；旧值不视为已验证 intelligence semantics |
| `g2/structure.py` | ADAPT-CONCEPT | 可参考 confirmed-bar 结构思想；不把当前算法视为完成的市场结构模型 |
| `g2/scanner.py` | ADAPT-CONCEPT | Detector taxonomy 可参考；新事件必须使用 QI Feature contract |
| `g2/models.py` | ADAPT-SUBSET ONLY | 禁止整文件复制 |
| `g2/runtime.py` | **EXCLUDE AS SOURCE FILE** | 只参考 Rolling→Scanner 调用顺序；不迁移 state/strategy/advisory/forward graph |

## M-03｜已知来源算法限制

### KSL-01｜旧 `FAILED_BREAKOUT` 不可直接复用

固定来源默认结构下：

`range_upper = H`

而：

`retest_area.upper = H + positive_width`

旧 Scanner 要求 FAILED_BREAKOUT：

`current_price < range_upper`
且
`current_price > retest_area.upper`

在正常正价格宽度下不能同时成立。

因此：

- `FAILED_BREAKOUT` **不进入任何默认迁移白名单**；
- 不得通过原算法“照搬 + 改 package 名”迁移；
- 若未来 QI 需要失败突破事件，必须作为独立 Formal Requirement 重新定义 setup、breakout confirmation、failed condition、time window、zone semantics、deterministic fixtures、acceptance examples。

### KSL-02｜旧 `relative_volume` 不可作为窗口相对成交量复用

旧 rolling：

`volume = sum(all current rolling trade quantities)`

但 baseline：

`mean(trade.quantity over trades[:-10])`

因此分子是“窗口总量”，分母是“单笔数量均值”。

该值不能直接解释成“当前窗口成交量 / 历史同长度窗口成交量”。

因此：

- 旧 `relative_volume` formula **不得进入 QI**；
- 依赖它的旧 `VOLUME_ANOMALY` detector 不进入默认迁移白名单；
- 若 QI 需要 Volume Anomaly，P1/P3 Requirement 必须重新冻结时间窗口、baseline windows、minimum history、late data policy、unit、threshold semantics、deterministic examples。

### KSL-03｜其它旧 Feature 也不是“自动验证通过”

没有被本次审查指出具体反例，不等于 price acceleration、volatility、flow imbalance、liquidity change、OI change、relative BTC strength 已经自动获得 QI 产品语义。

P1 选用任何 Detector 前必须在 Formal Requirement 中明确：

- feature definition；
- unit；
- window；
- availability semantics；
- threshold；
- known limitations；
- acceptance fixtures。

## M-04｜Edge

| Source | Decision | QI Rule |
|---|---|---|
| `edge/identity.py` | ADAPT | 新 QI domain tags |
| `edge/hashing.py` | ADAPT-SUBSET | canonical hash + non-finite rejection 一起迁移 |
| `g5/canonical.py::canonicalize_value` | ADAPT-SUBSET | 不迁移 G5 workspace replay |
| `edge/capture.py` | ADAPT-CONCEPT | as-of / immutable / lookahead prevention |
| `edge/models.py` | ADAPT-SUBSET | 删除 FORWARD_PAPER / trading vocabulary |
| `edge/forward.py` | ADAPT-CONCEPT | 固定 horizon/因果规则；删除 paper cost |
| `edge/forward_service.py` | ADAPT-CONCEPT | due scheduling / retry 思想 |
| `edge/aggregation.py` | ADAPT-SUBSET | coverage / uncertainty；删除 trading expectancy |
| `edge/attribution.py` | EXCLUDE | 绑定 Trade/Risk/Execution/Economic lineage |
| `edge/repository.py` | EXCLUDE AS SOURCE FILE | 新项目独立 Schema |

## M-05｜Advisory

| Source | Decision | QI Rule |
|---|---|---|
| `advisory/identity.py` | ADAPT-SUBSET | invocation/attempt identity |
| `advisory/contracts.py` | ADAPT-CONCEPT | immutable provider/model/prompt/schema/deadline |
| `advisory/attempts.py` | EXCLUDE AS SOURCE FILE | 目标 lifecycle 重建 |
| `advisory/predicates.py` | EXCLUDE AS SOURCE FILE | 只参考 fail-closed |
| `advisory/worker.py` | EXCLUDE AS SOURCE FILE | 目标 worker 重建 |

## M-06｜Runtime

`runtime_host.py`：**EXCLUDE AS SOURCE FILE**

`runtime.py`：**ADAPT-SUBSET ONLY**

仅参考 bounded worker、lifecycle、supervised execution、health snapshot。

不得复制 trading risk readiness、safety queue、economic recovery、默认健康语义。

---

# 6. Score Semantics Contract
## （吸收非阻断 Finding P2-01）

QI 必须区分以下四个概念。

## S-01｜Event Strength
Detector 对某个市场现象偏离阈值程度的启发式量。它不是 opportunity quality、success probability、calibrated confidence。

## S-02｜Processing Priority
系统在有限注意力/预算下的处理排序。它是 scheduling / attention rank，不是胜率。

## S-03｜Opportunity Quality
Decision Core 基于冻结规则对候选质量的分级或分数。必须有自己的定义和版本。不能直接 `quality = old scanner strength`。

## S-04｜Confidence Probability
只有满足 C-08 的预测定义和校准要求时才允许出现概率值。

因此旧源码 `priority = strength * 100` 不能迁移为 QI opportunity quality、QI confidence 或概率。

---

# 7. Canonical / Hashing Migration Contract
## （吸收非阻断 Finding P2-02）

QI 迁移 canonical hashing 时，最小完整单元不是单独 `canonicalize_value`，而是：

1. type-tagged canonicalization；
2. recursive non-finite rejection；
3. deterministic JSON byte encoding；
4. domain separation；
5. SHA-256 digest；
6. QI-specific domain tags；
7. deterministic tests。

Acceptance 必须包含嵌套 float NaN / ±Infinity、Decimal non-finite、list/tuple、Mapping、datetime timezone、enum、key ordering。

禁止只复制 `g5.canonical.canonicalize_value` 后声称已继承旧 hashing safety。

---

# 8. Runtime Health Contract
## （吸收非阻断 Finding P2-03）

QI 不继承旧 HealthRegistry 的 `missing check => HEALTHY` 语义。

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

# 9. Evaluation Cohort & Version Contract
## （关闭 QI-P0-R01 / P1-03）

## E-01｜完整 Funnel

所有候选至少记录：

- detected；
- evidence invalid/stale；
- deterministic rejection；
- duplicate；
- budget/rate suppressed；
- AI requested；
- AI failed；
- report published；
- delivery attempt；
- delivery failed/unknown；
- outcome immature；
- outcome unavailable；
- outcome measurable。

但“日志完整”本身不等于“统计分母正确”。

## E-02｜Evaluation Eligibility 必须事前确定

候选进入质量评估总体的资格，必须在知道 AI 是否成功、是否最终发布、通知是否成功、future market outcome 之前确定。

定义 `evaluation_eligible_at`，不得事后选择有利样本。

## E-03｜Outcome Coverage

对于 `evaluation_eligible` 候选，默认要求进入 Outcome coverage。

若成本/存储限制不能全部跟踪，只允许使用预先冻结的 deterministic sampling policy，例如基于 candidate_id + evaluation_policy_version 的稳定抽样。

不得看完结果后选样本。

## E-04｜配对 AI 评估

WITH_AI / WITHOUT_AI comparison 的 pair 必须在 AI outcome 之前确定。

Pair 至少绑定：

- candidate_id；
- exact evidence_id + evidence_version/hash；
- exact feature version/hash；
- same decision_input_cutoff；
- same outcome definition；
- evaluation policy version。

Baseline 与 AI 都不能看到 cutoff 后的数据。

## E-05｜AI Failure 不从 Pair 中删除

AI timeout、provider error、schema invalid、budget unavailable 时，该 pair 保留并记录 `AI_FAILED / AI_UNAVAILABLE`。

不能因为没有 AI 答案把困难样本静默删掉。

## E-06｜发布不是总体入样条件

`report published` 可以形成产品发布子集指标，但不能让 `published == evaluation denominator`。

至少区分：

- all eligible cohort；
- published cohort；
- AI-attempt cohort；
- AI-success cohort。

## E-07｜版本绑定

Primary evaluation 必须绑定当时精确的 Evidence revision、Decision version、Analysis attempt/final、Report version。

不能默认读取“当前最新 head”后冒充原始预测效果。

## E-08｜原始判断与修订判断分开

如果事后产生修订，original evaluation 与 revised evaluation 分开统计。

不能让修订版覆盖原始错误。

## E-09｜Missing Outcome

缺失 / coverage gap / immature 不硬算为 0 或失败，但必须留在 coverage denominator / missingness report 中。

## E-10｜Missed Opportunity

Scanner 漏报继续需要独立 reference event set。Candidate cohort 不能证明 Scanner 没漏掉什么。

---

# 10. Autonomous Development Governance — v0.3

## G-01｜正常流程

`Design → Freeze → DeepSeek Implementation → Independent Review → AI Acceptance → Closeout → Next Task`

普通 Task 不需要 Human 逐项确认。

## G-02｜Worker 无设计权

DeepSeek 只 implement、test、verify、self review、commit、evidence、completion report。

不得改 Requirement、Architecture、Scope、AC、Constitution、Governance，也不得自验 ACCEPTED。

## G-03｜Acceptance

Worker PASS ≠ Acceptance。

AI Acceptance 必须绑定 Project、Task ID、Frozen Requirement digest、Task Base、exact Candidate SHA、Independent Review PASS、delegated authority。

不得伪造 Human Acceptance。

## G-04｜Normal Fix Budget

同一 Problem Family 的 Normal Cycle：

1. Initial Implementation
2. Review
3. FIX-01
4. Review
5. FIX-02
6. Review

第三次 `FIX REQUIRED` 后停止 Normal Fix，进入 `ROOT_CAUSE_REVIEW`。

## G-05｜RCA 不自动授予新实施权限
## （关闭 QI-P0-R01 / P1-04）

RCA 必须产出：

- problem_family_id；
- accumulated_attempt_count；
- root-cause category；
- evidence；
- design delta；
- why previous approach failed；
- whether Human-owned boundary is touched；
- Recovery Batch authorization decision。

RCA：

- 不重置失败计数；
- 不自动开始 Coding；
- 不把同一问题改名为新 Problem Family；
- 不自动产生 FIX-03。

## G-06｜唯一一次 Post-RCA Recovery Batch

同一 Problem Family 默认最多允许：

Normal Cycle：
- Initial Implementation
- FIX-01
- FIX-02

之后 RCA-01。

若 RCA 确认存在可验证、实质性的新解法，且不改变 Human-owned boundary，Design Authority 可授权：

Post-RCA Recovery Batch：
- RECOVERY-01 Implementation
- 最多一个 RECOVERY-FIX-01

这是同一 Problem Family 的最后一个自动实施批次。

同一 Problem Family 的自动 Coding attempt 硬上限为 **5 次**。

## G-07｜预算耗尽后的终态

若 Post-RCA Recovery Batch 仍不能 PASS：

`BLOCKED + ESCALATION REQUIRED`

同一 Problem Family 不得再自动开始任何 Implementation/Fix。

允许 AI 继续调查、写 RCA、提供设计选项、处理其它不相关任务，但不能继续消耗同一问题族 Coding attempt。

这不会自动变成 `NEEDS_HUMAN_DECISION`。

只有真正要求改变 C-10 Human-owned boundary 时才询问 Human。

## G-08｜外部条件变化

若问题是 executor/tool unavailable、network、dependency outage、environment，BLOCKED 后可等待客观外部条件变化。

“条件恢复”本身不重置 Implementation budget。

若要重新授权同一 Problem Family 的 Coding 超过 G-06 上限，属于 Autonomous Governance exception，必须按 C-10 进入 Human-owned Governance Change；AI 不得自行批准例外。

## G-09｜Independent Review

Reviewer READ_ONLY，不改 source/config/DB/Git refs/Requirement，从 Repository Truth 自行检查。Worker Report 只作导航。证据不足 => BLOCKED。

Verdict 仅 PASS / FIX REQUIRED / BLOCKED / ESCALATION REQUIRED。

## G-10｜Task Identity

Formal Task 必须绑定 Project、Repository、Task ID、Requirement version/digest、Accepted Baseline、Task Base、Candidate SHA、execution config、acceptance criteria。

错窗口/错 Task/错 SHA 的反馈不得吸收。

---

# 11. Review-to-Freeze Identity Contract
## （关闭 QI-P0-R01 / P1-05）

v0.3 修改了实质设计，因此：

**QI-P0-R01 对 v0.2 的 FIX REQUIRED 不能被复用为 v0.3 PASS。**

## FZ-01｜Review Applicability Tuple

每个 Review verdict 必须绑定：

- review_task_id；
- project；
- artifact/repository path set；
- artifact version；
- exact SHA-256（未建 repo 的 artifact）或 Candidate Git SHA（建 repo 后）；
- source baseline refs；
- reviewed requirement/governance versions；
- review timestamp/source。

Verdict 只对该 tuple 生效。

## FZ-02｜语义变化使旧 PASS 失效

Review 后若发生 Constitution semantics、Governance semantics、Architecture boundary、Derivation classification、Evaluation semantics、Acceptance Boundary 的变化，旧 PASS 自动失效，必须重新 Review。

## FZ-03｜最终 P0 Review Target 必须是 Repository Candidate SHA

为消除“审本地文件 A → 入库时改成 B → 仍引用 A 的 PASS”风险，P0 流程调整为：

1. v0.3 Findings Closure；
2. 建立 **document-only Quant Intelligence repository candidate**；
3. 将最终候选拆入 canonical project files；
4. 计算实际 Candidate Git SHA；
5. 从 Candidate SHA 独立读取全部 P0 canonical files；
6. 执行 **GPT-6 Final P0 Re-Review**；
7. PASS 只绑定该 Candidate SHA；
8. 未经审查的语义变化不得进入 freeze；
9. PASS 后才形成 P0 Accepted Baseline。

因此，本地 v0.3 不是最终 P0 Review authority，它是 repo bootstrap 的设计输入。

## FZ-04｜PASS 后只允许无语义收口

如果 GPT-6 对 Candidate SHA PASS 后还需要状态标记、accepted pointer、registry pointer，必须进行 diff classification。

若修改 P0 canonical content bytes：

- 语义变化 → 旧 PASS 失效；
- 纯机械 metadata/pointer 变化 → 必须记录 exact diff 和 applicability judgement。

优先做法：先把需要进入 Accepted Baseline 的内容全部写入 Candidate，再 Review，PASS 后不改被审正文。

## FZ-05｜Baseline Manifest

最终仓库必须保存 P0 baseline mapping，至少列：

- Candidate/Accepted Git SHA；
- canonical file paths；
- file blob SHA 或 content digest；
- GPT-6 review task/result pointer；
- source MarketPulse SHA；
- Global Protocol version；
- date；
- status。

不得使用 MarketPulse SHA 冒充 QI baseline。

---

# 12. Migration Acceptance Preconditions

以下三项来自 `QI-P0-R01` 的非阻断 Findings，但现在升级为相关 Formal Task 的强制 AC 前置。

## MAP-01｜Score Semantics

Scanner / Decision Task 必须证明：

- strength != priority；
- priority != quality；
- quality != probability；
- confidence probability 有定义才能出现。

## MAP-02｜Canonical Safety

Canonical/Hashing Task 必须测试：

- nested non-finite rejection；
- deterministic bytes；
- domain separation；
- timezone-aware datetime；
- unsupported type fail closed。

## MAP-03｜Runtime Health

Runtime Task 必须测试：

- required component missing => NOT READY；
- unknown health => NOT READY；
- worker unexpected normal exit => UNAVAILABLE/FAILED；
- stale heartbeat => NOT READY；
- no exception != healthy。

---

# 13. P1 First-Slice Guardrails

P0 不在这里冻结具体 P1 Detector 集合。

但 P1 Formal Requirement 选择 Detector 时必须遵守：

1. `FAILED_BREAKOUT` 不可直接迁移；
2. 旧 `VOLUME_ANOMALY` 不可直接迁移；
3. 其它旧 Detector 也必须重新证明 Feature 定义；
4. P1 不因来源代码已存在而跳过语义验收；
5. P1 保持小范围、少事件类型；
6. first slice 必须包含 History、due Outcome、causal timestamps、exact version binding、coverage/missing status；
7. P1 明确标记 `WITHOUT_AI BASELINE`。

---

# 14. Updated P0 Roadmap

## P0-01
Initial Constitution / Governance / Architecture Draft — **DONE**

## P0-02
Derivation Review Input Closure / RC v0.2 — **DONE**

## P0-03
GPT-6 Independent Critical Review `QI-P0-R01` — **DONE / FIX REQUIRED**

## P0-04
Sol Findings Closure / RC v0.3 — **THIS DOCUMENT / DESIGN COMPLETE**

## P0-05
Document-only Repository Bootstrap Candidate

目标：

- 建立 Quant Intelligence Repository；
- 只写 P0 canonical docs / governance / state；
- 不写业务实现；
- 记录 source references；
- 形成真实 Candidate Git SHA。

## P0-06
GPT-6 Final P0 Re-Review

审查对象：

**Repository Candidate SHA**

不是聊天摘要，不是 Worker Report，不是 v0.2 verdict 的延伸。

若 PASS，进入 P0-07；若 FIX REQUIRED，返回 Sol Findings Closure。

## P0-07
P0 Final Freeze / Registry / Accepted Baseline

只有完成 exact reviewed Candidate、required closeout、Registry、baseline pointers，才能：

`P0 = ACCEPTED`

之后才设计 P1 Formal Task。

---

# 15. QI-P0-R01 Closure Matrix

| Finding | Severity | v0.3 处置 | Closure Status | Human Decision |
|---|---|---|---|---|
| P1-01 Manifest 漏 Feature producer + 已知算法风险 | Blocking | 新增 `rolling.py`、`g2/runtime.py` 决定；冻结 KSL-01/02；FAILED_BREAKOUT 与旧 VOLUME_ANOMALY 不进默认迁移白名单；其它 Feature 也需重新验收 | **DESIGN CLOSED / RE-REVIEW REQUIRED** | NO |
| P1-02 跨模块时间因果不足 | Blocking | 新增完整 Causal Time Contract：available_at / feature_as_of / cutoff / confirmed Kline / anchor types / fixed horizon / revision rules | **DESIGN CLOSED / RE-REVIEW REQUIRED** | NO |
| P1-03 Funnel ≠ Evaluation cohort | Blocking | 新增事前 cohort、deterministic sampling、exact-version pair、AI failure retention、original vs revised evaluation、missing coverage | **DESIGN CLOSED / RE-REVIEW REQUIRED** | NO |
| P1-04 RCA 可无限重入 | Blocking | Normal 3 attempts + 唯一 Post-RCA 2 attempts；同 Problem Family 自动 Coding 总上限 5；耗尽 => BLOCKED + ESCALATION REQUIRED；超限需 Governance Change | **DESIGN CLOSED / RE-REVIEW REQUIRED** | NO；只有超限例外才 YES |
| P1-05 Final P0 与 Review 内容未精确绑定 | Blocking | Final Review 改为 Repository Candidate SHA；Review applicability tuple；语义 diff 失效；baseline manifest | **DESIGN CLOSED / RE-REVIEW REQUIRED** | NO |
| P2-01 Score semantics | Non-blocking | 冻结四类 score 语义并加入迁移 AC | **INCORPORATED** | NO |
| P2-02 Canonical non-finite boundary | Non-blocking | 明确 canonicalize + recursive non-finite reject + hash wrapper 为一个迁移单元 | **INCORPORATED** | NO |
| P2-03 Runtime health default | Non-blocking | QI required health fail-closed；unexpected normal exit 不得健康 | **INCORPORATED** | NO |

`DESIGN CLOSED` 不代表 Reviewer 已接受。

五个 Blocking Findings 只有在独立 Final Re-Review PASS 后才能称为 `REVIEW CLOSED`。

---

# 16. Semantic Diff Summary — v0.2 → v0.3

## Added

- Causal Time Contract
- Evaluation Cohort & Version Contract
- Score Semantics Contract
- Runtime Health Contract
- explicit `rolling.py` derivation decision
- explicit `g2/runtime.py` exclusion
- known-source-limitations registry
- Post-RCA hard attempt budget
- final review binding to Repository Candidate SHA
- P0 document-only bootstrap before Final Re-Review
- non-finite canonical migration acceptance
- exact original/revised evaluation separation

## Changed

### Derivation
v0.2 的 Scanner/Structure ADAPT 被收紧：`rolling.py` 被明确纳入，已知错误语义不得迁移。

### Outcome
从 fixed anchor + horizon 扩展为 Detection Outcome / Post-Publication Outcome 双 scope，并冻结跨模块 available-time 约束。

### Evaluation
从“完整 funnel + paired AI”扩展为 pre-outcome cohort assignment、exact version binding、AI failure retention、sampling policy、original vs revised evaluation。

### Governance
RCA 后不再只有“有限尝试”原则，而是明确自动 Coding 总上限 **5 / Problem Family**。

### P0 Freeze
最终 Critical Review 改为 document-only repo Candidate SHA 建立后再审，避免本地审查包与最终仓库正文漂移。

---

# 17. NOT VERIFIED

当前仍未验证：

1. Quant Intelligence Repository — NOT ESTABLISHED。
2. Final Project Name — NOT FROZEN。
3. PostgreSQL final choice — PROPOSED / NOT FROZEN。
4. Runtime AI Provider — NOT SELECTED。
5. DeepSeek automatic invocation/return — NOT VERIFIED。
6. Target environment — NOT VERIFIED。
7. Full transitive dependency graph — NOT PROVEN。
8. Full migration test mapping — NOT COMPLETE。
9. P1 detector set — NOT FROZEN。
10. P1 universe / horizons / thresholds — NOT FROZEN。
11. Product KPI thresholds — NOT FROZEN。
12. v0.3 Independent Re-Review — NOT PERFORMED。
13. P0 Accepted Baseline — NOT AVAILABLE。

---

# 18. P0 Freeze Gate — v0.3

P0 只有同时满足以下条件才可 ACCEPTED：

1. Product Constitution Final；
2. Autonomous Governance Final；
3. Architecture Boundary Final；
4. Causal Time Contract Final；
5. Evaluation Contract Final；
6. Derivation Manifest Final；
7. source limitations 已显式处理；
8. document-only Repository 已建立；
9. canonical P0 files 已落库；
10. Candidate Git SHA 已固定；
11. GPT-6 Final P0 Re-Review 对该 Candidate SHA PASS；
12. blocking findings = 0；
13. review applicability 与 Candidate 一致；
14. Global Project Registry 已注册；
15. P0 Baseline Manifest 完成；
16. Accepted Baseline 是 QI 自己真实 Git SHA；
17. P1 Scope / Out of Scope / Acceptance Boundary 可恢复；
18. 首个 P1 Formal Task 尚需独立 Freeze 才能 Coding。

---

# 19. Current State

```text
P0-01  DONE
P0-02  DONE
P0-03  DONE / GPT-6 = FIX REQUIRED
P0-04  DESIGN COMPLETE / v0.3
P0-05  NOT STARTED
P0-06  NOT STARTED
P0-07  NOT STARTED

P0 = NOT ACCEPTED
Business Implementation Authorization = NONE
DeepSeek Business Coding = FORBIDDEN
PROJECT REPOSITORY NOT ESTABLISHED
```

下一步：

**P0-05｜Document-only Repository Bootstrap Candidate**

目的不是业务开发。

目的是让下一次 GPT-6 Final P0 Re-Review 可以绑定：

**真实 Repository Candidate SHA**

从而彻底关闭 `P1-05` 所指出的 Review → Freeze 漂移空档。
