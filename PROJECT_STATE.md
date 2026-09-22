# Quant Intelligence — PROJECT STATE

```text
Document ID:        QI-P0-DOC-PROJECT-STATE
Status:             REVIEW CANDIDATE
Phase:              P0 — Project Bootstrap
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
  P0 — Project Bootstrap

Current Task:
  QI-P0-05-FIX-01  (COMPLETED — most recently executed task)
  QI-P0-05-I01     (COMPLETED)

P0 Status:
  NOT ACCEPTED

Business Implementation Authorization:
  NONE

MarketPulse Derivation Source:
  7a023f99ac44ac744e5a9885fd82f8a87ef51a94

P0 Source Candidate:
  QI-P0-RC-0.3

P0 Source Candidate SHA-256:
  3c24a09e92eca77bf4c6dbfd1dc473fd5955e6be6c5c547ba63f130cbbe71d86

QI-P0-R01:
  FIX REQUIRED

Blocking Findings:
  DESIGN CLOSED / FINAL RE-REVIEW REQUIRED

Next:
  P0-06 GPT-6 Final P0 Re-Review, bound to the Final Candidate Git SHA
  recorded in P0_BASELINE_MANIFEST.md
```

---

# 2. Two boundaries that must not be misread

```text
P0 = NOT ACCEPTED
Business Implementation Authorization = NONE
```

- `QI-P0-R01 = FIX REQUIRED` is **not** superseded by this bootstrap.
  The five blocking findings are recorded as `DESIGN CLOSED / RE-REVIEW REQUIRED`.
  `DESIGN CLOSED` does **not** mean a reviewer has accepted them. They may only be called
  `REVIEW CLOSED` after an independent Final Re-Review PASS.
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
P0-03  DONE / GPT-6 = FIX REQUIRED
P0-04  DONE
P0-05  COMPLETED   (P0-05-I01 + P0-05-FIX-01)
P0-06  NEXT / NOT STARTED
P0-07  NOT STARTED

P0 = NOT ACCEPTED
Business Implementation Authorization = NONE
DeepSeek Business Coding = FORBIDDEN
PROJECT REPOSITORY = ESTABLISHED (zwmhua2-lab2/Quant-Intelligence)
```

下一步：

**P0-06｜GPT-6 Final P0 Re-Review**

审查对象必须是 **真实 Repository Candidate Git SHA**，记录于
`P0_BASELINE_MANIFEST.md`。

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
