# QI-P0-R01｜Independent Critical Review Result

日期：2026-09-23  
Project：Quant Intelligence  
Review Task ID：QI-P0-R01  
Reviewed Artifact：QI-P0-RC-0.2  
Reviewer：GPT-6 Astra Pro（本次独立审查会话）  
Mode：READ_ONLY / CRITICAL REVIEW  
Verdict：FIX REQUIRED  
Blocking Findings：5（P1-01—P1-05）  
Non-blocking Findings：3（P2-01—P2-03）  
P0：NOT ACCEPTED  
Implementation Authorization：NONE  
Repository / Source File Changes：NONE  
Output：本独立审查报告；未修改输入文件。

> P1/P2 在 Finding ID 中是严重性，不是路线图阶段。P1 表示必须在 P0 最终冻结前完成设计处置；不表示现在必须实现业务代码。P2 不阻断本次 P0 方向性冻结，但必须进入相应迁移任务的验收要求。本文的最小修复方向是审查建议，不是已接受设计。

## 1. 审查身份与证据边界

输入文件：`Quant_Intelligence_P0_Review_Candidate_v0.2(1).md`  
实际文件大小：26,151 bytes  
实际文件 SHA-256：

```text
32833aa3a3f0ef18b3b2b6220aabb32a44f304cb76f0ffe5ff95339c39fbcdb3
```

来源仓库：`zwmhua2-lab2/MarketPulse-AI`  
独立读取并核实存在的来源 commit：

```text
7a023f99ac44ac744e5a9885fd82f8a87ef51a94
```

本审查只把该 SHA 当 pinned source；没有把它声称为当前 main，也没有把 MarketPulse 的验收状态继承为 Quant Intelligence 的验收状态。新项目未建仓库是输入候选明确声明的状态，本次未执行新项目仓库发现或创建。

指定的 20 个源码文件均已完成定点读取；另沿已发现风险的真实调用链补读 `g2/rolling.py` 与 `g2/runtime.py`。这是定点静态审查，不是 22 个文件的逐行全量审计，不是全仓依赖证明，也没有执行测试或启动 Runtime。

## 2. 总体判断

产品方向正确，可以继续：独立情报产品、禁止交易、Evidence First、Forward Outcome 与 Paper Trading 分离、WITHOUT_AI 起步、单体架构及按能力派生，这些方向不需要推翻。

暂不允许冻结 v0.2。主要原因不是缺完整 Schema 或尚未选定数据库，而是仍有五个跨阶段边界缺口：已发现的来源算法风险没有进入 Manifest；时间因果仍缺最低验收不变量；评估样本和版本选择未完全闭合；RCA 后的自动重入没有明确上限；最终 P0 artifact 与通过审查的精确内容没有建立强绑定。

源项目代码中的问题不等于 Quant Intelligence 已存在实现缺陷。本审查要求先处置这些已知迁移风险，而不是修改 MarketPulse，也不是立即下发 DeepSeek 编码。

## 3. Blocking Findings

### QI-P0-R01-P1-01｜关键 Feature producer 未纳入派生处置，且源码已有具体算法风险

**Severity：P1 / BLOCKING P0 FREEZE。**  
**相关条款：** Manifest §4.2；D-01、D-03；Review Areas 4、5、7、16。  
**输入位置：** 候选 L303–L340、L344–L374。

**具体证据：**

- Manifest 将 `g2/structure.py` 与 `g2/scanner.py` 列为 ADAPT，但没有为其实际 Feature producer `g2/rolling.py` 给出决定。
- pinned source 的 `g2/runtime.py::ScannerPipeline.ingest` 实际执行 `working_rolling.ingest(...)`，随后把生成的 features 交给 `scanner.scan(...)`。不是两个孤立函数的假想拼接。
- `g2/structure.py::analyze_structure` 默认生成 `range_upper = H` 与 `retest_area.upper = H + max(H × 0.002, 1e-12)`。
- `g2/scanner.py::_breakout` 的 FAILED_BREAKOUT 分支要求同时满足 `current_price < range_upper` 与 `current_price > retest_area.upper`。对该默认结构、正价格，要求 P < H 且 P > H + 正数，不可同时成立。例：H=100 时，条件为 P<100 且 P>100.2。
- `g2/rolling.py::_snapshot` 用整窗成交量 `sum(trade.quantity)` 除以历史单笔成交量均值。120 笔数量均为 1 的成交会得到 `relative_volume=120`；旧 Scanner 默认阈值是 2.0，因此这个输入会触发 VOLUME_ANOMALY。它不能直接被解释为“相同时间窗口的成交量放大了 120 倍”。

