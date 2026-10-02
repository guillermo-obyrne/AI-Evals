# Module 4 · Launch Strategy · Section 4.0 Release Criteria

> Repo file `ai-evals/04-eval-gates/lab-2-launch-strategy.md`. Your PRD's release-criteria section: the numeric thresholds, the CI gate policy, and the mitigation lever for the Soft gate.
>
> Fill this with the **Launch Strategy Builder** tool, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly.

## 4.0 Release Criteria

| Severity | Metric | Threshold | Dataset | Method |
|---|---|---|---|---|
| Hard | Stale, unsupported or contradicted claims rendered (pricing, competitor comparisons); correct answers wrongly blocked | = 0% rendered; false-block cap ≤ 5% of correct answers | `Ascend_IQ_Logs` | Deterministic lookup against the verified record, run on PR with replayed fixtures; Release kill switch as a second layer for competitor claims |
| Soft | Unverified-entity rate (e.g. speakers not on the confirmed list) | < 2% | `Ascend_IQ_Logs` | Entity check against the confirmed list, plus human review of a sample at Staging |
| Soft | Response latency, p95 | ≤ 4.0s (target 2.0s) | `Ascend_IQ_Logs` | Load test under realistic traffic at Staging |
| Advisory | Share of outputs containing flagged slang | ≤ 5% | `Ascend_IQ_Logs` | Keyword check at PR, non-blocking |

## 4.1 CI Gate Policy

Every pull request is replayed against a frozen regression golden set of at least 30 recorded cases. Replay is deterministic and makes no live model calls, so results are reproducible from run to run. Policy is set **per dimension**; no blended "quality" score is used.

**Blocking dimensions** fail the merge if the score falls below the floor or regresses past the maximum allowed against `main`:

- **Faithfulness (grounding):** floor 95, max regression 3
- **Task completion:** floor 90, max regression 5
- **Safety / policy:** floor 98, max regression 1

**Warn-only dimensions** surface in the check but do not block the merge:

- **Tool selection:** floor 80, max regression 5
- **Latency (p95):** floor 70, max regression 8
- **Cost per task:** floor 70, max regression 10

Latency is validated under realistic load at Staging, because CI replay cannot reproduce production timing.

## 4.2 Mitigation Plan · Soft Gate

**Selected Lever:** Staged Rollout

Release to 5% of traffic first, then expand to 25% and finally 100%, advancing only while p95 latency and the unverified-entity rate stay within the Soft-gate thresholds. Controlling the audience this way keeps a Soft-gate miss small and reversible, and the real cases gathered at each stage feed the golden set so coverage grows as exposure does.
