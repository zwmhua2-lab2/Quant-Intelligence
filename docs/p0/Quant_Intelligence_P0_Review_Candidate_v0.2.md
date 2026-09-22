# Quant Intelligence｜P0 Review Candidate v0.2

日期：2026-09-23  
Project Control Chat Window：Quant Intelligence｜总控  
Working Project Name：Quant Intelligence（工作名）  
Review Candidate ID：QI-P0-RC-0.2  
Status：REVIEW CANDIDATE / NOT ACCEPTED / NOT IMPLEMENTATION AUTHORIZATION  
Project Repository：PROJECT REPOSITORY NOT ESTABLISHED  
MarketPulse Derivation Source：`7a023f99ac44ac744e5a9885fd82f8a87ef51a94`  
Independent GPT-6 Critical Review：NOT PERFORMED

> 本文件是 P0-02 的审查候选输入，不是业务实现任务，不授权 DeepSeek 开始 Coding，不代表 Quant Intelligence 已建立 Repository，也不代表 P0 已 Accepted。

---

## 1. P0-02 结论

P0-02 的目标是把 v0.1 的“模块级派生判断”收敛为：

1. 可追踪的文件级/能力级派生 Manifest；
2. 明确的交易耦合黑名单；
3. 依赖闭包处理策略；
4. Product Constitution / Governance / Architecture 的审查候选；
5. 可直接交给独立 GPT-6 的 Critical Review Task。

本轮未执行：

- 新项目业务 Coding；
- MarketPulse 源码修改；
- Quant Intelligence Repository 创建；
- 数据库迁移；
- 市场 Runtime 启动；
- 产品 AI Provider 调用；
- 推送通知；
- GPT-6 Independent Review。

---

## 2. Product Constitution — Review Candidate

### C-01｜产品身份

Quant Intelligence 是 **AI-assisted Quant Opportunity Intelligence System**。

它负责：

市场观察  
→ 市场结构/特征  
→ Opportunity Scanner  
→ Evidence  
→ Decision Core  
→ Deep AI Analysis  
→ Opportunity Report  
→ Push / UI  
→ History  
→ Forward Outcome / Review。

产品目标是提高“值得关注的市场机会”的发现、筛选、解释、推送和长期质量验证能力。

它不承诺盈利，也不把“异常”自动等同于“机会”。

### C-02｜不可被普通任务删除的核心闭环

以下链路属于 Core Product Loop：

`Market Data → Feature / Structure → Scanner → Evidence → Decision → Analysis → Report → Delivery → History → Forward Outcome / Review`

阶段实施可以分批，但不能通过普通 Task Design 将 Forward Outcome / Review 永久推迟成“以后再说”。

### C-03｜交易边界

以下能力在当前 Product Constitution 下全部禁止：

- Automatic Trading
- Paper Trading
- Live Trading
- Order / Fill Execution
- Position Management
- Stop-Loss / Take-Profit Execution
- Position Protection
- Trading Risk Admission
- Account Balance Management
- Trading Ledger
- Execution Recovery
- Economic Reconciliation

任何普通 Requirement、Fix、Refactor、Migration、Reuse Task 都不得重新引入这些能力。

如果未来需要加入，进入：

`NEEDS_HUMAN_DECISION`

并视为 Product Constitution Change。

### C-04｜允许的方向性分析

系统可以输出：

- bullish / bearish / neutral 倾向；
- 支持因素；
- 反证；
- 关键区域；
- 机会成立条件；
- 机会失效条件；
- 不确定性。

这些属于 Intelligence，不是 Order Intent、Trade Plan、Position Plan 或 Risk Admission。

### C-05｜MarketPulse 是来源，不是父产品 Runtime

MarketPulse 仅作为 pinned source reference。

新项目必须拥有独立：

- Repository；
- Namespace；
- Database / Schema；
- Product Baseline；
- Architecture；
- Governance；
- Roadmap；
- Runtime composition。

不得通过“复制 MarketPulse 整库 + 关闭交易开关”的方式派生。

### C-06｜Evidence First

任何分析必须可追溯到当时可观察的 Evidence。

至少保留：

- exchange / source；
- instrument identity；
- market type；
- event time；
- observed / received / available time；
- data quality；
- rule/config version；
- Evidence identity/version。