**问题：** v0.2 的模块选择基本正确，但尚未携带这些已经可见的来源能力限制。只重命名 Scanner DTO 并不足以获得可靠的事件语义。

**为什么重要：** 可能把不可达的事件分支和不匹配的量纲一起带进新产品；系统即使行情接通、测试绿色、能够推送，也未必在发现所声称的市场行为。

**最小修复方向：**

1. 为 `g2/rolling.py` 增加明确 KEEP/ADAPT/EXCLUDE 处置及精确来源路径；不允许因 Scanner 需要 features 就顺手复制。
2. 把上述两个行为列入 Known Source Limitations / Migration Constraints，并映射到 QI 的事件能力。
3. 对相应事件选择明确处置：暂不进入首批事件白名单，或在未来目标 Task 中重新冻结语义并验证。此处不替 Designer 选择算法。
4. 对选中的 Feature 冻结窗口口径、单位、数据可用性、正反例；FAILED_BREAKOUT 需要证明能够触发，也不会在没有先前突破的普通回落中误触发。

**关闭证据：** 更新后的 Manifest 有 producer 处置、已知限制、事件级接纳条件与验证映射；不要求 P0 现在实现这些测试。

**NEEDS_HUMAN_DECISION：NO。** 这是现有情报范围内的来源处置。若由此提出改变产品目标或核心闭环，则另行升级。不得修改 MarketPulse 源码。

### QI-P0-R01-P1-02｜时间字段已列出，但跨模块的最低因果不变量尚未冻结

**Severity：P1 / BLOCKING P0 FREEZE。**  
**相关条款：** C-06；A-04、A-05；D-05；Review Area 8。  
**输入位置：** 候选 L126–L141、L256–L283、L390–L408。

**具体证据：**

- 候选要求记录 event/received/available/decision 等时间，并规定固定 anchor+horizon，但没有明确 Feature/Evidence 的信息截止判定、数据修订的可用时间继承，以及报告延迟与评估 anchor 的关系。
- 来源 `market_data/normalizer.py::normalize_kline` 把 Kline 的 event_time 设为开盘时间，另有 close_time 与 confirmed；`_base` 的 received_time 则取规范化时的当前时间。
- `g2/rolling.py::_snapshot` 把本帧 event.event_time 放进 FeatureSnapshot，同时使用多个滚动输入。这说明 event_time 不是整个 Feature 的“当时已知时间”。
- 来源 `edge/forward.py` 已有显式的固定 due_at、reference authority、available_at eligibility 与 endpoint ordering，说明这些并非仅靠字段命名就能保持的行为。

**问题：** “可追溯到当时可观察 Evidence”是正确原则，但在重新组合时，仍可能把 Kline 开盘时间、较晚确认的数据或后来的修订值误当成当时已知信息。

**为什么重要：** 会产生可复现但不因果的分析与评估；时间字段和 hash 都存在，并不自动防止前视。

**最小修复方向：** 在 P0 冻结以下不变量，具体阈值、horizon 数值和 Schema 留给 P1：

- Feature/Evidence 的可用性不得早于其所需输入的实际可用性；供一次分析使用的输入必须满足冻结的 information cutoff。
- 迟到数据、事后确认或修订必须保留自己的可用时间与版本，不得回写原判断所见内容。
- Outcome 预先冻结评估对象、anchor 类型、参考价格口径、endpoint/迟到数据政策；重试、重启或推送不得移动原 scope 的 anchor/horizon。
- 事件发现后的表现与报告实际可见后的表现不是同一个指标。允许选择一种或分别计算，但不得混报、不得把报告发布前的涨跌算成用户收到报告后的效果。

上述要求不强制照搬来源的所有时间政策；例如 Outcome 是否允许迟到标签数据，应单独定义，不能混入原分析的输入可用性。

**关闭证据：** 一组可直接转为 AC 的因果规则，至少包含“开盘早、确认晚”“迟到/修订”“重启/重试”“报告晚于事件”四种反例的预期处置。

**NEEDS_HUMAN_DECISION：NO。** 仅落实 Evidence First 与既定因果要求。

### QI-P0-R01-P1-03｜完整漏斗不等于无偏评估，paired comparison 缺少样本与版本锁定

