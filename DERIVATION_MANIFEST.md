# Quant Intelligence — DERIVATION MANIFEST

```text
Document ID:        QI-P0-DOC-DERIVATION-MANIFEST
Status:             REVIEW CANDIDATE
Phase:              P0 — Project Bootstrap
P0 Status:          NOT ACCEPTED
Business Implementation Authorization: NONE
Source of truth:    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md (§5, §7, §12)
Clause range:       M-01 … M-06, KSL-01 … KSL-03, MAP-01 … MAP-03

MarketPulse Derivation Source:
  zwmhua2-lab2/MarketPulse-AI @ 7a023f99ac44ac744e5a9885fd82f8a87ef51a94
```

> Faithful split of `QI-P0-RC-0.3`. Derivation decisions and **known source limitations are
> preserved in full**; they must not be reduced for convenience.
>
> `KEEP / ADAPT / EXCLUDE` describes **migration design intent**. It does **not** mean the
> target code exists. No MarketPulse source file is imported into this repository.

---

## Scope boundary

`zwmhua2-lab2/MarketPulse-AI @ 7a023f99ac44ac744e5a9885fd82f8a87ef51a94` is a **pinned
derivation source only**.

Explicitly not permitted:

- modify MarketPulse
- push to MarketPulse
- cherry-pick back to MarketPulse
- treat MarketPulse as the new project repository
- import MarketPulse `.git` history or merge MarketPulse history

All source paths below are relative to `backend/src/marketpulse/` in the pinned source.

---

# Part A — KEEP / ADAPT / EXCLUDE

## M-01｜低耦合候选

| Source | Decision | QI Rule |
|---|---|---|
| `clock.py` | KEEP | 独立迁移 Clock/SystemClock/ReplayClock 语义 |
| `ids.py` | KEEP-SUBSET | 只保留实际需要的 UUID/UTC helpers |
| `market_data/base.py` | ADAPT | 新 feed contract |
| `market_data/models.py` | ADAPT | 重新定义 availability / market status 语义 |
| `market_data/normalizer.py` | ADAPT | 注入 Clock/received_at，不直接 `datetime.now()` 形成不可控时间 |
| `market_data/okx_live.py` | ADAPT | 只使用公开行情路径 |
| `market_data/universe_provider.py` | ADAPT | 新 Universe policy |
| `universe/*` | ADAPT | 不继承交易准入含义 |

## M-02｜Feature / Scanner

| Source | Decision | QI Rule |
|---|---|---|
| `g2/rolling.py` | **ADAPT-CONCEPT / NO DIRECT PORT** | Feature formulas 必须重新冻结；旧值不视为已验证 intelligence semantics |
| `g2/structure.py` | ADAPT-CONCEPT | 可参考 confirmed-bar 结构思想；不把当前算法视为完成的市场结构模型 |
| `g2/scanner.py` | ADAPT-CONCEPT | Detector taxonomy 可参考；新事件必须使用 QI Feature contract |
| `g2/models.py` | ADAPT-SUBSET ONLY | 禁止整文件复制 |
| `g2/runtime.py` | **EXCLUDE AS SOURCE FILE** | 只参考 Rolling→Scanner 调用顺序；不迁移 state/strategy/advisory/forward graph |

## M-03｜已知来源算法限制

### KSL-01｜旧 `FAILED_BREAKOUT` 不可直接复用

固定来源默认结构下：

`range_upper = H`

而：

`retest_area.upper = H + positive_width`

旧 Scanner 要求 FAILED_BREAKOUT：

`current_price < range_upper`

且

`current_price > retest_area.upper`

在正常正价格宽度下不能同时成立。

因此：

- `FAILED_BREAKOUT` **不进入任何默认迁移白名单**；
- 不得通过原算法“照搬 + 改 package 名”迁移；
- 若未来 QI 需要失败突破事件，必须作为独立 Formal Requirement 重新定义 setup、breakout confirmation、failed condition、time window、zone semantics、deterministic fixtures、acceptance examples。

> **Status: NOT DIRECTLY PORTABLE.**

### KSL-02｜旧 `relative_volume` 不可作为窗口相对成交量复用

旧 rolling：

`volume = sum(all current rolling trade quantities)`

但 baseline：

`mean(trade.quantity over trades[:-10])`

因此分子是“窗口总量”，分母是“单笔数量均值”。

该值不能直接解释成“当前窗口成交量 / 历史同长度窗口成交量”。

因此：

- 旧 `relative_volume` formula **不得进入 QI**；
- 依赖它的旧 `VOLUME_ANOMALY` detector 不进入默认迁移白名单；
- 若 QI 需要 Volume Anomaly，P1/P3 Requirement 必须重新冻结时间窗口、baseline windows、minimum history、late data policy、unit、threshold semantics、deterministic examples。

> **Status: NOT DIRECTLY PORTABLE.**

### KSL-03｜其它旧 Feature 也不是“自动验证通过”

没有被本次审查指出具体反例，不等于 price acceleration、volatility、flow imbalance、liquidity change、OI change、relative BTC strength 已经自动获得 QI 产品语义。