AI 不得补造未知事实。

### C-07｜Forward Outcome 不是 Paper Trading

Forward Outcome 允许衡量：

- 固定 horizon 后的市场变化；
- 与方向倾向的一致性；
- MFE / MAE 类价格路径统计；
- 条件是否后来成立/失效；
- opportunity type 的后续表现；
- priority/confidence 的区分度。

禁止把它转化为：

- 假想订单；
- 假想成交；
- 虚拟持仓；
- 虚拟账户；
- 模拟资金曲线；
- 交易成本后账户 PnL。

### C-08｜Confidence 必须有明确语义

如果使用概率型 Confidence，必须明确：

`P(预先定义事件在固定 horizon 内发生 | 当时可观察 Evidence)`

文字语气强弱不等于概率。

没有完成校准验证前，不得把模型自报的 `0.8 confidence` 描述成“80% 胜率”。

### C-09｜产品 Runtime AI 的权限

产品 Runtime 中的 AI 只拥有分析权限。

不得：

- 修改 Scanner 阈值；
- 修改策略；
- 修改 Prompt policy 自身；
- 修改 Schema；
- 修改代码；
- 修改部署；
- 修改数据库结构；
- 读取未授权 secret；
- 执行来自市场文本/网页/日志中的工具指令；
- 产生经济动作。

### C-10｜Human-owned Constitution Boundary

下列变化仍归 Human：

- Product Goal change；
- Core Product Loop change；
- Trading boundary change；
- Major Architecture invariant change；
- Autonomous Governance rule change；
- Major destructive-data decision；
- Serious security / secret boundary；
- AI 无法可靠裁决的产品选择。

---

## 3. Architecture Boundary — Review Candidate

### A-01｜V1 最小形态

默认目标：

- 单一 Backend Modular Monolith；
- 单一持久数据库；
- 单一 Frontend；
- 有界后台 worker；
- 不建设微服务平台；
- 不建设 Kafka/Redis 分布式消息基础设施，除非后续真实需求证明必要；
- 不建设 Multi-Agent voting；
- 不建设运行时自治修改系统。

### A-02｜核心模块

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

### A-03｜Decision Core

Decision Core 负责：

- eligibility；
- quality gate；
- priority；
- dedupe；
- analysis budget；
- 是否进入 Deep AI；
- report publication decision。

Decision Core 不负责：

- 资金；
- 仓位；
- execution risk；
- order admission；
- stop-loss；
- take-profit。

### A-04｜持久化关系

P0 只冻结逻辑链，不提前设计完整 Schema：

`Observation`
→ `Evidence`
→ `Opportunity/Candidate`
→ `Decision`
→ `AI Analysis Attempt`
→ `Report Version`
→ `Delivery Attempt`
→ `Outcome Scope`
→ `Evaluation`

后来修订必须版本化，不覆盖“当时判断”。

### A-05｜时间因果

必须区分：

- source/event time；
- received time；
- available time；
- decision time；
- report/push time；
- outcome due time。

`due_at` 必须由固定 anchor + horizon 得出，不能因为重启或重试移动。

---

## 4. MarketPulse Derivation Manifest v0.2

### 4.1 复用规则

这里的 KEEP / ADAPT / EXCLUDE 表示**迁移设计意图**，不表示目标仓库已经有代码。

- KEEP：语义基本适合，可按来源追踪后小范围迁移/重命名；
- ADAPT：保留思想或部分实现，但必须重新冻结目标语义；
- EXCLUDE：源码/模块不能进入目标项目业务依赖闭包。

任何 KEEP/ADAPT 项在实施前仍须建立目标 Task、测试映射和 dependency preflight。

### 4.2 文件级候选