**Severity：P1 / BLOCKING P0 FREEZE。**  
**相关条款：** A-04；E-01、E-03；P1/P2 Roadmap；Review Areas 10、11、16。  
**输入位置：** 候选 L256–L270、L456–L519、L692–L717。

**具体证据：**

- E-01 已正确要求保留被拒绝、预算压制和 AI 失败等漏斗状态。
- E-03 已正确要求相同 candidate、Evidence、information cutoff、outcome definition。
- 但没有锁定共同 cohort 在何时进入评估，也没有明确有效但未发布候选的 Outcome 采样、失败 arm 的保留方式，以及评估使用首次判断还是后续修订。
- A-04 的逻辑链把 Outcome 放在 Report/Delivery 后；这不必然表示错误，但也没有显式阻止“只有发布后的候选才有 Outcome”。
- 来源 `edge/aggregation.py` 的头部说明采用 stable snapshot 中的 canonical Evidence/Outcome head。保留原始历史并不自动阻止评估时选择后来修订的 head。

**问题：** 可以出现所有拒绝原因都记录了，但质量比较只使用“AI 成功且报告发布”的幸存样本；或者不覆盖原始记录，却以事后较好的版本取代原始判断来计分。

**为什么重要：** 这种系统能产生漂亮统计，却无法证明 AI 提高了选择质量；AI 失败的代价、未发布样本以及事后修订会被隐去。

**最小修复方向：**

- 共同 cohort 在知道 AI 是否成功、报告是否发布以及 outcome 之前确定；不要求对所有检测事件都调用 AI。
- 为有效候选定义 Outcome 覆盖或事前冻结的抽样政策；无效或无法度量样本保留原因，不能伪造收益，也不能静默删除分母。
- paired identity 至少绑定候选、精确 Evidence 版本、cutoff、baseline policy、treatment 配置及 outcome definition；不能只绑定可变的“最新版本”。
- 失败/超时/预算压制保留在相应总体统计中；可另列成功配对样本分析，但应同时披露其覆盖率和适用范围，不能把失败硬当作数值零。
- “原始发出时判断”与“事后修订分析”分开计分，不通过重试选择后来表现最好的答案。

P0 只需要冻结上述规则和后续 Task 的前置门槛，不需要实现统计平台、全候选深度 AI 或全量历史回测。

**关闭证据：** 用一个小型混合样例说明成功、失败、预算压制、未发布、未成熟、缺数据、修订候选分别进入什么集合、使用哪个版本；样例必须不能通过只保留成功推送改变总体结论。

**NEEDS_HUMAN_DECISION：NO。** 保持现有质量验证目标，不新增交易能力。

### QI-P0-R01-P1-04｜两轮 Fix 的停止点明确，但 RCA 后的自动重入仍可形成无界循环

**Severity：P1 / BLOCKING P0 FREEZE。**  
**相关条款：** C-10；G-04、G-05；Review Areas 13、14。  
**输入位置：** 候选 L190–L201、L573–L611。

**具体证据：** 候选规定第三次 FIX REQUIRED 进入 ROOT_CAUSE_REVIEW，禁止换 Task/窗口/模型清空历史；但 G-05 只列 RCA 分类，没有规定重入所需证据、剩余允许次数/预算及耗尽后的终态。

**问题：** 不清空历史，并不等于不能无限重复“RCA→重设计→修复→RCA”。当前文字没有证明自治系统最终会停止。

**为什么重要：** 这直接影响“不需要逐任务人工确认”的可控性，而不只是日志格式。

**最小修复方向：**

- 同问题族保留稳定身份、累积尝试和熔断状态；RCA 本身不重置它们。
- ROOT_CAUSE_REVIEW 不是新的 Implementation Authorization；重入必须有明确责任角色、RCA disposition、变化点和被冻结的有界授权。
- 明确累计上限或不可自动重入规则，以及耗尽后的 BLOCKED/PAUSED 终态，不能把无限重试写成“继续自治”。
- 普通能力/环境故障停止后，不自动等同 NEEDS_HUMAN_DECISION；只有需要改变 Human-owned 边界才升级。

不在本报告中替 Designer 冻结一个新的重试次数，也不授权增加已声明的两轮 Fix。

**关闭证据：** 一个重复失败的状态迁移样例必须有限终止；改 Task ID、改模型和 RCA 均不能自动清零或获得无界授权。

