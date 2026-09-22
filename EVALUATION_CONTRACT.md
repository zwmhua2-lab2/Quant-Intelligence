# Quant Intelligence — EVALUATION CONTRACT

```text
Document ID:        QI-P0-DOC-EVALUATION-CONTRACT
Status:             ACCEPTED
Phase:              P0 — Accepted Baseline
P0 Status:          ACCEPTED
Business Implementation Authorization: NONE
Source of truth:    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md (§6, §9)
Clause range:       S-01 … S-04, E-01 … E-10
```

> Faithful split of `QI-P0-RC-0.3`. Evaluation semantics are preserved exactly.
>
> **A complete funnel log is not a correct statistical denominator.** The distinction
> between the eligible cohort and the published cohort must never be collapsed, and
> `published samples` must never be substituted for `all evaluation samples`.

---

# Part A — Score Semantics Contract

> 吸收非阻断 Finding `P2-01`。

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

# Part B — Confidence Definition

> Normative owner of this definition is `PRODUCT_CONSTITUTION.md` **C-08**.
> This restatement exists because the Evaluation Contract must bind it; if the two ever
> diverge, C-08 governs.

概率型 Confidence 只有在预测对象被明确冻结后才合法：

`P(定义事件在固定 horizon 内发生 | 当时 Evidence)`

文字语气不等于概率。

未完成校准前，不得把模型自报 `0.8` 写成“80% 胜率”。

---

# Part C — Evaluation Cohort & Version Contract

> 关闭 `QI-P0-R01` / `P1-03`。

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

## Cross-references

```text
S-01 … S-04 score semantics        → mandatory AC preconditions in DERIVATION_MANIFEST.md (MAP-01)
S-04 / Part B confidence            → constitutionally owned by PRODUCT_CONSTITUTION.md (C-08)
E-04 paired evaluation cutoff       → causal invariants in ARCHITECTURE.md (T-01, T-03)
E-07 version binding                → architecture logical chain in ARCHITECTURE.md (A-04)
E-10 wrong: candidate cohort is not a recall proof
```