| 来源文件/能力 | 决定 | 原因 | 目标处理 |
|---|---|---|---|
| `marketpulse/clock.py` | KEEP | 极低耦合 Clock / SystemClock / ReplayClock | 可独立迁移并重命名 package |
| `marketpulse/ids.py` | KEEP | 通用 UUID/UTC helper，无交易依赖 | 只保留实际需要的 helper |
| `market_data/models.py` | ADAPT | 数据模型低耦合，但包含 `tradable` 等来源语义 | 重定义为 observation/market availability 语义，避免交易能力暗示 |
| `market_data/base.py` | ADAPT | Provider abstraction 可复用 | 按新 feed contract 重冻 |
| `market_data/normalizer.py` | ADAPT | OKX normalization 有价值；当前直接 `datetime.now(UTC)` | 注入 Clock/received time，强化 deterministic/as-of semantics |
| `market_data/okx_live.py` | ADAPT | 实盘行情接入能力可利用 | 只迁移公开行情数据；剥离与交易 readiness/账户能力的任何耦合 |
| `market_data/universe_provider.py` | ADAPT | Universe 数据入口可用 | 重新冻结目标 universe filter |
| `universe/models.py` | ADAPT | 小型 universe model 候选 | 重审字段语义 |
| `universe/builder.py` | ADAPT | universe 构建思想可复用 | 不继承交易准入含义 |
| `universe/service.py` | ADAPT | 服务边界可参考 | 目标 repo 重新组合 |
| `g2/structure.py` | ADAPT | 依赖仅 NormalizedKline + MarketStructure；实现简单可解释 | 保留算法思想，重新命名和测试 |
| `g2/scanner.py` | ADAPT | Detector 明确不输出 trade direction | 不直接搬 G2 models；抽取新 intelligence event contract |
| `g2/models.py` | ADAPT-SUBSET ONLY | 混有 AdvisoryWorkRef、lifecycle、state、DirectionBias 等多种历史语义 | 只迁移确实需要的 DTO/enum；禁止整文件复制 |
| `edge/identity.py` | ADAPT | deterministic identity 思想有价值 | 重新建立 QI namespace/domain tags |
| `edge/hashing.py` | ADAPT | canonical/domain-separated hashing 有价值，但依赖 G5 canonicalizer | 抽取独立 canonical core，不引入 G5 replay 层 |
| `g5/canonical.py::canonicalize_value` | ADAPT-SUBSET ONLY | canonicalization 函数通用，但文件同时包含 PostgreSQL workspace replay | 只迁移 canonical value algorithm；不得复制 G5 workspace/replay machinery |
| `edge/capture.py` | ADAPT-CONCEPT | as-of anchor、拒绝未来数据、immutable capture 很有价值；代码强绑定 G2/DB/ForwardAdmission | 不直接复制文件；在 QI Evidence 模块重新实现核心约束 |
| `edge/models.py` | ADAPT-SUBSET ONLY | Evidence/Outcome contract 思想有价值；含 `FORWARD_PAPER`、单一 BREAKOUT_RETEST family 等旧语义 | 重新定义 QI Evidence/Outcome vocabulary |
| `edge/forward.py` | ADAPT-CONCEPT | due_at、available_at、endpoint selection、因果规则可复用 | 移除 SPOT_LONG_TAKER_ROUND_TRIP / fee / slippage / paper cost 语义后重建 |
| `edge/forward_service.py` | ADAPT-CONCEPT | durable scheduling/retry 可参考 | 只复用到期 outcome orchestration 思路 |
| `edge/aggregation.py` | ADAPT-SUBSET | deterministic aggregation/coverage/uncertainty 有价值 | 删除 trading expectancy 语义，按 intelligence quality 重新命名指标 |
| `edge/aggregation_orchestration.py` | ADAPT-CONCEPT | snapshot/fold/orchestration 模式可参考 | 目标 repo 新实现 |
| `edge/aggregation_snapshot.py` | ADAPT-CONCEPT | stable snapshot 思想有价值 | 只在确有需要时引入 |
| `runtime.py` | ADAPT-SUBSET | worker supervision/health registry 可用；readiness vocabulary 含 Risk/Safety/Recovery 交易语义 | 抽取通用 worker supervision，不复制 readiness constants |
| `runtime_host.py` | EXCLUDE AS SOURCE FILE | 大量绑定 execution/position/risk/recovery | 目标 RuntimeHost 从零组合 |
| `api/app.py` | ADAPT-CONCEPT | FastAPI lifespan/readiness 结构可参考 | 不复制旧 readiness semantics |
| `frontend` 工程基础 | ADAPT | React/TS/Vite 可复用工程经验 | 新 UI 信息架构，不复制交易页面 |
| `advisory/identity.py` | ADAPT-SUBSET | deterministic invocation/attempt identity 可借鉴 | 重新建立 QI analysis identity |
| `advisory/contracts.py` | ADAPT-CONCEPT | immutable provider config/deadline 合约有价值 | 移除 Gate A/Gate B/Paper vocabulary |
| `advisory/attempts.py` | EXCLUDE AS SOURCE FILE | 强绑定现有 DB records、G2 AdvisoryWorkRef、fence | 目标 AI attempt lifecycle 新建 |
| `advisory/predicates.py` | EXCLUDE AS SOURCE FILE | 强绑定 DecisionSnapshot/G2 durable state/activation gate | 只参考 fail-closed 原则 |
| `advisory/worker.py` | EXCLUDE AS SOURCE FILE | 强绑定当前 invocation DB、fake provider、Gate semantics | 目标 AI worker 新建 |
| `edge/attribution.py` | EXCLUDE | 明确观察 Trade Plan → Risk → Execution → Order/Fill → Economic Result | 不得进入新项目 |
| `edge/repository.py` | EXCLUDE AS SOURCE FILE | persistence schema 混有 attribution、旧 evidence/outcome 与 source DB models | 新项目独立 repository/schema |
| `g3/*` | EXCLUDE | 交易决策/Trade Plan 域 | 禁止迁移 |
| `g4/*` | EXCLUDE | Risk/Position/Execution/Economic 域 | 禁止迁移 |
| `g5/replay.py` | EXCLUDE | 完整旧 workspace/replay，不是 QI V1 必需 | 不迁移 |
| MarketPulse DB migrations | EXCLUDE | 继承旧业务 Schema 会带入交易债务 | QI 从独立 migration baseline 开始 |

