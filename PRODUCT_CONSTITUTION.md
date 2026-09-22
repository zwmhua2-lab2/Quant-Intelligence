# Quant Intelligence — PRODUCT CONSTITUTION

```text
Document ID:        QI-P0-DOC-CONSTITUTION
Status:             REVIEW CANDIDATE
Phase:              P0 — Project Bootstrap
P0 Status:          NOT ACCEPTED
Business Implementation Authorization: NONE
Source of truth:    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md (§2)
Clause range:       C-01 … C-10
```

> This document is a faithful split of the P0 Review Candidate `QI-P0-RC-0.3`.
> It preserves product semantics exactly. It does not redesign the product.
> Clause IDs `C-01 … C-10` are inherited unchanged from the source candidate.

---

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

## Cross-references

```text
C-02  Core Product Loop           → implemented architecturally in ARCHITECTURE.md (A-02, A-04)
C-03  Prohibited economic power   → enforced in ARCHITECTURE.md (A-03) and DERIVATION_MANIFEST.md (M-04/M-05/M-06 EXCLUDE rules)
C-05  Independent project         → this repository; see DERIVATION_MANIFEST.md
C-06  Evidence First              → causal invariants in ARCHITECTURE.md (T-01 … T-07)
C-07  Forward Outcome ≠ Paper Trading → Outcome anchor separation in ARCHITECTURE.md (T-04)
C-08  Confidence                  → normative restatement in EVALUATION_CONTRACT.md (Score Semantics / Confidence)
C-10  Human-owned Boundary        → escalation rules in AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md (G-07, G-08)
```
