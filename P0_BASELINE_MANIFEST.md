# Quant Intelligence — P0 BASELINE MANIFEST

```text
Document ID:        QI-P0-DOC-BASELINE-MANIFEST
Status:             REVIEW CANDIDATE
Phase:              P0 — Project Bootstrap
```

> **Status is `REVIEW CANDIDATE`, NOT `ACCEPTED`.**
> No accepted P0 baseline exists. Nothing here may be read as P0 acceptance.

---

# 1. Baseline Identification

```text
Project:
  Quant Intelligence

Repository:
  zwmhua2-lab2/Quant-Intelligence

Candidate Branch:
  p0/bootstrap-candidate

Base SHA (candidate branch point = main initialisation commit):
  3b37624f7ca6b99cf30506be1da9605b93a1a4f8

Bootstrap Content Commit SHA:
  9c095bdce0674d145cd9add74db68c9a6781acb9

Previous Candidate SHA (QI-P0-05-I01 — superseded, see §1a):
  39f4b2dba440470717b04fd15e8a114fb2ee939f

Reviewed Final Candidate Git SHA (QI-P0-06-R01 review target — immutable):
  f7ea873b9b61527af481646fb88f63f6a52f9a52

P0-07 Closeout Candidate SHA:
  = tip commit of branch p0/bootstrap-candidate (see §1a)
```

## 1a. Self-reference note (read this before binding a review)

A Git object cannot contain its own hash. Therefore:

- `P0-07 Closeout Candidate SHA` is the **tip of `p0/bootstrap-candidate`**, i.e. the commit
  that introduces this manifest text. It cannot be written literally inside this file.
- Resolution procedure (deterministic, no chat context needed):

```bash
git ls-remote https://github.com/zwmhua2-lab2/Quant-Intelligence.git refs/heads/p0/bootstrap-candidate
```

- The literal value obtained from that command at hand-off is published in the
  `QI-P0-07-I01` completion report. It is **not** duplicated into this file, for the
  reason above.
- The **reviewed** candidate is a past, immutable commit and is therefore recorded literally:
  `f7ea873b9b61527af481646fb88f63f6a52f9a52`. The `QI-P0-06-R01` `PASS` binds that SHA and
  nothing later.
- **`39f4b2dba440470717b04fd15e8a114fb2ee939f` must no longer be used as the `P0-06` review
  target.** P0 canonical content changed semantically in `QI-P0-05-FIX-01` (P1–P4 roadmap
  restoration), so the previous candidate is superseded.
- The commit history of this branch is exactly five commits:

```text
<Base SHA>                 main initialisation  (3b37624…)
  └─ <Bootstrap Commit>    bootstrap Quant Intelligence P0 review candidate  (9c095bd…)
       └─ <Prev Candidate> finalize P0 candidate manifest  (39f4b2d… — SUPERSEDED)
            └─ <Reviewed Candidate> fix: restore P1–P4 roadmap baseline  (f7ea873… — REVIEWED, PASS)
                 └─ <Closeout Candidate> prepare P0 acceptance closeout candidate  ← CURRENT TIP
```

---

# 2. Source Artifacts

```text
P0 Source Candidate:
  QI-P0-RC-0.3

P0 Source Artifact Path:
  docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md

P0 Source Artifact SHA-256:
  3c24a09e92eca77bf4c6dbfd1dc473fd5955e6be6c5c547ba63f130cbbe71d86


Roadmap Restoration Source (QI-P0-05-FIX-01):
  QI-P0-RC-0.2  §9 Initial Roadmap — Review Candidate

Roadmap Restoration Source Path:
  docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.2.md

Roadmap Restoration Source SHA-256:
  32833aa3a3f0ef18b3b2b6220aabb32a44f304cb76f0ffe5ff95339c39fbcdb3


MarketPulse Derivation Source:
  zwmhua2-lab2/MarketPulse-AI
  7a023f99ac44ac744e5a9885fd82f8a87ef51a94

Global Protocol reference:
  zwmhua2-lab2/ai-dev-protocol @ protocol-v1.4.5
  commit 1d17ba0e986c128715cbec7763f734696fb0956d
```

