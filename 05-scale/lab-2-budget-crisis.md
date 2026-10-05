# Lab 2, Ascend IQ Budget Crisis

> Repo file `ai-evals/05-scale/lab-2-budget-crisis.md`. Allocate Level 1/2/3 coverage across the 5 failure modes under the **$200K/quarter cap** with **max 3 at Level 3**.
>
> Fill this with the **Budget Crisis Tool**, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly.

**Quarterly budget cap:** $200,000
**Total Level 3 spend:** $150,000 (2 of max 3 L3 slots used)
**Total portfolio spend (all levels):** $163,750, leaving $36,250 of headroom

## Portfolio decision grid

| Failure | Trust metric | Risk | Level | Cost |
|---|---|---|---|---|
| Data Fabrication | Hallucination Rate | P0 | L3 | $85,000 |
| Context Specificity | UX Trust | P1 | L2 | $7,000 |
| Source Attribution Failure | Robustness | P1 | L3 | $65,000 |
| Data Bias | Fairness | P2 | L2 | $5,500 |
| Cost Overruns | Latency | P3 | L1 | $1,250 |

_Reference L3 costs: Hallucination $85K · Context $70K · Attribution $65K · Bias $55K · Latency $25K. L2 ≈ 10% of L3, L1 ≈ 5% of L3._

_Third L3 slot unused: the only remaining candidate that fits under the cap is Latency ($25K) on a P3 risk, which is already covered by the CI cost and latency checks and the Staging load test. Context ($70K) or Bias ($55K) at L3 would take the total past $200K._

## Fallback methods (non-Level 3 items)

### L2 · Context Specificity (UX Trust · P1)
- **Method:** Weekly review of a sample of logged responses, checking that each answer honors the scope of the request (for example, when asked to isolate negative reviews, it returns only negatives).
- **Why this fallback is defensible:** This is the P1 I downgraded to fund Source Attribution at L3. The failure degrades the quality of an answer without presenting a wrong "verified" fact, and it matches our M1 trade-off: a weaker answer costs a VP a moment, while a wrong or unsupported claim can cost the account. The M2 audit also found UX_TRUST failures in only 1 of 20 rows per failure type. A weekly log review still catches a rising trend.

### L2 · Data Bias (Fairness · P2)
- **Method:** Bi-weekly human audit comparing the share of US versus APAC sources cited in summaries with their share of the underlying corpus, to detect regional skew.
- **Why this fallback is defensible:** No #FAIRNESS-tagged failures appeared in the M2 audit, and it is a P2 risk, so continuous monitoring at $55K is not proportionate. A bi-weekly audit with a concrete comparison gives regular, checkable coverage for about a tenth of the cost.

### L1 · Cost Overruns (Latency · P3)
- **Method:** Infrastructure-level cost tracking with threshold alerting, plus spot-checks on deploy and the existing M4 CI "Cost per task" warn-only dimension (floor 70, max regression 10).
- **Why this fallback is defensible:** It is the lowest-severity risk and runs on infrastructure we already have. **Upgrade trigger:** move to L2 if spot-checks show a 25% or greater cost increase in 10% or more of sampled calls.
