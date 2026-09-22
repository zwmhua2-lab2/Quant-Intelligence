# Quant Intelligence — ROADMAP

```text
Document ID:        QI-P0-DOC-ROADMAP
Status:             REVIEW CANDIDATE
Phase:              P0 — Project Bootstrap
P0 Status:          NOT ACCEPTED
Business Implementation Authorization: NONE
Source of truth:    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md (§13, §14, §17, §18, §19)
```

---

# Part A — Phase Axis

```text
P0   Constitution / Governance / Bootstrap      ← CURRENT PHASE
P1   First Observable Intelligence Loop         ← GUARDRAILS ONLY, NOT AUTHORIZED
P2   NOT DEFINED IN AUTHORITATIVE INPUT (v0.3)
P3   NOT DEFINED IN AUTHORITATIVE INPUT (v0.3)
P4   NOT DEFINED IN AUTHORITATIVE INPUT (v0.3)
```

**No phase beyond P0 is authorized for implementation.** Nothing in this document
authorizes P1, P2, P3 or P4 implementation work.

## SOURCE GAP — mandatory disclosure

The authoritative input for this bootstrap, `QI-P0-RC-0.3`, defines:

- the **P0** roadmap in full (§14, `P0-01 … P0-07`);
- **P1** only as *First-Slice Guardrails* (§13), and as a referenced target of future
  Formal Requirements (§13, §17 item 9/10, §18 item 17/18, `KSL-02`);
- **P2**, **P3**, **P4** — **not at all**.

Provenance note (factual, not adopted content): predecessor candidate `QI-P0-RC-0.2` §9
carried a four-phase outline (`P1 First Observable Intelligence Loop`, `P2 Deep AI`,
`P3 Quality / UX`, `P4 V1 System Audit`). `v0.3` §14 is titled *"Updated P0 Roadmap"* and
restates only the P0 portion; `v0.3` §16 "Changed" does **not** record a removal of the
P1–P4 outline.

This bootstrap does **not** import the predecessor's P1–P4 definitions into the P0
candidate, and does **not** invent replacement content. The P1–P4 scope therefore remains
**UNRESOLVED and NOT AUTHORIZED** until a future Human-authorized design step defines it.

---

# Part B — P0 Roadmap

## P0-01

Initial Constitution / Governance / Architecture Draft — **DONE**

## P0-02

Derivation Review Input Closure / RC v0.2 — **DONE**

## P0-03

GPT-6 Independent Critical Review `QI-P0-R01` — **DONE / FIX REQUIRED**

## P0-04

Sol Findings Closure / RC v0.3 — **DONE**

## P0-05

Document-only Repository Bootstrap Candidate — **CURRENT**

目标：

- 建立 Quant Intelligence Repository；
- 只写 P0 canonical docs / governance / state；
- 不写业务实现；
- 记录 source references；
- 形成真实 Candidate Git SHA。

## P0-06

GPT-6 Final P0 Re-Review — **NOT STARTED**

审查对象：

**Repository Candidate SHA**

不是聊天摘要，不是 Worker Report，不是 v0.2 verdict 的延伸。

若 PASS，进入 P0-07；若 FIX REQUIRED，返回 Sol Findings Closure。

## P0-07

P0 Final Freeze / Registry / Accepted Baseline — **NOT STARTED**

只有完成 exact reviewed Candidate、required closeout、Registry、baseline pointers，才能：

`P0 = ACCEPTED`

之后才设计 P1 Formal Task。

### P0 status summary

```text
P0-01  DONE
P0-02  DONE
P0-03  DONE / FIX REQUIRED
P0-04  DONE
P0-05  CURRENT
P0-06  NOT STARTED
P0-07  NOT STARTED
```

---

# Part C — P1 First-Slice Guardrails

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

# Part D — P0 Freeze Gate

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

# Part E — NOT VERIFIED

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

> Item 1 (repository establishment) was resolved by the separate P0-05 pre-step recorded in
> `PROJECT_STATE.md`. All other items remain open. Item 12 is now scheduled as `P0-06`.