Both source artifacts are stored **byte-for-byte** as delivered; neither is reformatted,
renormalised or regenerated. Their SHA-256 values were re-verified **inside the Git object
database** (the bytes GitHub serves), not only in the working tree.

---

# 3. Canonical File Set

Canonical P0 documents (the review target set):

| # | Path | Blob SHA |
|---|---|---|
| 1 | `AI_BOOTSTRAP.md` | `1c02090f26a3df3a11ea870b08e537c87c62ed6f` |
| 2 | `PRODUCT_CONSTITUTION.md` | `ab66232722d79c6c4f57089e4e8aeffb6e167550` |
| 3 | `AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md` | `3077c167262d50e97b3df552f434151f4c5719d6` |
| 4 | `ARCHITECTURE.md` | `2ad1cbf4b044c48dfb48ecc7acd6755b3c6115f4` |
| 5 | `DERIVATION_MANIFEST.md` | `c8a3dbdd8eada38d9e8d0f7cb4d5c0949e81531f` |
| 6 | `EVALUATION_CONTRACT.md` | `f19aec35a0039367c01956c60100ba32e80b15e2` |
| 7 | `ROADMAP.md` | `6e25e4c8bcc70d07885b6960bccd8a4784b7049f` |
| 8 | `PROJECT_STATE.md` | `6630b8e7683a3c4b4b500adfe6b28f0b8a4dffbb` |
| 9 | `P0_BASELINE_MANIFEST.md` | *(self — see §1a; obtain via `git ls-tree -r <Closeout Candidate SHA>`)* |

Preserved source artifacts and review records:

| # | Path | Blob SHA |
|---|---|---|
| 10 | `docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md` | `4557aea05a9b2f6b0e296417a445bcc5103bdede` |
| 11 | `docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.2.md` | `7224569c8a8b65570a2d6146f27ad16c4be50fad` |
| 12 | `docs/p0/QI-P0-05-FIX-01_Roadmap_Restoration_Amendment.md` | `6ff13f3398e12e76f922270a6f839dcc98c1e6a3` |
| 13 | `reviews/README.md` | `d8740216ad4e8334b15e972f47aad84d53899da4` |
| 14 | `reviews/QI-P0-R01_Independent_Critical_Review_Result.md` | `3a234f47b4d6701a0b39ec2bf0031a4ea0eb6a29` |

Closeout artifacts added by `QI-P0-07-I01`:

| # | Path | Blob SHA |
|---|---|---|
| 15 | `reviews/QI-P0-06-R01_Final_P0_ReReview_Result.md` | `83689528a73bef3a8130b0b92a4e47d0f8e1026e` |
| 16 | `docs/p0/QI-P0-07-I01_Closeout_Diff_Classification.md` | *(new in this closeout — obtain via `git ls-tree -r <Closeout Candidate SHA>`)* |

Rows 7, 8, 12 and 13 were refreshed by `QI-P0-07-I01` (status metadata, documentation
accuracy, review provenance). The five semantic invariants — rows 2, 3, 4, 5 and 6 — are
**unchanged** from the reviewed candidate `f7ea873b…`; the `QI-P0-06-R01` `PASS` therefore
remains valid for them. See `docs/p0/QI-P0-07-I01_Closeout_Diff_Classification.md`.

Bootstrap support files (not canonical P0 content):

| Path | Blob SHA |
|---|---|
| `README.md` | `78dc3e52c8fe60c11c6dde2381f10835a6203f2a` |
| `.gitignore` | `864cb918005990c43726e19db60c0857632e98d2` |

---

# 4. Review Status

