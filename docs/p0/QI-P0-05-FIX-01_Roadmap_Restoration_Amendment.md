# QI-P0-05-FIX-01 — Roadmap Restoration Amendment

```text
Document ID:        QI-P0-DOC-FIX01-ROADMAP-AMENDMENT
Amendment ID:       QI-P0-05-FIX-01
Parent Task:        QI-P0-05-I01
Problem Family:     QI-P0-05-ROADMAP-RESTORATION
Status:             REVIEW CANDIDATE
Phase:              P0 — Project Bootstrap
P0 Status:          NOT ACCEPTED
Business Implementation Authorization: NONE
Change type:        DOCUMENTATION FIX ONLY — restoration of previously defined content
```

---

# 1. Why this amendment exists

`QI-P0-RC-0.3` updated the P0 roadmap while closing the `QI-P0-R01` findings, but it did
**not** restate the P1–P4 phase definitions that already existed in the preceding candidate
`QI-P0-RC-0.2` §9.

This was **not** an intentional deletion and **not** a new product decision. It was an
omission, surfaced during `QI-P0-05-I01` and disclosed to Human as a source gap.

The interim `ROADMAP.md` consequently carried:

```text
P2   NOT DEFINED IN AUTHORITATIVE INPUT (v0.3)
P3   NOT DEFINED IN AUTHORITATIVE INPUT (v0.3)
P4   NOT DEFINED IN AUTHORITATIVE INPUT (v0.3)
```

That state was unacceptable before `P0-06 GPT-6 Final P0 Re-Review`, and this amendment
removes it by restoring the original definitions.

---

# 2. Restoration source (authoritative)

```text
Quant_Intelligence_P0_Review_Candidate_v0.2.md
  @ SHA-256 32833aa3a3f0ef18b3b2b6220aabb32a44f304cb76f0ffe5ff95339c39fbcdb3
Section 9 — Initial Roadmap — Review Candidate
```

Nothing was invented, extended or redesigned. The restored text below is the source text.

---

# 3. Exact restored text

## 3.1 Phase axis (restored)

```text
P0   Constitution / Governance / Bootstrap
P1   First Observable Intelligence Loop
P2   Deep AI
P3   Quality / UX
P4   V1 System Audit
```

## 3.2 P1｜First Observable Intelligence Loop

```text
真实行情
→ 少量 Scanner Event
→ Evidence
→ deterministic Decision baseline
→ Report
→ 单通知通道
→ 简体中文最小 UI
→ History
→ due Outcome
```

P1 明确标记 `WITHOUT_AI BASELINE`。

## 3.3 P2｜Deep AI

加入：

- 单一真实 Provider；
- frozen prompt/schema；
- structured output；
- timeout/retry；
- failure downgrade；
- WITH_AI / WITHOUT_AI paired evaluation。

## 3.4 P3｜Quality / UX

完善：

- priority；
- dedupe；
- coverage；
- miss-rate reference；
- calibration；
- type outcome；
- history/report experience。

## 3.5 P4｜V1 System Audit

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

# 4. v0.3 content is NOT rolled back

This restores a **phase axis**. It does **not** revert any `QI-P0-RC-0.3` decision.

The `v0.3` P1 First-Slice Guardrails remain fully authoritative and are preserved without
weakening in `ROADMAP.md` Part D:

```text
FAILED_BREAKOUT 不可直接迁移
旧 VOLUME_ANOMALY 不可直接迁移
其它旧 Detector 也必须重新证明 Feature 定义
P1 不因来源代码已存在而跳过语义验收
P1 保持小范围、少事件类型
first slice 必须包含 History、due Outcome、causal timestamps、
  exact version binding、coverage/missing status
P1 明确标记 WITHOUT_AI BASELINE
```

Resulting roadmap semantics:

```text
v0.2 phase axis  +  v0.3 strengthened guardrails
```

Not a rollback of v0.3.

---

# 5. Boundary statements

## 5.1 No Human-owned change

```text
NEEDS_HUMAN_DECISION: NO
```

This amendment does not change Product Goal, Core Product Loop, Trading boundary, Major
Architecture invariant, or Autonomous Governance rules. It restores previously defined
content that was inadvertently dropped.

## 5.2 No business authorization

```text
Business Implementation Authorization = NONE
P1 / P2 / P3 / P4 = NOT IMPLEMENTATION AUTHORIZED
```

No P1 formal task is created, no P1 work is started, and nothing here authorizes
implementation of any phase beyond P0.

## 5.3 Content ceiling

Restored content does not exceed the `v0.2` §9 phase definitions. No P2/P3/P4 capability
beyond that source text is introduced.

---

# 6. Effect on the P0 review candidate

```text
Previous Candidate SHA:
  39f4b2dba440470717b04fd15e8a114fb2ee939f   ← NO LONGER the P0-06 review target

New Final Candidate SHA:
  = tip commit of p0/bootstrap-candidate after this amendment
    (published in the QI-P0-05-FIX-01 completion report; see P0_BASELINE_MANIFEST.md §1a)
```

Because P0 canonical content changed semantically, the previous candidate SHA must not be
used as the `P0-06` review target. No review verdict had been issued against it, so no prior
PASS lapses — there was none.

---

# 7. Files touched by this amendment

```text
ROADMAP.md                                            (P1–P4 phase definitions restored)
PROJECT_STATE.md                                      (task status advanced)
P0_BASELINE_MANIFEST.md                               (new candidate SHA + refreshed blob SHAs)
reviews/README.md                                     (fix provenance note)
docs/p0/QI-P0-05-FIX-01_Roadmap_Restoration_Amendment.md   (this record)
docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.2.md      (restoration source artifact, added)
```

Untouched, as required:

```text
PRODUCT_CONSTITUTION.md
AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md
ARCHITECTURE.md
EVALUATION_CONTRACT.md
DERIVATION_MANIFEST.md
docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md
reviews/QI-P0-R01_Independent_Critical_Review_Result.md
```

---

# 8. Provenance

```text
Created by:  QI-P0-05-FIX-01
Date:        2026-09-23
Scope:       restore previously defined P1–P4 roadmap content only
Business code created: NO
MarketPulse modified: NO
Global Protocol modified: NO
```