---

## 5. Dependency Closure Rules

### D-01｜不允许“整层迁移”

即使目标文件来自 KEEP/ADAPT 模块，也不得因为 Python import 方便而把整个 `g2`、`edge`、`g5`、`advisory` 搬进新项目。

### D-02｜Canonicalizer 处理

`edge/hashing.py` 的 canonical hash 依赖 `g5.canonical.canonicalize_value`。

但 `g5/canonical.py` 同文件还包含 PostgreSQL durable workspace snapshot 能力。

因此迁移策略是：

- 只提取 canonical value serialization 的通用算法；
- 在 QI 中建立独立 `canonical`/`hashing` utility；
- 使用新的 QI domain tags；
- 不迁移 G5 Replay workspace。

### D-03｜G2 Models 处理

`g2/models.py` 不是纯 Scanner DTO 文件。

它同时包含：

- Advisory handoff；
- lifecycle/state；
- DirectionBias；
- feature/event structures。

因此 Scanner 迁移必须先定义新的 QI observation/event contracts，不能直接 import 整份 G2 model。

### D-04｜Runtime 处理

`runtime.py` 中：

- worker supervision；
- health registry；
- lifecycle state pattern

可以参考/抽取。

但其中 Risk Admission、Safety Queue、Recovery 等 readiness vocabulary 不进入 QI。

`runtime_host.py` 整文件禁止迁移。

### D-05｜Edge 处理

Evidence / Outcome 的价值来自：

- immutable revision；
- deterministic identity；
- as-of capture；
- lookahead prevention；
- fixed horizon；
- idempotency；
- coverage/maturity。

不来自：

- paper cost；
- trade PnL；
- risk/execution lineage；
- `FORWARD_PAPER` 命名；
- trading expectancy。

### D-06｜Advisory 处理

可复用思想：

- invocation identity；
- attempt identity；
- immutable provider/model/prompt/schema version；
- deadline；
- retry boundedness；
- final result binding；
- fail-closed validation。

不能复制：

- Gate A/Gate B/Paper activation taxonomy；
- G2 durable state predicates；
- Trade Decision vocabulary；
- source DB record graph。

---

## 6. Explicit Trading Contamination Blacklist

目标项目依赖图中出现以下业务对象时，默认视为 P0 boundary violation，除非 Human 正式修改 Constitution：

