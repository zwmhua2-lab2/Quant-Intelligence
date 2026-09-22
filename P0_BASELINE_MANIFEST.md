# Quant Intelligence — P0 BASELINE MANIFEST

```text
Document ID:        QI-P0-DOC-BASELINE-MANIFEST
Status:             ACCEPTED
Phase:              P0 — Accepted Baseline
P0 Status:          ACCEPTED
Business Implementation Authorization: NONE
```

> **Status is `ACCEPTED`.** The accepted P0 baseline is `main` + annotated tag `p0-v1.0.0`.
> The acceptance authority is `DELEGATED_AI` — this is **not** a Human P0 acceptance.
> Human confirmation applies only to the Global Project Registry registration.
> Acceptance does **not** authorize business implementation and does **not** start P1.

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

P0-07 Closeout Candidate SHA (frozen — AI acceptance target, QI-P0-07-I01):
  fabe3616bec75b2212b51d15ea37485377de5145

Accepted Baseline Tag:
  p0-v1.0.0   (annotated)

Accepted Baseline SHA:
  = commit referenced by refs/tags/p0-v1.0.0 (see §1a)
```

## 1a. Self-reference note (read this before binding a review)

A Git object cannot contain its own hash. Therefore:

- `Accepted Baseline SHA` is **the commit referenced by the tag `p0-v1.0.0`**, i.e. the final
  freeze commit that introduces this manifest text. It cannot be written literally inside
  itself.
- Resolution procedure (deterministic, no chat context needed):

```bash
git rev-parse p0-v1.0.0^{commit}
git ls-remote https://github.com/zwmhua2-lab2/Quant-Intelligence.git refs/tags/p0-v1.0.0
```

- The literal value obtained from that command is published in the `QI-P0-07-I02` completion
  report and recorded in `docs/p0/QI-P0-07-I02_Final_Freeze_Record.md`. It is **not** duplicated
  into this file, for the reason above.
- The **reviewed** candidate is a past, immutable commit and is therefore recorded literally:
  `f7ea873b9b61527af481646fb88f63f6a52f9a52`. The `QI-P0-06-R01` `PASS` binds that SHA and
  nothing later.
- The **AI-accepted closeout candidate** is likewise frozen and recorded literally:
  `fabe3616bec75b2212b51d15ea37485377de5145` (`QI-P0-07-I01`; Independent Closeout Verification
  `PASS`, AI Acceptance `ACCEPTED`).
- **`39f4b2dba440470717b04fd15e8a114fb2ee939f` must no longer be used as the `P0-06` review
  target.** P0 canonical content changed semantically in `QI-P0-05-FIX-01` (P1–P4 roadmap
  restoration), so the previous candidate is superseded.
- The commit history is exactly six commits:

```text
<Base SHA>                 main initialisation  (3b37624…)
  └─ <Bootstrap Commit>    bootstrap Quant Intelligence P0 review candidate  (9c095bd…)
       └─ <Prev Candidate> finalize P0 candidate manifest  (39f4b2d… — SUPERSEDED)
            └─ <Reviewed Candidate> fix: restore P1–P4 roadmap baseline  (f7ea873… — REVIEWED, PASS)
                 └─ <Closeout Candidate> prepare P0 acceptance closeout candidate  (fabe3616… — AI ACCEPTED)
                      └─ <Accepted Baseline> accept Quant Intelligence P0 baseline  (← refs/tags/p0-v1.0.0)
