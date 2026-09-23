# Quant Intelligence — PROJECT STATE

```text
Document ID:        QI-P0-DOC-PROJECT-STATE
Status:             ACCEPTED
Phase:              P0 — Accepted Baseline
Source of truth:    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md (§1, §19)
                   + P0-05 Repository Precondition Update
                   + docs/p0/QI-P0-05-FIX-01_Roadmap_Restoration_Amendment.md
```

> This is a **current-state snapshot**, not a full history.

---

# 1. Current State Snapshot

```text
Project:                                Quant Intelligence

Phase:
  P0 — Accepted Baseline

Current Task:
  QI-P0-07-I02-FIX-01 (DONE — Final Freeze Consistency + Local Scope Cleanup)
  QI-P0-07-I02     (COMPLETED — Final Freeze / Registry / Accepted Baseline;
                    its p0-v1.0.0 freeze tag was superseded by I02-FIX-01)
  QI-P0-07-I01     (COMPLETED — AI-accepted P0 acceptance closeout candidate)
  QI-P0-06-R01     (COMPLETED — Final P0 Re-Review, verdict PASS)

P0 Status:
  ACCEPTED
  Acceptance Authority: DELEGATED_AI

Business Implementation Authorization:
  NONE

MarketPulse Derivation Source:
  7a023f99ac44ac744e5a9885fd82f8a87ef51a94

P0 Source Candidate:
  QI-P0-RC-0.3

P0 Source Candidate SHA-256:
  3c24a09e92eca77bf4c6dbfd1dc473fd5955e6be6c5c547ba63f130cbbe71d86

QI-P0-R01:
  FIX REQUIRED  (verdict on QI-P0-RC-0.2 — not erased by later revisions)
  Finding closure: 8 / 8 CLOSED at P0 design / migration-contract level

GPT-6 Reviewed SHA:
  f7ea873b9b61527af481646fb88f63f6a52f9a52

Verdict:
  PASS

Blocking Findings:
  0

P0-07-I01 Closeout SHA:
  fabe3616bec75b2212b51d15ea37485377de5145

Global Registry Commit:
  08031c45dbd735ebad911e96d17a4a042788fa95

Previous Final Freeze Attempt:
  p0-v1.0.0
  → 8417a189d5fcc9d9b89e41d2344574f5205fd2fe
  Status: SUPERSEDED FINAL-FREEZE ATTEMPT
  Reason: non-semantic Context Entry state inconsistency in AI_BOOTSTRAP.md

Authoritative Accepted Baseline:
  p0-v1.0.1
  → = commit referenced by refs/tags/p0-v1.0.1

Supersession reason:
  p0-v1.0.0 was superseded only because the AI_BOOTSTRAP accepted-state text remained stale.
  No Product / Governance / Architecture / Derivation / Evaluation semantic change.
  The semantic core of the p0-v1.0.0 freeze was and remains valid.

Next:
  P1-04 Independent Requirement / Architecture Review
  (authoritative P1 state: see §7)
  NOT P1 Coding.
```

---

# 2. Two boundaries that must not be misread

```text
P0 = ACCEPTED
Business Implementation Authorization = NONE
```

- `QI-P0-R01 = FIX REQUIRED` is **not** superseded by this bootstrap.
  The five blocking findings were held at `DESIGN CLOSED / FINAL RE-REVIEW REQUIRED` until the
  independent Final Re-Review `QI-P0-06-R01` — bound to the exact candidate SHA
  `f7ea873b9b61527af481646fb88f63f6a52f9a52` and returned `PASS` — closed them.
  `DESIGN CLOSED` alone never meant a reviewer had accepted them; `REVIEW CLOSED` was reachable
  only through that independent Final Re-Review PASS.
- The `P0-05` tasks (`QI-P0-05-I01`, `QI-P0-05-FIX-01`) completing successfully are
  **not** P0 acceptance, are **not** a GPT-6 PASS, and do **not** authorize business coding.
- Restoring the P1–P4 roadmap phase definitions (`QI-P0-05-FIX-01`) does **not** authorize
  P1, P2, P3 or P4 implementation. It restores previously defined content only.

---

# 3. Repository state (P0-05 Repository Precondition Update)

| Item | Value |
|---|---|
| Repository | `zwmhua2-lab2/Quant-Intelligence` |
| Default branch | `main` |
| Candidate branch | `p0/bootstrap-candidate` |
| Repository type | Independent (not a fork of MarketPulse-AI) |
| Content type | Document-only P0 candidate |
| Business code | NONE |
| Trading capability | NONE |

