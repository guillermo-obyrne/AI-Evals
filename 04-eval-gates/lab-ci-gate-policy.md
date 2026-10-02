# Lab, CI Eval Gate Policy (Ascend IQ PR #218)

> Repo file `ai-evals/04-eval-gates/lab-ci-gate-policy.md`. Output of the **CI Gate demo** (instructor-led): PR #218 swaps the Ascend IQ retrieval prompt; a 30-case regression golden set is replayed on every PR. You set floors + max-regression per dimension, decide blocking vs warn-only, then make the merge call.
>
> Fill this with the **CI Gate Demo** tool, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly.

| Dimension | main | PR | Δ | Floor | Max reg | Blocking | Result |
|---|---:|---:|---:|---:|---:|---|---|
| Faithfulness (grounding) | 96 | 87 | −9 | 95 | 3 | yes | FAIL |
| Task completion | 92 | 93 | +1 | 90 | 5 | yes | PASS |
| Tool selection | 90 | 88 | −2 | 80 | 5 | no | PASS |
| Safety / policy | 99 | 99 | 0 | 98 | 1 | yes | PASS |
| Latency (p95) | 84 | 80 | −4 | 70 | 8 | no | PASS |
| Cost per task | 88 | 82 | −6 | 70 | 10 | no | PASS |

**Gate result:** BLOCKED

## Merge decision

BLOCK merge. Faithfulness (grounding) fell from 96 to 87, a 9-point regression that is 3× the 3-point maximum and lands 8 points below the 95 floor, so the PR violates both limits on a blocking P0 dimension. Task completion and safety are within policy; tool selection, latency and cost are warn-only and do not block. The developer should review the failing golden-set cases (likely ungrounded or invented claims), fix the retrieval prompt, and re-run the same 30-case replay. The policy stays unchanged.