P1 选用任何 Detector 前必须在 Formal Requirement 中明确：

- feature definition；
- unit；
- window；
- availability semantics；
- threshold；
- known limitations；
- acceptance fixtures。

## M-04｜Edge

| Source | Decision | QI Rule |
|---|---|---|
| `edge/identity.py` | ADAPT | 新 QI domain tags |
| `edge/hashing.py` | ADAPT-SUBSET | canonical hash + non-finite rejection 一起迁移 |
| `g5/canonical.py::canonicalize_value` | ADAPT-SUBSET | 不迁移 G5 workspace replay |
| `edge/capture.py` | ADAPT-CONCEPT | as-of / immutable / lookahead prevention |
| `edge/models.py` | ADAPT-SUBSET | 删除 FORWARD_PAPER / trading vocabulary |
| `edge/forward.py` | ADAPT-CONCEPT | 固定 horizon/因果规则；删除 paper cost |
| `edge/forward_service.py` | ADAPT-CONCEPT | due scheduling / retry 思想 |
| `edge/aggregation.py` | ADAPT-SUBSET | coverage / uncertainty；删除 trading expectancy |
| `edge/attribution.py` | **EXCLUDE** | 绑定 Trade/Risk/Execution/Economic lineage |
| `edge/repository.py` | **EXCLUDE AS SOURCE FILE** | 新项目独立 Schema |

## M-05｜Advisory

| Source | Decision | QI Rule |
|---|---|---|
| `advisory/identity.py` | ADAPT-SUBSET | invocation/attempt identity |
| `advisory/contracts.py` | ADAPT-CONCEPT | immutable provider/model/prompt/schema/deadline |
| `advisory/attempts.py` | **EXCLUDE AS SOURCE FILE** | 目标 lifecycle 重建 |
| `advisory/predicates.py` | **EXCLUDE AS SOURCE FILE** | 只参考 fail-closed |
| `advisory/worker.py` | **EXCLUDE AS SOURCE FILE** | 目标 worker 重建 |

## M-06｜Runtime

`runtime_host.py`：**EXCLUDE AS SOURCE FILE**

`runtime.py`：**ADAPT-SUBSET ONLY**

仅参考 bounded worker、lifecycle、supervised execution、health snapshot。

不得复制 trading risk readiness、safety queue、economic recovery、默认健康语义。

### Explicit exclusion summary

```text
runtime_host.py          = EXCLUDE AS SOURCE FILE
edge/attribution.py      = EXCLUDE
g3/*                     = EXCLUDE
g4/*                     = EXCLUDE
```

These exclusions may not be reduced for convenience.

---

# Part B — Canonical / Hashing Migration Contract

> 吸收非阻断 Finding `P2-02`。

QI 迁移 canonical hashing 时，最小完整单元不是单独 `canonicalize_value`，而是：

1. type-tagged canonicalization；
2. recursive non-finite rejection；
3. deterministic JSON byte encoding；
4. domain separation；
5. SHA-256 digest；
6. QI-specific domain tags；
7. deterministic tests。

Acceptance 必须包含嵌套 float NaN / ±Infinity、Decimal non-finite、list/tuple、Mapping、datetime timezone、enum、key ordering。

禁止只复制 `g5.canonical.canonicalize_value` 后声称已继承旧 hashing safety。

---

# Part C — Migration Acceptance Preconditions

以下三项来自 `QI-P0-R01` 的非阻断 Findings，但现在升级为相关 Formal Task 的强制 AC 前置。

## MAP-01｜Score Semantics

Scanner / Decision Task 必须证明：

- strength != priority；
- priority != quality；
- quality != probability；
- confidence probability 有定义才能出现。

（语义定义见 `EVALUATION_CONTRACT.md` / Score Semantics Contract。）

## MAP-02｜Canonical Safety

Canonical/Hashing Task 必须测试：

- nested non-finite rejection；
- deterministic bytes；
- domain separation；
- timezone-aware datetime；
- unsupported type fail closed。

（迁移单元定义见本文件 Part B。）

## MAP-03｜Runtime Health

Runtime Task 必须测试：

- required component missing => NOT READY；
- unknown health => NOT READY；
- worker unexpected normal exit => UNAVAILABLE/FAILED；
- stale heartbeat => NOT READY；
- no exception != healthy。

（不变量定义见 `ARCHITECTURE.md` / Runtime Health Contract H-01 … H-03。）

---

## Cross-references

```text
M-02 rolling.py / g2/runtime.py   → closes QI-P0-R01 P1-01
KSL-01 FAILED_BREAKOUT            → NOT DIRECTLY PORTABLE
KSL-02 relative_volume/VOLUME_ANOMALY → NOT DIRECTLY PORTABLE
Part B canonical contract         → closes QI-P0-R01 P2-02
Part C MAP-01 … MAP-03            → mandatory AC preconditions for later Formal Tasks
```

## Residual uncertainty (inherited from source review, NOT resolved here)

`QI-P0-R01` §5 area 5 and §7 record that a full transitive dependency / package-init /
build-dependency proof for the pinned source was **not** completed. Migration must still
perform its own preflight. This precondition is not discharged by this document.
