# Quant Intelligence — REVIEWS

```text
Status:             REVIEW CANDIDATE
Phase:              P0 — Project Bootstrap
P0 Status:          NOT ACCEPTED
Business Implementation Authorization: NONE
```

> This directory holds independent review records and findings-closure provenance.
> Review artifacts are preserved **byte-for-byte**. They are never regenerated,
> reconstructed from a summary, or rewritten.

---

# 1. Review Registry

| Review Task ID | Reviewer | Reviewed Artifact | Verdict | Full Artifact In Repo |
|---|---|---|---|---|
| `QI-P0-R01` | GPT-6 Astra Pro (independent session) | `QI-P0-RC-0.2` | `FIX REQUIRED` | **YES** |
| `QI-P0-06-R01` | GPT-6 Astra Pro | Repository candidate `f7ea873b9b61527af481646fb88f63f6a52f9a52` | `PASS` | **YES** |

---

# 2. QI-P0-R01

```text
QI-P0-R01 = completed externally
Verdict   = FIX REQUIRED
Full artifact = PRESENT IN THIS BOOTSTRAP INPUT
```

The original review result file was supplied as an input to the P0-05 bootstrap and is
stored here unmodified:

```text
Path:      reviews/QI-P0-R01_Independent_Critical_Review_Result.md
SHA-256:   b16fb10846f5b349d2d6792d42edb15027d8de1520f0e553ed779f3e6c4e349f
Bytes:     24484
```

## Review identity (as recorded in the artifact)

```text
Date:                  2026-09-23
Project:               Quant Intelligence
Review Task ID:        QI-P0-R01
Reviewed Artifact:     QI-P0-RC-0.2
Reviewer:              GPT-6 Astra Pro
Mode:                  READ_ONLY / CRITICAL REVIEW
Verdict:               FIX REQUIRED
Blocking Findings:     5  (P1-01 … P1-05)
Non-blocking Findings: 3  (P2-01 … P2-03)
P0:                    NOT ACCEPTED
Implementation Authorization: NONE
Reviewed artifact SHA-256:    32833aa3a3f0ef18b3b2b6220aabb32a44f304cb76f0ffe5ff95339c39fbcdb3
Source repository (pinned):   zwmhua2-lab2/MarketPulse-AI @ 7a023f99ac44ac744e5a9885fd82f8a87ef51a94
```

> `P1`/`P2` in the finding IDs denote **severity**, not roadmap phase. `P1` means the finding
> had to be addressed by design before any P0 freeze; it never meant "implement business
> code now".

---

# 3. QI-P0-R01 Closure Matrix

> Source: `QI-P0-RC-0.3` §15, transcribed faithfully.
> `DESIGN CLOSED` does not mean a reviewer has accepted the closure.

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

# 3a. QI-P0-06-R01 — Final P0 Re-Review and Finding Closure

> Added by `QI-P0-07-I01`. §3 above remains the faithful transcription of `QI-P0-RC-0.3` §15
> and is intentionally **not** rewritten; this section records the later closure outcome.

```text
Review Task ID:        QI-P0-06-R01
Reviewer:              GPT-6 Astra Pro
Reviewed Candidate SHA:
  f7ea873b9b61527af481646fb88f63f6a52f9a52
Reviewed Candidate Tree SHA:
  9ccc44986d5ca5b68ad13cadcaaca971480e5b60
Mode:                  READ_ONLY
Verdict:               PASS
Blocking Findings:     0
Full artifact:         PRESENT
Blocking Findings (P0):
  NONE
```

## Full artifact identity

```text
Path:      reviews/QI-P0-06-R01_Final_P0_ReReview_Result.md
SHA-256:   9d5ac3ff473254138f33b08174199b142c9ea939fd0759a2d8ba43ebec6c7e48
Blob SHA:  83689528a73bef3a8130b0b92a4e47d0f8e1026e
Bytes:     24362
```

Stored byte-for-byte as supplied (`cmp` verified). Not reformatted, not regenerated.

## Prior finding closure (as recorded by the review artifact)

| Prior Finding | Status |
|---|---|
| `P1-01` Feature producer / source algorithm risks | `CLOSED` |
| `P1-02` Cross-module causal time | `CLOSED` |
| `P1-03` Evaluation cohort / version binding | `CLOSED` |
| `P1-04` Autonomous fix / RCA fuse | `CLOSED` |
| `P1-05` Review-to-freeze identity | `CLOSED` |
| `P2-01` Score semantics | `CLOSED` |
| `P2-02` Canonical safety | `CLOSED` |
| `P2-03` Runtime health | `CLOSED` |

`CLOSED` here means: the independent re-review confirmed the P0 design disposition and the
future migration acceptance constraints. It does **not** mean business implementation exists,
tests ran, source code was fixed, or any P1–P4 phase is authorized.

