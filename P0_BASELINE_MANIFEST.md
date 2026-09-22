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

Base SHA (candidate branch point / main initialisation):
  3b37624f7ca6b99cf30506be1da9605b93a1a4f8

Bootstrap Content Commit SHA:
  PENDING_COMMIT

Manifest Finalization Commit SHA:
  PENDING_COMMIT

Final Candidate Git SHA:
  PENDING_COMMIT
```

> `Final Candidate Git SHA` is the **exact** revision that an independent GPT-6 Final P0
> Re-Review must bind. It is the last commit on the candidate branch.
> The bootstrap is intentionally performed as two commits: the first carries the P0 content,
> the second only replaces the `PENDING_COMMIT` placeholders above with measured values.
> No canonical P0 content changes between the two commits.

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
renormalised or regenerated. Its SHA-256 is re-verified after storage.

---

# 3. Canonical File Set

Canonical P0 documents (the review target set):

| # | Path | Blob SHA |
|---|---|---|
| 1 | `AI_BOOTSTRAP.md` | PENDING_COMMIT |
| 2 | `PRODUCT_CONSTITUTION.md` | PENDING_COMMIT |
| 3 | `AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md` | PENDING_COMMIT |
| 4 | `ARCHITECTURE.md` | PENDING_COMMIT |
| 5 | `DERIVATION_MANIFEST.md` | PENDING_COMMIT |
| 6 | `EVALUATION_CONTRACT.md` | PENDING_COMMIT |
| 7 | `ROADMAP.md` | PENDING_COMMIT |
| 8 | `PROJECT_STATE.md` | PENDING_COMMIT |
| 9 | `P0_BASELINE_MANIFEST.md` | PENDING_COMMIT |

Preserved source artifact and review records:

| # | Path | Blob SHA |
|---|---|---|
| 10 | `docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md` | PENDING_COMMIT |
| 11 | `reviews/README.md` | PENDING_COMMIT |
| 12 | `reviews/QI-P0-R01_Independent_Critical_Review_Result.md` | PENDING_COMMIT |

Bootstrap support files (not canonical P0 content):

```text
README.md
.gitignore
```

---

# 4. Review Status

```text
QI-P0-R01:
  FIX REQUIRED
  Reviewer:  GPT-6 Astra Pro (independent session)
  Reviewed artifact: QI-P0-RC-0.2 @ SHA-256 32833aa3a3f0ef18b3b2b6220aabb32a44f304cb76f0ffe5ff95339c39fbcdb3
  Blocking findings:   5  (P1-01 … P1-05)
  Non-blocking findings: 3 (P2-01 … P2-03)
  Closure status: DESIGN CLOSED / FINAL RE-REVIEW REQUIRED

Blocking Findings:
  DESIGN CLOSED / FINAL RE-REVIEW REQUIRED

Business Implementation Authorization:
  NONE

P0 Status:
  NOT ACCEPTED

Next Review:
  P0-06 GPT-6 Final P0 Re-Review, bound to Final Candidate Git SHA
```

---

# 5. Known Source Limitations Carried Into This Baseline

```text
FAILED_BREAKOUT                          = NOT DIRECTLY PORTABLE
old relative_volume / VOLUME_ANOMALY     = NOT DIRECTLY PORTABLE
```

Full statements in `DERIVATION_MANIFEST.md` (KSL-01, KSL-02).

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
```