- `PaperExecutionService`
- `PaperOrderRecord`
- `PositionRecord`
- `TradePlanRecord`
- `PositionProtectionService`
- `SystemRiskGate`
- `RecoveryManager`（经济恢复语义）
- Order / Fill domain
- Account / Balance domain
- Trading Ledger
- Economic Result authority
- Risk Admission
- Trade Plan
- Position lifecycle

注意：

单词 `risk` 可以用于普通软件风险、数据质量风险，但不得借命名重新引入 Trading Risk Admission。

---

## 7. Evaluation Contract — Review Candidate

### E-01｜必须记录完整候选漏斗

至少区分：

- detected；
- evidence invalid；
- stale；
- rejected by deterministic quality gate；
- duplicate；
- rate/budget suppressed；
- AI requested；
- AI failed；
- report published；
- delivery attempted；
- delivery failed/unknown；
- outcome immature；
- outcome unavailable；
- outcome measurable。

只保存“最终通知”的样本会产生选择偏差。

### E-02｜Missed Opportunity 需要独立参考集

Scanner 完全没有发现的行情不会进入 candidate log。

因此漏报评价必须额外定义：

- reference event taxonomy；
- frozen matching window；
- actual observed Universe；
- data coverage；
- coverage gap。

只能报告：

“相对于冻结 reference definition 的 miss rate”。

### E-03｜Deep AI 增益

在相同：

- candidate；
- Evidence；
- information cutoff；
- outcome definition

下对比：

- WITHOUT_AI；
- WITH_AI。

至少比较：

- selection / prioritization quality；
- contradiction detection；
- factual coverage；
- report usefulness；
- latency；
- provider failure rate；
- cost。

AI 不得访问 future outcome 再修改当时答案。

### E-04｜Outcome 无交易账户含义

可以计算价格变化和路径统计，但不计算：

- simulated fill；
- position sizing；
- leverage；
- fee-adjusted account return；
- portfolio PnL。

---

## 8. Autonomous Development Governance — Review Candidate

### G-01｜正常自治流程

`Design → Freeze → DeepSeek Implementation → Independent GPT Review → AI Acceptance → Closeout → Next Task`

Human 不作为普通任务按钮。

### G-02｜Worker 无设计权

DeepSeek V4.1 Flash：

只实现、测试、自审、提交、报告证据。

不得：

- 改 Requirement；
- 改 Architecture；
- 扩 Scope；
- 删除/弱化 AC；
- 修改 Constitution；
- 修改 Governance；
- 自己接受自己的实现。

### G-03｜Acceptance

Worker PASS ≠ Acceptance。

只有：

- Frozen Requirement 已知；
- Candidate SHA 固定；
- Independent Review PASS；
- 没有阻断 finding；
- identity/baseline 无漂移；

才允许由 delegated AI Acceptance Authority 标记普通 Task ACCEPTED。

不得伪造 Human Acceptance。

### G-04｜两轮 Fix 熔断

同一问题族：

Implementation  
→ Review  
→ FIX-01  
→ Review  
→ FIX-02  
→ Review。

第三次仍 FIX REQUIRED：

进入 `ROOT_CAUSE_REVIEW`。

不得通过：

- 改 Task ID；
- 换窗口；
- 换模型；
- 换名字；
- 拆成“新任务”

来清空失败历史。

### G-05｜RCA

RCA 至少分类：

- Executor capability；
- implementation defect；
- Task design；
- Requirement；
- Architecture；
- Baseline/context；
- verification/evidence；
- tool/network/environment。

只有触及 Human-owned boundary 才进入 `NEEDS_HUMAN_DECISION`。

### G-06｜Repository Truth

正式项目建立后，优先级：

Repository Truth  
> Frozen Artifact  
> Review Evidence  
> Worker Report  
> Chat History / Memory。

PROJECT_STATE 只保存快照和指针，不作为历史证据仓库。

### G-07｜任务身份

Formal Task 必须绑定：

- Project；
- Task ID；
- Repository；
- Accepted Baseline；
- Task Base；
- Requirement version/digest；
- Candidate SHA；
- execution config；
- acceptance criteria。

错项目、错任务、错 SHA 的结果不得吸收。

### G-08｜独立 Review

Reviewer：