**NEEDS_HUMAN_DECISION：NO，限于补齐既有熔断语义。** 若修复方案要增加自治权限、突破原授权修复边界或改变治理规则，则 YES，不能由本报告自行批准。

### QI-P0-R01-P1-05｜最终 P0 基线缺少与审查对象的精确内容绑定

**Severity：P1 / BLOCKING P0 FREEZE。**  
**相关条款：** G-03、G-07；§11 Reviewed Artifact；§12 P0 Freeze Gate。  
**输入位置：** 候选 L557–L569、L625–L639、L764–L772、L880–L896。

**具体证据：** 本次审查绑定实际文件 bytes+SHA-256；Freeze Gate 要求真实审查、findings 关闭、必要 re-review、最终 artifact 入库及真实 Git baseline，却没有明确“最终文件内容如何证明就是通过审查的内容”。

**问题：** 可能把 v0.2 的 Review PASS 当作“项目已有审查”，在 bootstrap/finalization 期间改变规则，再把改变后的文件冻结。G-03 对普通候选 SHA 的要求没有直接补上这个 P0 文档包的绑定关系。

**为什么重要：** Reviewer 看过某个版本，不能为后来的其他内容背书；独立审查若不绑定最终版本，冻结门槛存在空档。

**最小修复方向：**

- 用审查记录/manifest 绑定最终被审查文件的路径、版本和实际 digest，以及对应 findings closure/re-review。
- 对 Review 后到 P0 Final 之间的每个 diff 明确处置；语义变化或未审查的新内容不能沿用旧 PASS。
- closeout 核验最终仓库中的内容与审查绑定相符，再记录目标项目 baseline SHA。可以把状态记录和审查主体分开，避免自引用 digest。
- 纯状态/排版变更不必重做全仓审计，但必须有差异分类和明确的审查适用记录，不能静默豁免。

**关闭证据：** “审查后改一条 Constitution/Governance/Architecture 规则”会使旧 PASS 不能直接完成 P0 Freeze；最终文件集可从 baseline 反查审查证据。

**NEEDS_HUMAN_DECISION：NO。** 这是落实已有 review/baseline 不漂移原则，不授予新的 Acceptance 权限。

## 4. Non-blocking Findings

### QI-P0-R01-P2-01｜事件强度、处理优先级和质量/概率应显式分开

**Severity：P2 / NON-BLOCKING P0。**  
**相关条款：** C-01、C-08、A-03、Manifest Scanner 项；Review Areas 7、9。  
**证据：** `g2/scanner.py::_event` 中 priority 直接由 strength×100 截断/取整生成；`_strength` 是指标相对阈值的裁剪，不是统计校准。

**问题与影响：** 直接保留名为 priority 的分数，容易在 Decision/UI 被误读为“机会质量”或“成功率”。候选已经写“不把异常等同机会”，但字段迁移还需落实。

**最小修复：** 目标事件强度、队列/分析优先级、规则质量等级、概率 confidence 分别命名和定义。可以用启发式强度排序，但必须如实标记；不要求 P1 就有高质量概率模型。对应迁移 AC 验证不得把 strength=0.8 或 priority=80 展示为 80% 胜率。

**NEEDS_HUMAN_DECISION：NO。**

### QI-P0-R01-P2-02｜canonicalize_value 抽取必须保留外围拒绝策略

**Severity：P2 / NON-BLOCKING P0。**  
**相关条款：** Manifest hashing/canonical 项；D-02；Review Area 6。  
**证据：** `g5/canonical.py::canonicalize_value` 对 float 使用 value.hex()；递归拒绝 NaN/Infinity 的 `_reject_non_finite` 在 `edge/hashing.py`，由 canonical_bytes 调用。

**问题与影响：** “只提取通用 canonical 算法”不能被实现成只搬一个函数；否则可能丢失原 hashing 的拒绝规则。v0.2 没有要求删除这层检查，本 finding 是迁移验收补充，不认定新项目已存在缺陷。

**最小修复：** 显式列出算法、错误类型、支持类型、非有限值拒绝及 QI domain tags 的抽取范围；目标 Task 加入类型区分、时区、嵌套非有限数值、domain separation 的正反例。不迁移 workspace replay 或 SQLAlchemy 依赖。

**NEEDS_HUMAN_DECISION：NO。**