```text
QI-P0-R01:
  FIX REQUIRED  (verdict on QI-P0-RC-0.2 — not erased by later revisions)
  Reviewer:              GPT-6 Astra Pro (independent session)
  Reviewed artifact:     QI-P0-RC-0.2
  Reviewed artifact SHA-256:
    32833aa3a3f0ef18b3b2b6220aabb32a44f304cb76f0ffe5ff95339c39fbcdb3
  Blocking findings:     5  (P1-01 … P1-05)
  Non-blocking findings: 3  (P2-01 … P2-03)
  Full artifact:         PRESENT — reviews/QI-P0-R01_Independent_Critical_Review_Result.md
  Closure status:        REVIEW CLOSED (by QI-P0-06-R01 PASS — see below)

GPT-6 Final Review — QI-P0-06-R01:
  Reviewer:              GPT-6 Astra Pro
  Reviewed Candidate SHA:
    f7ea873b9b61527af481646fb88f63f6a52f9a52
  Blocking Findings:     0
  Verdict:               PASS
  Non-blocking findings: 1  (QI-P0-06-R01-P2-01 — corrected by QI-P0-07-I01)
  Full artifact:         PRESENT — reviews/QI-P0-06-R01_Final_P0_ReReview_Result.md
  Full artifact SHA-256:
    9d5ac3ff473254138f33b08174199b142c9ea939fd0759a2d8ba43ebec6c7e48
  Full artifact Git blob SHA:
    83689528a73bef3a8130b0b92a4e47d0f8e1026e
  Full artifact bytes:   24362
  Prior finding closure: 8 / 8 CLOSED  (P1-01 … P1-05, P2-01 … P2-03)

Blocking Findings:
  NONE (QI-P0-06-R01)

Business Implementation Authorization:
  NONE

P0 Status:
  NOT ACCEPTED

P1 / P2 / P3 / P4 Status:
  NOT IMPLEMENTATION AUTHORIZED

Next Review:
  none scheduled before P0-07-I02. The QI-P0-07-I01 closeout candidate still requires
  Independent Closeout Verification and AI Acceptance — that is not a fresh GPT-6
  design review, and it does not itself accept P0.
```

> `QI-P0-R01` is a verdict on `QI-P0-RC-0.2` and is unaffected by later revisions.
> `QI-P0-06-R01` is a verdict on `f7ea873b…` only; it is **not** a verdict on the
> `P0-07` closeout candidate.

---

# 5. Known Source Limitations Carried Into This Baseline

```text
FAILED_BREAKOUT                          = NOT DIRECTLY PORTABLE
old relative_volume / VOLUME_ANOMALY     = NOT DIRECTLY PORTABLE
runtime_host.py                          = EXCLUDE AS SOURCE FILE
edge/attribution.py                      = EXCLUDE
g3/*                                     = EXCLUDE
g4/*                                     = EXCLUDE
```

Full statements in `DERIVATION_MANIFEST.md` (KSL-01, KSL-02, M-06).

---

# 6. Freeze rules attached to this manifest

1. A review PASS binds only the exact `Final Candidate Git SHA`.
2. Any semantic change to P0 canonical content after review invalidates the prior PASS.
3. Pure mechanical metadata / pointer changes must be recorded with an exact diff and an
   applicability judgement.
4. `MarketPulse` SHA must never be presented as the QI baseline.
5. This manifest must not be marked `ACCEPTED` before `P0-07`.
6. Superseded candidate SHAs must not be reused as a review target.

---

# 7. Provenance

```text
Created by:   QI-P0-05-I01 Document-only Repository Bootstrap Candidate
Amended by:   QI-P0-05-FIX-01 Roadmap Restoration (problem family QI-P0-05-ROADMAP-RESTORATION)
Amended by:   QI-P0-07-I01 P0 Acceptance Closeout Candidate
Date:         2026-09-23
Scope:        split / organize / persist / verify / commit only
Business code created: NO
```
