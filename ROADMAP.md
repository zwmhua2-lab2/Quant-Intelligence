# Quant Intelligence — ROADMAP

```text
Document ID:        QI-P0-DOC-ROADMAP
Status:             ACCEPTED
Phase:              P0 — Accepted Baseline
P0 Status:          ACCEPTED
Business Implementation Authorization: NONE
Source of truth:    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md (§13, §14, §17, §18, §19)
                    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.2.md §9   (P1–P4 phase axis)
                    docs/p0/QI-P0-05-FIX-01_Roadmap_Restoration_Amendment.md    (restoration provenance)
```

---

# Part A — Phase Axis

```text
P0   Constitution / Governance / Bootstrap        ← DONE / ACCEPTED
P1   First Observable Intelligence Loop
P2   Deep AI
P3   Quality / UX
P4   V1 System Audit
```

```text
P0     = ACCEPTED
P0-07  = DONE / ACCEPTED
P1     = DESIGN NEXT / NOT STARTED
P1–P4  = NOT IMPLEMENTATION AUTHORIZED
```

**No phase beyond P0 is authorized for implementation.** Nothing in this document
authorizes P1, P2, P3 or P4 implementation work, and no P1 formal task is created here.

The P1–P4 definitions below are **restored previously-defined content**, not a new design
decision and not an extension. Restoration rationale, source and exact text:
`docs/p0/QI-P0-05-FIX-01_Roadmap_Restoration_Amendment.md`.

---

# Part B — Phase Definitions

> Source: `QI-P0-RC-0.2` §9 *Initial Roadmap — Review Candidate* (restored verbatim).
> These phases are `NOT IMPLEMENTATION AUTHORIZED`.

## P0｜Constitution / Governance / Bootstrap

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

## P1｜First Observable Intelligence Loop

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

> P1 event/Detector selection is additionally constrained by Part D
> (v0.3 First-Slice Guardrails). Those guardrails remain authoritative and are not
> weakened by this restoration.

## P2｜Deep AI

加入：

- 单一真实 Provider；
- frozen prompt/schema；
- structured output；
- timeout/retry；
- failure downgrade；
- WITH_AI / WITHOUT_AI paired evaluation。

## P3｜Quality / UX

完善：

- priority；
- dedupe；
- coverage；
- miss-rate reference；
- calibration；
- type outcome；
- history/report experience。

## P4｜V1 System Audit

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

# Part C — P0 Roadmap

## P0-01

Initial Constitution / Governance / Architecture Draft — **DONE**

## P0-02

Derivation Review Input Closure / RC v0.2 — **DONE**

## P0-03

GPT-6 Independent Critical Review `QI-P0-R01` — **DONE / FIX REQUIRED**

## P0-04

Sol Findings Closure / RC v0.3 — **DONE**

## P0-05

Document-only Repository Bootstrap Candidate — **DONE**

目标：

- 建立 Quant Intelligence Repository；
- 只写 P0 canonical docs / governance / state；
- 不写业务实现；
- 记录 source references；
- 形成真实 Candidate Git SHA。

执行记录：

```text
P0-05-I01     Document-only Repository Bootstrap Candidate     → COMPLETED
P0-05-FIX-01  Roadmap Restoration (P1–P4 phase axis)           → COMPLETED
```

## P0-06

GPT-6 Final P0 Re-Review — **DONE / PASS**

审查对象：

**Repository Candidate SHA**

不是聊天摘要，不是 Worker Report，不是 v0.2 verdict 的延伸。

若 PASS，进入 P0-07；若 FIX REQUIRED，返回 Sol Findings Closure。

审查结果（status metadata）：

```text
Review Task:            QI-P0-06-R01
Reviewed Candidate SHA: f7ea873b9b61527af481646fb88f63f6a52f9a52
Verdict:                PASS
Blocking Findings:      0
Full artifact:          reviews/QI-P0-06-R01_Final_P0_ReReview_Result.md
```

## P0-07

P0 Final Freeze / Registry / Accepted Baseline — **DONE / ACCEPTED**

只有完成 exact reviewed Candidate、required closeout、Registry、baseline pointers，才能：

`P0 = ACCEPTED`

之后才设计 P1 Formal Task。

### P0 status summary

```text
P0-01  DONE
P0-02  DONE
P0-03  DONE / FIX REQUIRED
P0-04  DONE
P0-05  DONE
P0-06  DONE / PASS
P0-07  DONE / ACCEPTED

P0 = ACCEPTED
```

---

# Part D — P1 First-Slice Guardrails

> Source: `QI-P0-RC-0.3` §13. Authoritative and unchanged by the roadmap restoration.

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

# Part E — P0 Freeze Gate

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

# Part F — NOT VERIFIED

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
12. v0.3 Independent Re-Review — PERFORMED as `P0-06` on the repository candidate
    (`f7ea873b…`, verdict `PASS`); outside-repo archive byte-level SHA-256
    recomputation still NOT PERFORMED。
13. P0 Accepted Baseline — AVAILABLE（`refs/tags/p0-v1.0.0`）。

> Item 1 (repository establishment) was resolved by the `P0-05` pre-step recorded in
> `PROJECT_STATE.md`. Item 12 is now `P0-06` and has PASSed on the repository candidate.
> Item 13 was resolved by `P0-07-I02` (accepted baseline tag `p0-v1.0.0`).
> All other items remain open.
