# Quant Intelligence — AUTONOMOUS DEVELOPMENT GOVERNANCE

```text
Document ID:        QI-P0-DOC-GOVERNANCE
Status:             REVIEW CANDIDATE
Phase:              P0 — Project Bootstrap
P0 Status:          NOT ACCEPTED
Business Implementation Authorization: NONE
Source of truth:    docs/p0/Quant_Intelligence_P0_Review_Candidate_v0.3.md (§10, §11)
Clause range:       G-01 … G-10, FZ-01 … FZ-05
```

> Faithful split of `QI-P0-RC-0.3`. Governance semantics are preserved exactly and
> are **not** simplified or relaxed.
>
> Two invariants that must never be dropped:
>
> ```text
> Worker PASS != Acceptance
> max automatic Coding attempts per Problem Family = 5
> ```

---

# Part A — Autonomous Development Governance

## G-01｜正常流程

`Design → Freeze → DeepSeek Implementation → Independent Review → AI Acceptance → Closeout → Next Task`

普通 Task 不需要 Human 逐项确认。

## G-02｜Worker 无设计权

DeepSeek 只 implement、test、verify、self review、commit、evidence、completion report。

不得改 Requirement、Architecture、Scope、AC、Constitution、Governance，也不得自验 ACCEPTED。

## G-03｜Acceptance

**Worker PASS ≠ Acceptance。**

AI Acceptance 必须绑定 Project、Task ID、Frozen Requirement digest、Task Base、exact Candidate SHA、Independent Review PASS、delegated authority。

不得伪造 Human Acceptance。

## G-04｜Normal Fix Budget

同一 Problem Family 的 Normal Cycle：

1. Initial Implementation
2. Review
3. FIX-01
4. Review
5. FIX-02
6. Review

第三次 `FIX REQUIRED` 后停止 Normal Fix，进入 `ROOT_CAUSE_REVIEW`。

## G-05｜RCA 不自动授予新实施权限

RCA 必须产出：

- problem_family_id；
- accumulated_attempt_count；
- root-cause category；
- evidence；
- design delta；
- why previous approach failed；
- whether Human-owned boundary is touched；
- Recovery Batch authorization decision。

RCA：

- 不重置失败计数；
- 不自动开始 Coding；
- 不把同一问题改名为新 Problem Family；
- 不自动产生 FIX-03。

## G-06｜唯一一次 Post-RCA Recovery Batch

同一 Problem Family 默认最多允许：

Normal Cycle：

- Initial Implementation
- FIX-01
- FIX-02

之后 RCA-01。

若 RCA 确认存在可验证、实质性的新解法，且不改变 Human-owned boundary，Design Authority 可授权：

Post-RCA Recovery Batch：

- RECOVERY-01 Implementation
- 最多一个 RECOVERY-FIX-01

这是同一 Problem Family 的最后一个自动实施批次。

**同一 Problem Family 的自动 Coding attempt 硬上限为 5 次。**

完整状态迁移序列：

```text
Initial → FIX-01 → FIX-02 → RCA-01 → RECOVERY-01 → RECOVERY-FIX-01
```

## G-07｜预算耗尽后的终态

若 Post-RCA Recovery Batch 仍不能 PASS：

`BLOCKED + ESCALATION REQUIRED`

同一 Problem Family 不得再自动开始任何 Implementation/Fix。

允许 AI 继续调查、写 RCA、提供设计选项、处理其它不相关任务，但不能继续消耗同一问题族 Coding attempt。

这不会自动变成 `NEEDS_HUMAN_DECISION`。

只有真正要求改变 C-10 Human-owned boundary 时才询问 Human。

## G-08｜外部条件变化

若问题是 executor/tool unavailable、network、dependency outage、environment，BLOCKED 后可等待客观外部条件变化。

“条件恢复”本身不重置 Implementation budget。

若要重新授权同一 Problem Family 的 Coding 超过 G-06 上限，属于 Autonomous Governance exception，必须按 C-10 进入 Human-owned Governance Change；AI 不得自行批准例外。

## G-09｜Independent Review

Reviewer READ_ONLY，不改 source/config/DB/Git refs/Requirement，从 Repository Truth 自行检查。Worker Report 只作导航。证据不足 => BLOCKED。

Verdict 仅 PASS / FIX REQUIRED / BLOCKED / ESCALATION REQUIRED。

## G-10｜Task Identity

Formal Task 必须绑定 Project、Repository、Task ID、Requirement version/digest、Accepted Baseline、Task Base、Candidate SHA、execution config、acceptance criteria。

错窗口/错 Task/错 SHA 的反馈不得吸收。

---

# Part B — Review-to-Freeze Identity Contract

v0.3 修改了实质设计，因此：

**`QI-P0-R01` 对 v0.2 的 `FIX REQUIRED` 不能被复用为 v0.3 PASS。**

## FZ-01｜Review Applicability Tuple

每个 Review verdict 必须绑定：

- review_task_id；
- project；
- artifact/repository path set；
- artifact version；
- exact SHA-256（未建 repo 的 artifact）或 **Candidate Git SHA**（建 repo 后）；
- source baseline refs；
- reviewed requirement/governance versions；
- review timestamp/source。

Verdict 只对该 tuple 生效。

## FZ-02｜语义变化使旧 PASS 失效

Review 后若发生 Constitution semantics、Governance semantics、Architecture boundary、Derivation classification、Evaluation semantics、Acceptance Boundary 的变化，旧 PASS 自动失效，必须重新 Review。

## FZ-03｜最终 P0 Review Target 必须是 Repository Candidate SHA

为消除“审本地文件 A → 入库时改成 B → 仍引用 A 的 PASS”风险，P0 流程调整为：

1. v0.3 Findings Closure；
2. 建立 **document-only Quant Intelligence repository candidate**；
3. 将最终候选拆入 canonical project files；
4. 计算实际 Candidate Git SHA；
5. 从 Candidate SHA 独立读取全部 P0 canonical files；
6. 执行 **GPT-6 Final P0 Re-Review**；
7. PASS 只绑定该 Candidate SHA；
8. 未经审查的语义变化不得进入 freeze；
9. PASS 后才形成 P0 Accepted Baseline。

因此，本地 v0.3 不是最终 P0 Review authority，它是 repo bootstrap 的设计输入。

## FZ-04｜PASS 后只允许无语义收口

如果 GPT-6 对 Candidate SHA PASS 后还需要状态标记、accepted pointer、registry pointer，必须进行 diff classification。

若修改 P0 canonical content bytes：

- 语义变化 → 旧 PASS 失效；
- 纯机械 metadata/pointer 变化 → 必须记录 exact diff 和 applicability judgement。

优先做法：先把需要进入 Accepted Baseline 的内容全部写入 Candidate，再 Review，PASS 后不改被审正文。

## FZ-05｜Baseline Manifest

最终仓库必须保存 P0 baseline mapping，至少列：

- Candidate/Accepted Git SHA；
- canonical file paths；
- file blob SHA 或 content digest；
- GPT-6 review task/result pointer；
- source MarketPulse SHA；
- Global Protocol version；
- date；
- status。

不得使用 MarketPulse SHA 冒充 QI baseline。

---

## Binding requirement for the next review

```text
GPT-6 Final P0 Re-Review MUST bind the exact Candidate Git SHA.
```

The Candidate Git SHA is recorded in `P0_BASELINE_MANIFEST.md`.
