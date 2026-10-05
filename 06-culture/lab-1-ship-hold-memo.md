# Ship/Hold Memo · Ascend IQ

> Repo file `ai-evals/06-culture/lab-1-ship-hold-memo.md`. A Pyramid-Principle executive memo: the recommendation comes **first**.
>
> Fill this with the **Ship/Hold Memo Builder**, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly.

> **Decision:** 🛑 HOLD

**To:** [CPO] · cc Eng Lead · Trust & Safety
**From:** Guillermo O'Byrne · AI Evals Cohort · October 5, 2026

## The Answer

**Hold Ascend IQ for one sprint:** it can still present a stale price, like $49 instead of $59, as verified intelligence, and one screenshot of that can tarnish the brand and put $50k+ contracts at risk faster than a staged rollout can contain it.

We ship when three conditions are met: 0 stale or contradicted prices rendered on a golden set of at least 30 cases (including the $49 vs $59 case), a judge calibrated to κ ≥ 0.60 on the real judge, and a false-block rate of 5% or less on correct answers. A go/no-go review at the end of the sprint decides: if the gate passes, we begin the staged 5% → 25% → 100% rollout; if it doesn't, we escalate rather than ship.

## The Arguments

### 1. Trust: the failure we have is the promise we sell

Ascend IQ promises verified market intelligence, and hallucination is where it fails most: 10 of the 20 audited rows (our P0). A wrong value presented as verified, like $49 instead of $59, breaks the one thing that differentiates the product. A VP who repeats that number to their own leadership will not trust the next answer.

### 2. Business risk: a staged rollout can't contain a screenshot

The exposure is $50k+ contracts and the brand. A wrong price shown to even 5% of traffic can be screenshotted and shared in an afternoon, so limiting the audience limits the cost of a miss far less than it does for a slow or clunky answer. Launching a day late costs a window; launching with a visible error can cost the accounts the launch is meant to win.

### 3. Eval readiness: the gate and the judge are not proven yet

The Hard gate requires 0% stale or contradicted claims rendered, but the golden pricing set (at least 30 cases, including $49 vs $59) is not yet built and the deterministic pricing gate has not run against it. The real judge scored Cohen's κ 0.33 against a 0.60 bar; the later 1.00 came from a stand-in judge and does not count. Both are concrete, one-sprint fixes, which is why this is a Hold and not an open-ended delay.

## Evidence · Trust Metrics

```
- Hallucination failures in audit: 10/20 rows (Gate: 0% stale/contradicted claims rendered) FAIL · M2 audit-log, M4 §4.0
- Judge Cohen's κ, real judge: 0.33 (Gate: ≥ 0.60) FAIL · M3 judge-calibration
- Golden pricing set: not built (Gate: ≥ 30 cases incl. $49 vs $59) NOT MET · M3 eval spec, M4 CI policy
- Pricing gate on golden set: not run (Gate: 0 stale prices; false-block ≤ 5%) NOT MEASURED · M3 eval spec, M4 §4.0
- p95 latency: not measured (Gate: ≤ 4.0s, target 2.0s) NOT MEASURED · M4 §4.0
- Bias coverage: no measured % (accepted risk; 0/20 fairness failures in audit) NO GATE FAILURE · M2 audit-log, M5
- Eval budget: $163,750 of $200K cap, 2 of 3 L3 slots used (Gate: ≤ $200K) PASS · M5 Lab 2
```

## Business Risk

_Quantified SHIP-path vs HOLD-path risk (revenue, churn, competitive window)._

## Next Step · Decision Needed

_A specific decision request with a deadline — e.g. "Approve the Hold rollback by Friday to keep the Q3 launch window."_

## Reflection

_What defining "good enough" forced you to confront._