### QI-P0-R01-P2-03｜HealthRegistry 的缺省 HEALTHY 不能直接用于新 Runtime readiness

**Severity：P2 / NON-BLOCKING P0。**  
**相关条款：** A-01、Manifest runtime.py 项、D-04。  
**证据：** 来源 `runtime.py::HealthRegistry.status` 的默认值是 HEALTHY，条目不存在时直接返回默认值；`supervised_worker` 对异常退出做标记，但正常返回不会标记长期 worker 已停止。

**问题与影响：** 抽取 registry/supervision 后，尚未注册、失联或意外返回的关键 worker 可能被错误理解为健康。来源完整 RuntimeHost 可能另有检测，但 QI 已明确不迁移它，因此不能默认继承其兜底。

**最小修复：** 新 Runtime Task 明确必需组件、unknown/not-started/stopped/failed 的 readiness 行为，验证“关键组件缺失不得报 READY”和“长期 worker 非预期返回可观察”。不因此引入交易 Risk Gate 或复杂分布式平台。

**NEEDS_HUMAN_DECISION：NO。**

## 5. Required Review Areas 覆盖

| # | Required Area | 本次判断 |
|---|---|---|
| 1 | Product Goal / Core Loop | 一致；保留 Forward Outcome/Review |
| 2 | Trading exclusions 语义逃逸 | 当前声明边界明确；未发现直接授权交易的条款 |
| 3 | Forward Outcome 与 Paper Trading | 允许价格路径统计、禁止假想订单/账户；方向合理 |
| 4 | KEEP/ADAPT/EXCLUDE 过度复用 | 总体裁剪合理；P1-01 补齐来源能力限制 |
| 5 | transitive import / G3/G4 | 已核实关键耦合；不构成全闭包证明，迁移仍须 preflight |
| 6 | canonical/hashing | 独立抽取合理；P2-02 保留外围行为 |
| 7 | strength / priority / opportunity quality | P1-01、P2-01 |
| 8 | Evidence as-of / horizon | P1-02 |
| 9 | Confidence 可校准性 | C-08 语义合理；当前未验证校准效果 |
| 10 | miss-rate 非循环证明 | 独立 reference 思路正确；不把该范围外样本声称为总召回率 |
| 11 | WITH_AI / WITHOUT_AI pairing | P1-03 |
| 12 | AI Acceptance 独立性 | Worker 不可自验收的边界正确；不据此宣称工具端隔离已验证 |
| 13 | Fix fuse | P1-04 |
| 14 | Human authority | C-10 保留；本报告不改变该边界 |
| 15 | 不必要基础设施 | 未发现必须新增的平台级基础设施；保留单体方向 |
| 16 | P1 最小完整闭环 | WITHOUT_AI + Report/Delivery/History/due Outcome 合理；P1-01—03 必须进入后续 AC |
| 17 | P0 Freeze Gate | P1-05 |

这里的“合理/一致”是本次设计审查判断，不是局部或整体 ACCEPTED 状态。

## 6. 已实际抽查的源码范围

共同来源：`zwmhua2-lab2/MarketPulse-AI @ 7a023f99ac44ac744e5a9885fd82f8a87ef51a94`。以下路径均位于 `backend/src/marketpulse/`。指定长文件只读取风险相关范围，未声称通读。

| 文件 | 实际审查范围 |
|---|---|
| clock.py | 已返回的全文件；Clock/SystemClock/ReplayClock |
| market_data/models.py | 已返回的全文件；行情身份、时间、质量、Kline |
| market_data/normalizer.py | 已返回的全文件；_base/normalize_kline 等 |
| g2/scanner.py | 已返回的全文件；事件规则、strength、priority |
| g2/models.py | 已返回的全文件；DTO/enum/Advisory/lifecycle 混合语义 |
| g2/structure.py | 已返回的全文件；analyze_structure |
| edge/capture.py | 文件头、imports、ReferenceQuote、EdgeCaptureContext/classify_reference；响应后部截断，未全读 |
| edge/models.py | L1–L230；EvidenceSource、交易归因 enum、identity 等 |
| edge/forward.py | L1–L260；policy、due_at、reference、endpoint ordering |
| edge/aggregation.py | L1–L240；canonical head、coverage/measurement、统计边界 |
| edge/attribution.py | L1–L170；交易链及 G3/DB 耦合 |
| edge/hashing.py | 请求 L1–L240，返回文件全部内容；canonical bytes/digest/拒绝策略 |
| g5/canonical.py | L1–L250；canonicalize_value 与 workspace 混合依赖 |
| runtime.py | 请求 L1–L230，返回文件全部内容；readiness/health/supervision |
| runtime_host.py | L1–L215；composition ownership、imports、交易依赖 |
| advisory/contracts.py | L1–L230；Gate taxonomy/config/authority |
| advisory/identity.py | L1–L200；identity/input binding |
| advisory/attempts.py | 请求 L1–L200，返回文件全部内容；DB/G2/fence 耦合 |
| advisory/predicates.py | 请求 L1–L220，返回文件全部内容；DB snapshot/state/activation 耦合 |
| advisory/worker.py | L1–L230；fake provider、durable finalizer 依赖 |
| g2/rolling.py（补充） | 请求 L1–L240，返回文件全部内容；Feature producer 与两个静态反例 |
| g2/runtime.py（补充） | L360–L600 的返回内容；ScannerPipeline.ingest 调用链及相关辅助方法 |