The repository was created by Human and verified empty before bootstrap. This resolves
`v0.3 §17` item 1 (`Quant Intelligence Repository — NOT ESTABLISHED`) — see `ROADMAP.md`
Part F for the remaining open items.

---

# 4. P0-04 Round Record

> Source: `QI-P0-RC-0.3` §1, transcribed faithfully.

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

# 5. Status Snapshot

## 5a. Historical snapshot — as recorded in source candidate §19

> **HISTORICAL ONLY.** This is the state as of `QI-P0-RC-0.3` at the time it was written.
> It is retained for provenance and must **not** be read as the current status.
> The current, authoritative status is §5b.

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

## 5b. Current status (authoritative)

```text
P0-01  DONE
P0-02  DONE
P0-03  DONE
P0-04  DONE
P0-05  DONE
P0-06  DONE / PASS
P0-07  DONE / ACCEPTED

P0 = ACCEPTED

Business Implementation Authorization = NONE
DeepSeek Business Coding = FORBIDDEN

P1 = DESIGN
P1 Implementation Authorization = NONE
PROJECT REPOSITORY = ESTABLISHED (zwmhua2-lab2/Quant-Intelligence)
```

下一步：

**P1-04 Independent Requirement / Architecture Review**

P0 已 `ACCEPTED`。P1 设计候选已落库到 `p1/design-candidate`（`QI-P1-03-I01`），
但 `P1` 仍为 `DESIGN / NOT FROZEN`，`P1–P4` 均**未**获得实现授权。
下一步**不是** P1 Coding，也**不是** P1-I01。

---

# 6. Reading order

```text
1. AI_BOOTSTRAP.md
2. PROJECT_STATE.md
3. PRODUCT_CONSTITUTION.md
4. AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md
5. ARCHITECTURE.md
6. DERIVATION_MANIFEST.md
7. EVALUATION_CONTRACT.md
8. ROADMAP.md
9. P0_BASELINE_MANIFEST.md
```

Authoritative precedence:

```text
Repository Truth > Chat History > Memory > Worker Report
```

---

# 7. P1 Design Candidate State (authoritative for P1)

> Added by `QI-P1-03-I01` (document-only P1 design candidate persistence).
> This section is the **authoritative P1 current-state pointer**. It does not change
> any P0 state above.

```text
P0 = ACCEPTED

P1-01 = DESIGN COMPLETE
P1-02 = DESIGN COMPLETE
P1-03 = DESIGN CANDIDATE PERSISTED / REVIEW PENDING

P1 Requirement / Architecture = NOT FROZEN
P1 Implementation Authorization = NONE
Business Coding = FORBIDDEN

Next =
P1-04 Independent Requirement / Architecture Review
```

```text
P1 Candidate Branch:  p1/design-candidate
P1 Design Input:      docs/p1/QI-P1_Design_Input_v0.1.md
P1 Design Candidate:  P1 DESIGN REVIEW CANDIDATE / NOT FROZEN
```

Canonical P1 documents (design review candidate, not frozen):

```text
P1_REQUIREMENTS.md
P1_ARCHITECTURE.md
P1_DATA_CONTRACT.md
P1_REUSE_MANIFEST.md
P1_IMPLEMENTATION_PLAN.md   (PLANNED ONLY — NOT IMPLEMENTATION AUTHORIZATION)
P1_DESIGN_MANIFEST.md
docs/p1/README.md
dev_log/QI-P1-03-I01.md
```

## 7a. P1 reading order

```text
1. docs/p1/README.md
2. docs/p1/QI-P1_Design_Input_v0.1.md   (authoritative source artifact)
3. P1_REQUIREMENTS.md
4. P1_ARCHITECTURE.md
5. P1_DATA_CONTRACT.md
6. P1_REUSE_MANIFEST.md
7. P1_IMPLEMENTATION_PLAN.md
8. P1_DESIGN_MANIFEST.md
```

## 7b. Boundaries that must not be misread

```text
P1-03 (design candidate persisted)  ≠  P1 design frozen
P1 design candidate persisted       ≠  P1 implementation authorized
P1-04 review PASS is required       ≠  a start of P1-I01
```

- A persisted design candidate is **not** a frozen design. `P1-04` must return a verdict
  before P1-05 Freeze can be considered.
- `P1 Implementation Authorization` remains `NONE` and `Business Coding` remains
  `FORBIDDEN` throughout P1-04.
- `P1-I01` … `P1-I04` are **planned only**; none is started or authorized.
