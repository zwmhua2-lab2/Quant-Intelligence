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

Manifest Finalization Commit SHA:
  = tip commit of branch p0/bootstrap-candidate (see §1a)

Final Candidate Git SHA:
  = tip commit of branch p0/bootstrap-candidate (see §1a)
```

## 1a. Self-reference note (read this before binding a review)

A Git object cannot contain its own hash. Therefore:

- `Manifest Finalization Commit SHA` and `Final Candidate Git SHA` are the **same commit** —
  the tip of `p0/bootstrap-candidate`, i.e. the commit that introduces this manifest text.
- It is the **last commit** on the candidate branch, exactly as required: the review target
  is the last commit's exact Git SHA.
- Resolution procedure (deterministic, no chat context needed):

```bash
git ls-remote https://github.com/zwmhua2-lab2/Quant-Intelligence.git refs/heads/p0/bootstrap-candidate
```

- The literal value obtained from that command at hand-off is published in the
  `QI-P0-05-I01` completion report. It is **not** duplicated into this file, for the
  reason above.
- The commit history is exactly two commits on this branch:

```text
<Base SHA>                main initialisation  (3b37624…)
  └─ <Bootstrap Commit>   bootstrap Quant Intelligence P0 review candidate  (9c095bd…)
       └─ <Final Candidate> finalize P0 candidate manifest  ← REVIEW TARGET
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

MarketPulse Derivation Source:
  zwmhua2-lab2/MarketPulse-AI
  7a023f99ac44ac744e5a9885fd82f8a87ef51a94

Global Protocol reference:
  zwmhua2-lab2/ai-dev-protocol @ protocol-v1.4.5
  commit 1d17ba0e986c128715cbec7763f734696fb0956d
```

The P0 source artifact is stored **byte-for-byte** as delivered; it is not reformatted,
renormalised or regenerated. Its SHA-256 was re-verified **inside the Git object database**
(the bytes GitHub serves), not only in the working tree.

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
| 7 | `ROADMAP.md` | `18b16f4420bc61f7d8c642b3473141314d5b3836` |
| 8 | `PROJECT_STATE.md` | `bd800cf0ce7a725b8d9fd9caa5034c6068ca1144` |
| 9 | `P0_BASELINE_MANIFEST.md` | *(self — see §1a; obtain via `git ls-tree -r <Final Candidate Git SHA>`)* |

Preserved source artifact and review records:

| # | Path | Blob SHA |
|---|---|---|
| 10 | `docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md` | `4557aea05a9b2f6b0e296417a445bcc5103bdede` |
| 11 | `reviews/README.md` | `47c250a83166f51144f7b73d57fea7ef5f91daee` |
| 12 | `reviews/QI-P0-R01_Independent_Critical_Review_Result.md` | `3a234f47b4d6701a0b39ec2bf0031a4ea0eb6a29` |

Bootstrap support files (not canonical P0 content):

| Path | Blob SHA |
|---|---|
| `README.md` | `78dc3e52c8fe60c11c6dde2381f10835a6203f2a` |
| `.gitignore` | `864cb918005990c43726e19db60c0857632e98d2` |

---

# 4. Review Status

```text
QI-P0-R01:
  FIX REQUIRED
  Reviewer:              GPT-6 Astra Pro (independent session)
  Reviewed artifact:     QI-P0-RC-0.2
  Reviewed artifact SHA-256:
    32833aa3a3f0ef18b3b2b6220aabb32a44f304cb76f0ffe5ff95339c39fbcdb3
  Blocking findings:     5  (P1-01 … P1-05)
  Non-blocking findings: 3  (P2-01 … P2-03)
  Full artifact:         PRESENT — reviews/QI-P0-R01_Independent_Critical_Review_Result.md
  Closure status:        DESIGN CLOSED / FINAL RE-REVIEW REQUIRED

Blocking Findings:
  DESIGN CLOSED / FINAL RE-REVIEW REQUIRED

Business Implementation Authorization:
  NONE

P0 Status:
  NOT ACCEPTED

Next Review:
  P0-06 GPT-6 Final P0 Re-Review, bound to Final Candidate Git SHA
```

> `P0_BASELINE_MANIFEST.md` was deliberately written to be **byte-stable under the
> finalization commit**: every hash in §3 was measured on the bootstrap content commit, and
> only this file's own row is self-referential. No canonical content changed between the
> bootstrap commit and the finalization commit.

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

---

# 7. Provenance

```text
Created by:  QI-P0-05-I01 Document-only Repository Bootstrap Candidate
Date:        2026-09-23
Scope:       split / organize / persist / verify / commit only
Business code created: NO
```