关键裁剪的源码依据：runtime_host 直接依赖 PaperExecutionService、Position/TradePlan、G4 risk/protection/recovery；edge/attribution 直接观察交易链；g5/canonical 同时包含 PostgreSQL workspace snapshot；advisory 的 attempts/predicates/worker 依赖来源 durable graph。因此相应 EXCLUDE/CONCEPT/SUBSET 的方向有独立源码支持。

## 7. NOT VERIFIED

- 未运行任何测试；静态反例没有被称为 pytest/runtime evidence。
- 未构建目标项目；没有证明 QI runtime、AI provider、UI、推送或数据库已经可用。
- 未完成全仓 transitive dependency / package-init / build dependency 静态证明。
- 未完成所有拟迁移能力的 tests mapping。
- 未验证目标机器、DeepSeek 自动调用与回传、DevRelay 或浏览器全自动执行。
- 未验证真实市场质量、召回率、概率校准、AI 增益或业务达标阈值。
- 未独立确认新项目仓库是否存在；只记录输入声明的 PROJECT REPOSITORY NOT ESTABLISHED。
- 未核验来源仓库当前 main/tag，也未核验 Global Project Registry；本任务的源码审查基准是指定 SHA。
- 未独立审计所有历史 Human 授权记录；本审查依据输入明示边界及当前任务语境，不生成新的 Human 授权。
- 未采集端到端审查起止耗时，不从 commit 时间或工具时间补造。

## 8. Assumptions

- 本次上传文件是要求按其内置 QI-P0-R01 任务进行独立审查的候选，而非隐含授权业务开发。
- QI 是独立项目；MarketPulse 只是来源，不能被本任务修改。
- 普通设计/验收自治保持既有授权；Human-owned Constitution/治理/重大架构边界不因这次审查而被扩大。
- 未冻结的 DB、provider、universe、horizon 数值和质量阈值继续保持未冻结状态；本报告不代为选定。

## 9. Unreviewed Scope

MarketPulse 全仓系统安全和产品正确性；未抽查的 Universe/API/Frontend 文件；全量依赖闭包；运行性能/持续可靠性；目标 Schema/部署；实际模型服务能力；新项目全部实现；Global Protocol 全量合规及 Registry 写入。

## 10. Next Authorized Design Step

按候选 §13 的失败分支进入：

`P0-04｜Sol Extra High Findings Closure`

处理 P1-01—P1-05，并把 P2-01—P2-03 纳入对应迁移验收前置条件。输出新候选版本、逐项 closure matrix、相对 v0.2 的 diff、实际文件 digest，再做必要独立 re-review。

本报告不修改 Constitution，不冻结新决策，不创建仓库，不授权 DeepSeek 编码，不启动行情/AI/推送，也不标记 P0 ACCEPTED。源 MarketPulse 的两个具体算法行为只在本次报告中作为迁移风险记录；任何对原项目的修复都必须另走原项目任务。

---

Result：FIX REQUIRED  
Changes：只生成本独立审查报告；输入文件和仓库无修改。  
Verification：候选实际 bytes/digest 校验；pinned commit 读取；20 个指定源码文件定点抽查；2 个关联源码文件调用链核验；静态逻辑反例。  
Remaining：5 个 P0 阻断设计处置项、3 个后续迁移验收项；无实现授权。  
Evidence：本报告输入指纹、来源 SHA、Finding 证据与源码范围清单。