- 不改代码；
- 不改 Requirement；
- 不改 DB；
- 不改 Git refs；
- 不相信 Worker 自评；
- 从 Repository Truth 自行读取 source/diff/test/evidence。

Verdict 仅：

- PASS
- FIX REQUIRED
- BLOCKED
- ESCALATION REQUIRED

### G-09｜工具自动化与治理自治分离

本项目已授权的是：

“普通设计和验收无需逐任务等待 Human”。

这不等于：

“已经验证 DevRelay/浏览器/DeepSeek 可以完全自动跑”。

自动传递和工具连接必须另外验证，不可伪造。

---

## 9. Initial Roadmap — Review Candidate

### P0｜Constitution / Governance / Bootstrap

完成：

- source truth；
- derivation manifest；
- Constitution；
- Governance；
- Architecture boundary；
- GPT-6 critical review；
- findings closure；
- Repository bootstrap；
- Registry；
- Final Baseline freeze。

**P0 完成前无业务 coding authorization。**

### P1｜First Observable Intelligence Loop

最小完整链：

真实行情  
→ 少量 Scanner Event  
→ Evidence  
→ deterministic Decision baseline  
→ Report  
→ 单通知通道  
→ 简体中文最小 UI  
→ History  
→ due Outcome。

P1 明确标记 `WITHOUT_AI BASELINE`。

### P2｜Deep AI

加入：

- 单一真实 Provider；
- frozen prompt/schema；
- structured output；
- timeout/retry；
- failure downgrade；
- WITH_AI / WITHOUT_AI paired evaluation。

### P3｜Quality / UX

完善：

- priority；
- dedupe；
- coverage；
- miss-rate reference；
- calibration；
- type outcome；
- history/report experience。

### P4｜V1 System Audit

真实持续观察并验证：

- reliability；
- recovery；
- latency；
- budget；
- data gaps；
- no-trading boundary；
- product quality evidence。

由 GPT-6 做 System-Level Audit。

---

## 10. P0 Open Assumptions / Not Verified

以下不能在 GPT-6 Review 前伪装成事实：

1. Quant Intelligence 正式 Repository：NOT ESTABLISHED。
2. 最终项目名称：NOT FROZEN。
3. PostgreSQL 是否最终作为 QI V1 DB：PROPOSED / NOT FROZEN。
4. Product Runtime AI Provider：NOT SELECTED。
5. DeepSeek 端到端自动调用/回传能力：NOT VERIFIED。
6. 新项目目标环境依赖：NOT VERIFIED。
7. MarketPulse 所有 tests 与目标迁移项的完整 test mapping：NOT COMPLETE。
8. 每一个 ADAPT 文件的完整 transitive dependency：只完成关键风险闭包，不声称全仓静态依赖证明。
9. P1 instrument universe / horizons / thresholds：NOT FROZEN。
10. 产品价值指标的达标阈值：NOT FROZEN，不编造。

---

## 11. GPT-6 Critical Review — Frozen Review Task v0.2

Review Task ID：`QI-P0-R01`  
Task Type：READ_ONLY / CRITICAL REVIEW  
Target：Independent GPT-6 review context  
Implementation Permission：NONE  
Project Repository：PROJECT REPOSITORY NOT ESTABLISHED  
Reviewed Artifact：本 `QI-P0-RC-0.2` 的实际文件 bytes + SHA-256  
MarketPulse Source：`7a023f99ac44ac744e5a9885fd82f8a87ef51a94`

### 11.1 Review Goal

判断本 Review Candidate 是否：

1. 忠实执行 Human 已授权的产品方向；
2. 没有通过复用重新带回交易系统；
3. 没有让 AI 获得 Human-owned Constitution 权限；
4. 能够支持正常任务自治而不变成 Worker 自验；
5. 有足够清晰的 V1 Architecture boundary；
6. 能真实验证机会发现质量与 Deep AI 增益；
7. 没有明显过度设计。

### 11.2 Required Review Areas

Reviewer 必须检查：

