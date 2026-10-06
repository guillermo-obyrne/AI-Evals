# Ship/Hold Memo · Ascend IQ

> Repo file `ai-evals/06-culture/lab-1-ship-hold-memo.md`. A Pyramid-Principle executive memo: the recommendation comes **first**.
>
> Fill this with the **Ship/Hold Memo Builder**, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly.

> **Decision:** 🛑 HOLD

**To:** [CPO] · cc Eng Lead · Trust & Safety
**From:** Guillermo O'Byrne · AI Evals Cohort · October 5, 2026

## The Answer

**Hold Ascend IQ for one sprint:** it can still present a stale price, like $49 instead of $59, as verified intelligence, and a wrong figure forwarded among executives can tarnish the brand and put $50k+ contracts at risk faster than a staged rollout can contain it.

We ship when three conditions are met: (1) 0 stale or contradicted prices rendered on a 150-case pricing set (75 should-block cases, including $49 vs $59, and 75 should-pass cases), built before any rollout; (2) a judge calibrated to κ ≥ 0.60 on the real judge; and (3) a false-block rate of 5% or less on correct answers. The sprint also measures the gate's added latency, and total p95 must stay within the 4.0s gate. A go/no-go review at the end of the sprint decides: if the conditions hold, we begin the staged 5% → 25% → 100% rollout; if they don't, we escalate rather than ship.

**Scope.** Pricing is 1 of the 10 hallucination failures in the audit, but it is the highest-severity one. The sprint ships a pricing-scope gate that blocks stale prices before they render rather than repairing the model. At launch, any fact type without a verified-record check (speakers, titles, HQ, integrations) is withheld or flagged as unverified instead of stated as verified. When the gate blocks a figure, the user sees a plain message ("I couldn't verify this figure, so I've left it out") with an analyst follow-up, and keeps the verified content.

**Sample size.** 150 cases is a starting bound, not proof: zero failures in 75 cases supports only a rate below about 4% on each side. A larger set (toward 500) grows from real staged-rollout cases after launch.

## The Arguments

### 1. Trust: the failure we have is the promise we sell

Ascend IQ promises verified market intelligence, and hallucination is where it fails most: 10 of the 20 audited rows (our P0), though only 1 of those 10 is a pricing error. A wrong value presented as verified, like $49 instead of $59, breaks the one thing that differentiates the product. A VP who repeats that number to their own leadership will not trust the next answer. The 10/20 figure is a small audit sample, not a measured production rate. A rough 95% interval on it runs from about 27% to 73%, and even the low end is far above the 0% Hard gate. The $49 vs $59 case is a confirmed counterexample to that gate, not an estimate.

### 2. Business risk: a staged rollout can't contain a screenshot

The exposure is $50k+ contracts and the brand. A wrong price shown to even 5% of traffic can be screenshotted and forwarded among executives within hours, so limiting the audience limits the cost of a miss far less than it does for a slow or clunky answer. Launching a day late costs a window; launching with a visible error can cost the accounts the launch is meant to win.

### 3. Eval readiness: the gate and the judge are not proven yet

The Hard gate requires 0% stale or contradicted claims rendered, but the 150-case pricing set (including $49 vs $59) is not yet built and the deterministic pricing gate has not run against it. The real judge scored Cohen's κ 0.33 against a 0.60 bar; the later 1.00 came from a stand-in judge and does not count. Both are concrete, one-sprint fixes, which is why this is a Hold and not an open-ended delay.

## Evidence · Trust Metrics

```
- Hallucination failures in audit: 10/20 rows, small sample, rough 95% interval 27%–73% (Gate: 0% stale/contradicted claims rendered) FAIL · M2 audit-log, M4 §4.0
- Judge Cohen's κ, real judge: 0.33 (Gate: ≥ 0.60) FAIL · M3 judge-calibration
- Pricing set: not built (Gate: 150 cases, 75 block incl. $49 vs $59 / 75 pass; a starting bound, not proof) NOT MET · M3 eval spec, M4 CI policy
- Pricing gate on the 150-case set: not run (Gate: 0 stale prices; false-block ≤ 5%; added latency measured) NOT MEASURED · M3 eval spec, M4 §4.0
- p95 latency: not measured (Gate: ≤ 4.0s, target 2.0s) NOT MEASURED · M4 §4.0
- Bias coverage: no measured % (accepted risk; 0/20 fairness failures in audit) NO GATE FAILURE · M2 audit-log, M5
- Planned eval allocation: $163,750 of $200K quarterly cap, 2 of 3 L3 slots (Gate: ≤ $200K; a plan, actual spend not reported here) WITHIN CAP · M5 Lab 2
```

## Business Risk

**SHIP path:** In the M2 audit, 10 of 20 sampled responses (50%) contained a stale, unsupported or contradicted fact, and the evals that would catch it are incomplete: the 150-case pricing set is not built, the pricing gate has not run, and the real judge is uncalibrated (κ 0.33). Without changes, the accounts pitched for the staged rollout would be exposed to that failure rate with no reliable check in front of them. Each is a potential $50k+ contract.

**HOLD path:** A one-sprint delay. Holding protects the same $50k+ contracts from exposure to an unfixed failure. It also protects the brand: a wrong figure forwarded among executives, or posted publicly, spreads faster than a rollout can contain, and the reputation damage and churn could outweigh the cost of the lost contracts. We are not quantifying lost revenue from the delay, because we have no measured figure for it.

## Next Step · Decision Needed

**Approve a one-sprint Hold by Friday, October 9, 2026.** The go/no-go review is held two weeks after approval: October 23, 2026 if approved on time (the date moves if approval comes later). At that review, the rollout begins only if all three ship conditions are met: 0 stale or contradicted prices on the 150-case pricing set, judge κ ≥ 0.60, and a false-block rate of 5% or less, with the gate's added latency measured. The 150-case set is built before any rollout. If they are not met, we escalate rather than ship.

## Reflection

Defining "good enough" forced three things on me.

**Hard metrics over ratings.** User ratings are tempting, but they arrive too late and are noisy. I first planned to track UX trust, then moved to Robustness because it can be measured directly before release.

**Floors and regressions work as a ratchet.** Setting a floor and a maximum regression for each metric is not trivial and takes experience. Once set, they act as a ratchet: the floor stops quality eroding across PRs, and the ideal score only moves up over time. The floor should only be raised when it is held, backed by evidence and approved. Otherwise it can damage speed by holding back releases. A tight setup can also be noisy when the test set is small.

**An added evaluator does not automatically improve the product.** It needs calibration. The analogy that made it click was a trainee grader: the human is the master evaluator, and the trainee cannot be trusted until their grades are checked against the master's. Working through TPR and TNR took the most thought, and the real judge's κ of 0.33 is why this memo says Hold.