```

- `main` was **fast-forwarded** from `3b37624…` to `fabe3616…` by `QI-P0-07-I02`, then advanced by
  the final freeze commit. No merge commit, no squash, no rebase, no force push.
- The historical branch `p0/bootstrap-candidate` remains at `fabe3616…`, retained for provenance.

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

Canonical P0 documents (the accepted baseline set):

Blob identities are Git blob SHAs. They are comparable across commits and independently
reproducible via `git ls-tree -r <commit> -- <path>`.

| # | Path | Blob at `fabe3616…` (closeout candidate) | Blob at `p0-v1.0.0` (accepted baseline) |
|---|---|---|---|
| 1 | `AI_BOOTSTRAP.md` | `1c02090f26a3df3a11ea870b08e537c87c62ed6f` | `630432fbd49e3d160388376339f20f03fb89b641` |
| 2 | `PRODUCT_CONSTITUTION.md` | `ab66232722d79c6c4f57089e4e8aeffb6e167550` | `14be44bb39aba6ead1f482c2bb6cd9dc75131c8a` |
| 3 | `AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md` | `3077c167262d50e97b3df552f434151f4c5719d6` | `89ee2ccc239b15aafdf1daa9dc340f01fd6f782d` |
| 4 | `ARCHITECTURE.md` | `2ad1cbf4b044c48dfb48ecc7acd6755b3c6115f4` | `9e23964528e54c171960f921a8c91224ade2e319` |
| 5 | `DERIVATION_MANIFEST.md` | `c8a3dbdd8eada38d9e8d0f7cb4d5c0949e81531f` | `011905580ad818000f27b302d38291366165a316` |
| 6 | `EVALUATION_CONTRACT.md` | `f19aec35a0039367c01956c60100ba32e80b15e2` | `18aad0a857ec0a015c5b1b7aa57628956ba055e8` |
| 7 | `ROADMAP.md` | `6e25e4c8bcc70d07885b6960bccd8a4784b7049f` | `81b98d18264b32e4cf2a2ee63d244e808654e746` |
| 8 | `PROJECT_STATE.md` | `6630b8e7683a3c4b4b500adfe6b28f0b8a4dffbb` | `d87e63cc3d0812d121b66cdc3b58f169064e98fc` |
| 9 | `P0_BASELINE_MANIFEST.md` | `7a51748f7fc01cb89639f606dacebc664425e613` | *(self — see §1a)* |

Rows 1, 7 and 8 carry **status-only** changes in this freeze (metadata header plus phase /
review-status statements). Rows 2, 3, 4, 5 and 6 carry **metadata-only** changes: exactly three
lines each, all inside the metadata header fenced block. Their **semantic bodies below that
boundary are byte-equivalent** to `fabe3616…` — see §3a for the proof. The `QI-P0-06-R01` `PASS`
therefore remains valid for the five semantic invariants.

Preserved source artifacts and review records:

| # | Path | Blob at `fabe3616…` | Blob at `p0-v1.0.0` |
|---|---|---|---|
| 10 | `docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md` | `4557aea05a9b2f6b0e296417a445bcc5103bdede` | `4557aea05a9b2f6b0e296417a445bcc5103bdede` |
| 11 | `docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.2.md` | `7224569c8a8b65570a2d6146f27ad16c4be50fad` | `7224569c8a8b65570a2d6146f27ad16c4be50fad` |
| 12 | `docs/p0/QI-P0-05-FIX-01_Roadmap_Restoration_Amendment.md` | `6ff13f3398e12e76f922270a6f839dcc98c1e6a3` | `6ff13f3398e12e76f922270a6f839dcc98c1e6a3` |
| 13 | `reviews/README.md` | `d8740216ad4e8334b15e972f47aad84d53899da4` | `3947fbce2d00abcdb0ed2635b6169a8c224cf73a` |
| 14 | `reviews/QI-P0-R01_Independent_Critical_Review_Result.md` | `3a234f47b4d6701a0b39ec2bf0031a4ea0eb6a29` | `3a234f47b4d6701a0b39ec2bf0031a4ea0eb6a29` |

Rows 10–12 and 14 are byte-identical to the closeout candidate. Row 13 (`reviews/README.md`) is
a living registry: status metadata plus the final acceptance provenance required by §7 of that
file were added; the historical `QI-P0-R01 = FIX REQUIRED` records were **not** erased.

Closeout artifacts added by `QI-P0-07-I01`:

| # | Path | Blob at `fabe3616…` | Blob at `p0-v1.0.0` |
|---|---|---|---|
| 15 | `reviews/QI-P0-06-R01_Final_P0_ReReview_Result.md` | `83689528a73bef3a8130b0b92a4e47d0f8e1026e` | `83689528a73bef3a8130b0b92a4e47d0f8e1026e` |
| 16 | `docs/p0/QI-P0-07-I01_Closeout_Diff_Classification.md` | `82ad0147990bf23869e4f2c06abf940d1d86e0dc` | `82ad0147990bf23869e4f2c06abf940d1d86e0dc` |

Final freeze artifacts added by `QI-P0-07-I02`:

| # | Path | Blob at `fabe3616…` | Blob at `p0-v1.0.0` |
|---|---|---|---|
| 17 | `docs/p0/QI-P0-07-I02_Final_Freeze_Record.md` | *(absent)* | `eac1a263545c196a5f4f641bf0bbce2a67fc71b8` |
| 18 | `docs/p0/QI-P0-07-I02_Final_Freeze_Diff_Classification.md` | *(absent)* | *(self — obtain via `git ls-tree -r p0-v1.0.0^{commit}`)* |

Bootstrap support files (not canonical P0 content):

| Path | Blob at `fabe3616…` | Blob at `p0-v1.0.0` |
|---|---|---|
| `README.md` | `78dc3e52c8fe60c11c6dde2381f10835a6203f2a` | `491fa527b8a95dc65c248058a85910877f409ae5` |
| `.gitignore` | `864cb918005990c43726e19db60c0857632e98d2` | `864cb918005990c43726e19db60c0857632e98d2` |

## 3a. Semantic body invariant — five canonical documents

A Git object cannot contain its own hash, but a **body** can be compared byte-for-byte. For the
five semantic canonical documents the metadata header is the first fenced `text` block
(lines 3 … *closing fence*). Everything **below** that boundary must remain byte-equivalent to
the accepted closeout candidate `fabe3616…`. Verified by extraction and SHA-256 comparison:

| Path | Metadata block (1-based lines) | Semantic body SHA-256 at `fabe3616…` | Semantic body SHA-256 at `p0-v1.0.0` | Result |
|---|---|---|---|---|
| `PRODUCT_CONSTITUTION.md` | 3 … 11 | `3dd409fd1f2cae13fdec0f19a094ade4cb3b39e9a76cb91655a4d8ba0a332a09` | `3dd409fd1f2cae13fdec0f19a094ade4cb3b39e9a76cb91655a4d8ba0a332a09` | BYTE-EQUIVALENT |
| `AUTONOMOUS_DEVELOPMENT_GOVERNANCE.md` | 3 … 11 | `14ca57a6ffa9bc9aca078d37a5012116e180f6d0cf82e405b73cc211115c6462` | `14ca57a6ffa9bc9aca078d37a5012116e180f6d0cf82e405b73cc211115c6462` | BYTE-EQUIVALENT |
| `ARCHITECTURE.md` | 3 … 11 | `831189115b48bee9e77f2a0f7c8e501401661fcd31f0641dc64437e13ce66d4b` | `831189115b48bee9e77f2a0f7c8e501401661fcd31f0641dc64437e13ce66d4b` | BYTE-EQUIVALENT |
| `DERIVATION_MANIFEST.md` | 3 … 14 | `9a99d2ef307cd5d63fd42057970c8a47397b99c6dc5087f97c84bc9ffbda931c` | `9a99d2ef307cd5d63fd42057970c8a47397b99c6dc5087f97c84bc9ffbda931c` | BYTE-EQUIVALENT |
| `EVALUATION_CONTRACT.md` | 3 … 11 | `8b4126901d0f65e0ef58905720c77f2cbaf7e4b608a245abfa75336e32197108` | `8b4126901d0f65e0ef58905720c77f2cbaf7e4b608a245abfa75336e32197108` | BYTE-EQUIVALENT |

Independently, `git diff --numstat fabe3616… p0-v1.0.0 -- <path>` reports exactly `3 3` changed
lines for each of the five documents, all inside the metadata block. No other hunk exists.
Therefore `FINAL_FREEZE_SEMANTIC_DRIFT` is **not** triggered.

Full values and reproduction commands:
`docs/p0/QI-P0-07-I02_Final_Freeze_Diff_Classification.md`.

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
  ACCEPTED
  Acceptance Authority:  DELEGATED_AI   (NOT a Human P0 acceptance)

P1 / P2 / P3 / P4 Status:
  NOT IMPLEMENTATION AUTHORIZED

P0 Acceptance Provenance:
  QI-P0-06-R01 GPT-6 Final P0 Re-Review:           PASS
    reviewed candidate SHA:
      f7ea873b9b61527af481646fb88f63f6a52f9a52
    blocking findings: 0
  QI-P0-07-I01 Independent Closeout Verification:  PASS
  QI-P0-07-I01 AI Acceptance:                      ACCEPTED
    closeout candidate SHA:
      fabe3616bec75b2212b51d15ea37485377de5145
  Human Registry Confirmation:                     CONFIRMED
    (Human confirmation applies only to Global Project Registry registration)

Global Project Registry:
  REGISTERED
  repository:  zwmhua2-lab2/ai-dev-protocol
  file:        PROJECT_REGISTRY.md
  commit:      08031c45dbd735ebad911e96d17a4a042788fa95
  protocol:    protocol-v1.4.5 @ 1d17ba0e986c128715cbec7763f734696fb0956d (unchanged)

Accepted Baseline Tag:
  p0-v1.0.0   (annotated)

Accepted Baseline SHA:
  = commit referenced by refs/tags/p0-v1.0.0 (see §1a)

Next Review:
  none. The accepted baseline is frozen and no further P0 review is scheduled.
  The next design artifact is the P1 Requirement / Architecture Design — that is NOT a
  P0 review, and it does NOT authorize P1 implementation.
```

> `QI-P0-R01` is a verdict on `QI-P0-RC-0.2` and is unaffected by later revisions.
> `QI-P0-06-R01` is a verdict on `f7ea873b…` only; it is **not** a verdict on the
> `P0-07` closeout candidate. P0 acceptance rests on the separate `QI-P0-07-I01`
> Independent Closeout Verification plus delegated AI Acceptance.

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
Amended by:   QI-P0-07-I02 Final Freeze / Registry / Accepted Baseline
Date:         2026-09-23
Scope:        split / organize / persist / verify / commit only
Business code created: NO
```