1. Product Goal / Core Loop 是否一致；
2. Trading exclusions 是否存在语义逃逸；
3. Forward Outcome 是否仍可能演化为 Paper Trading；
4. KEEP/ADAPT/EXCLUDE 是否过度复用；
5. 文件级 Manifest 是否仍可能通过 transitive import 带入 G3/G4；
6. canonical/hashing 抽取方案是否合理；
7. Scanner strength / priority 是否被错误当成 opportunity quality；
8. Evidence as-of / available_at / horizon 是否阻止 lookahead；
9. Confidence 是否定义为可校准对象；
10. miss-rate 是否避免“只统计 Scanner 已发现样本”的循环证明；
11. WITH_AI / WITHOUT_AI 是否是真正 paired comparison；
12. AI Acceptance 是否保持 independent review；
13. Fix fuse 是否能防无限自动修复；
14. Human authority 是否仍不可被 AI 自行改变；
15. Architecture 是否为了“高级”而增加不必要基础设施；
16. P1 是否形成真正完整但小的 observable loop；
17. P0 Freeze Gate 是否充分。

### 11.3 Source Verification Requirement

GPT-6 不得只相信本文件对 MarketPulse 的描述。

至少应独立抽查 pinned source 中：

- `marketpulse/clock.py`
- `marketpulse/market_data/models.py`
- `marketpulse/market_data/normalizer.py`
- `marketpulse/g2/scanner.py`
- `marketpulse/g2/models.py`
- `marketpulse/g2/structure.py`
- `marketpulse/edge/capture.py`
- `marketpulse/edge/models.py`
- `marketpulse/edge/forward.py`
- `marketpulse/edge/aggregation.py`
- `marketpulse/edge/attribution.py`
- `marketpulse/edge/hashing.py`
- `marketpulse/g5/canonical.py`
- `marketpulse/runtime.py`
- `marketpulse/runtime_host.py`
- `marketpulse/advisory/contracts.py`
- `marketpulse/advisory/identity.py`
- `marketpulse/advisory/attempts.py`
- `marketpulse/advisory/predicates.py`
- `marketpulse/advisory/worker.py`

不要求因此升级为 MarketPulse 全仓系统审计。

### 11.4 Forbidden Actions

Reviewer 不得：

- 修改文件；
- 建仓库；
- 写代码；
- 改 Constitution；
- 自己批准新的 Human-owned 产品变化；
- 把建议直接当 Accepted decision；
- 伪造测试/runtime evidence。

### 11.5 Verdict

只允许：

- PASS
- FIX REQUIRED
- BLOCKED
- ESCALATION REQUIRED

### 11.6 Finding Format

每个 finding 必须包含：

- Finding ID；
- Severity；
- 相关条款；
- 具体证据；
- 问题；
- 为什么重要；
- 最小修复方向；
- 是否触发 `NEEDS_HUMAN_DECISION`。

Reviewer 还必须单列：

- NOT VERIFIED；
- assumptions；
- unreviewed scope；
- reviewed artifact digest。

---

## 12. P0 Freeze Gate

P0 只有同时满足以下条件才能成为 ACCEPTED：

1. Product Constitution Final；
2. Autonomous Governance Final；
3. Architecture Boundary Final；
4. Derivation Manifest Final；
5. GPT-6 Critical Review 已真实执行；
6. 所有 blocking findings 已关闭；
7. 必要 re-review 已完成；
8. Quant Intelligence Repository 已真实建立；
9. Global Project Registry 已登记；
10. 最终 artifact 已写入目标 Repository；
11. 新项目拥有自己的真实 Git baseline SHA；
12. P1 Scope / Out of Scope / Acceptance Boundary 已形成；
13. **仍未自动授权业务编码，直到首个 P1 Formal Task 被冻结。**

---

## 13. P0-02 Status

Result：

`P0-02 REVIEW INPUT CLOSURE = COMPLETE`

但：

`P0 = NOT ACCEPTED`

当前推荐下一动作：

`P0-03｜Independent GPT-6 Critical Review`

不是：

- 建业务代码；
- 启动 DeepSeek；
- 创建 Scanner；
- 接入真实 AI；
- 开始 UI。

若 GPT-6 输出 `FIX REQUIRED`：

进入 `P0-04｜Sol Extra High Findings Closure`。

若 GPT-6 输出 `PASS`：

进入 P0 Bootstrap/Repository freeze 收口，然后才能形成最终 P0 Baseline。