## New non-blocking finding introduced by this review

```text
Finding ID:  QI-P0-06-R01-P2-01
Severity:    P2 / NON-BLOCKING P0 FREEZE
Nature:      documentation-accuracy issue in
             docs/p0/QI-P0-05-FIX-01_Roadmap_Restoration_Amendment.md
             (a) §4 navigation pointed First-Slice Guardrails at ROADMAP.md Part C
                 (Part C is the P0 Roadmap; the guardrails are in Part D)
             (b) §7 touched-files list omitted the added restoration source artifact
                 docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.2.md
Correction:  applied by QI-P0-07-I01
Status:      CORRECTED / NON-SEMANTIC / CLOSEOUT VERIFICATION REQUIRED
Human decision required: NO
```

Correction diff and classification:
`../docs/p0/QI-P0-07-I01_Closeout_Diff_Classification.md`.

No other finding is recorded by `QI-P0-06-R01`.

---

# 4. Semantic Diff Summary — v0.2 → v0.3

> Source: `QI-P0-RC-0.3` §16, transcribed faithfully.
> Recorded here as closure provenance; the authoritative content lives in the canonical
> documents listed beside each item.

## Added

- Causal Time Contract → `ARCHITECTURE.md` Part B
- Evaluation Cohort & Version Contract → `EVALUATION_CONTRACT.md` Part C
- Score Semantics Contract → `EVALUATION_CONTRACT.md` Part A
- Runtime Health Contract → `ARCHITECTURE.md` Part C
- explicit `rolling.py` derivation decision → `DERIVATION_MANIFEST.md` M-02
- explicit `g2/runtime.py` exclusion → `DERIVATION_MANIFEST.md` M-02
- known-source-limitations registry → `DERIVATION_MANIFEST.md` KSL-01 … KSL-03
- Post-RCA hard attempt budget → `AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md` G-06, G-07
- final review binding to Repository Candidate SHA → `AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md` FZ-03
- P0 document-only bootstrap before Final Re-Review → `ROADMAP.md` P0-05, P0-06
- non-finite canonical migration acceptance → `DERIVATION_MANIFEST.md` Part B, MAP-02
- exact original/revised evaluation separation → `EVALUATION_CONTRACT.md` E-08

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

# 5. Binding requirement for the next review

```text
GPT-6 Final P0 Re-Review (P0-06) MUST bind the exact Final Candidate Git SHA
recorded in ../P0_BASELINE_MANIFEST.md.
```

A verdict is valid only for the tuple it was issued against. If P0 canonical content
changes semantically after the review, the verdict lapses and a new review is required.

---

# 6. Candidate Revision History

> Recorded so a reviewer can tell whether the artifact set changed after any verdict.

| Revision | Candidate SHA | Content change | Verdict issued |
|---|---|---|---|
| `QI-P0-05-I01` | `39f4b2dba440470717b04fd15e8a114fb2ee939f` | Initial document-only P0 candidate | none |
| `QI-P0-05-FIX-01` | see `../P0_BASELINE_MANIFEST.md` §1a — head of `p0/bootstrap-candidate` | Restored P1–P4 roadmap phase definitions from `../docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.2.md` §9; advanced `P0-05` status | `f7ea873b9b61527af481646fb88f63f6a52f9a52` = `QI-P0-06-R01` `PASS` |
| `QI-P0-07-I01` | see `../P0_BASELINE_MANIFEST.md` §1a — head of `p0/bootstrap-candidate` | Closeout candidate: persisted the `QI-P0-06-R01` review artifact, applied the `QI-P0-06-R01-P2-01` documentation-accuracy correction, updated review registry / status metadata / baseline pointers | none (pending Independent Closeout Verification) |

`QI-P0-05-FIX-01` (problem family `QI-P0-05-ROADMAP-RESTORATION`) is a **documentation fix**.
It restores previously defined roadmap content that `QI-P0-RC-0.3` omitted. It is not a new
Product Constitution decision, and it does not authorize P1–P4 implementation.
Provenance: `../docs/p0/QI-P0-05-FIX-01_Roadmap_Restoration_Amendment.md`.

A verdict is valid only for the tuple it was issued against. **`QI-P0-06-R01` issued `PASS`
against revision `QI-P0-05-FIX-01` (`f7ea873b…`)**; that verdict binds only that exact SHA.
`QI-P0-R01 = FIX REQUIRED` remains a verdict on `QI-P0-RC-0.2` and is unaffected.
The `QI-P0-07-I01` closeout candidate changes only review provenance, status metadata,
documentation accuracy and hash pointers — see
`../docs/p0/QI-P0-07-I01_Closeout_Diff_Classification.md` for the exact diff and applicability
judgement required by `AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md` FZ-04.
