# Lab, Trajectory Eval (Ascend IQ usage-drop task)

> Repo file `ai-evals/03-eval-suites/lab-1b-trajectory.md`. Grade the agent's **path**, not just the final answer.
>
> Trace graded: `T-01-A` from `trajectory-traces.csv` (redundant call, off-scope step, missing diagnostic steps).

**Matching mode:** Unordered. All five golden steps are required, `get_ingestion_status` and `compare_weeks` are interchangeable in order, and `draft_reply` must come last.
**Score:** 1.5/6 (PASS = 1, PARTIAL = 0.5) · **Verdict:** HOLD

## Recorded path

| # | Tool | What happened |
|---|---|---|
| 1 | `get_account` | Enterprise plan confirmed |
| 2 | `get_usage` (weeks=4) | Returned 4 weeks of weekly-active-user counts |
| 3 | `get_usage` (weeks=4) | Identical call repeated, same result |
| 4 | `search_web` | Not requested by the task; pulls an external opinion instead of account data |
| 5 | none | `get_ingestion_status` and `compare_weeks` never called |
| 6 | `draft_reply` | "Your 30% dip looks like a seasonal trend…" with no verification |

**Golden path:** `get_account → get_usage → get_ingestion_status → compare_weeks → draft_reply` (real cause: a 3-day ingestion gap, Jul 8–10).

## Dimension scores

| Dimension | Score | Note |
|---|---|---|
| Tool selection | FAIL | Never called `get_ingestion_status` or `compare_weeks`, the two diagnostic tools. Step 4 called `search_web`, which is out of scope. |
| Argument correctness | PASS | `get_account` and `get_usage` used the right `account_id` and `weeks=4`. There are no arguments to grade for the tools that were never called. |
| No redundant / looping steps | PARTIAL | Step 3 repeats the step 2 `get_usage` call identically. It is not a loop and returned no new data point, so it is less severe. |
| Recovery | FAIL | Step 3 added nothing. The agent moved to an external search instead of the missing diagnostic tools. |
| Plan coherence | FAIL | It skipped the ingestion check and the week-over-week compare (step 5) and went from a repeated call to an off-scope search to a reply. |
| Task completion | FAIL | The cause was never verified, and step 6 blames seasonality when the real cause is a 3-day ingestion gap. |

## Verdict

**HOLD.** The first two steps were sound, but the path never verified the cause. The reply blames seasonality with no evidence, which is the same unverified-claim failure as the M2 P0. A plausible reply does not excuse a failed path. Fix before ship: block `draft_reply` until `get_ingestion_status` and `compare_weeks` have run.
