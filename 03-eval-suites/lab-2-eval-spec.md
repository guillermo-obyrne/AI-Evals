# M3 · Lab 2 · Eval Spec, Ascend IQ P0

> Repo file `ai-evals/03-eval-suites/lab-2-eval-spec.md`. The PM's contract for what "good" means and how it's enforced.

## Part 1 · The 5-Part Eval Spec

| Question | Answer |
|---|---|
| **01 · Target Risk** | The agent states a stale, unsupported, or contradicted price as verified fact (the M2 P0: $49 quoted when the current price is $59). Scope for now: pricing. Other fact types (rate limits, HQ, titles, integrations, speakers) are follow-on. |
| Risk Type | Output |
| Trust Metric | Hallucination |
| **02 · Evaluator** | Hybrid: deterministic lookup (Layer 1) plus LLM-as-Judge (Layer 3) |
| Detection logic | Layer 1: extract each price claim from the response, normalize currency, unit and billing period, and compare it to the current value in the verified pricing record. Block on no matching record, a value mismatch, or a stale record. Layer 3: an LLM judge reviews claim types with no verified record (people, integrations) against the retrieved source. The judge is advisory until calibrated. |
| **03 · Threshold** | 0 stale or contradicted prices rendered on the golden pricing set (N cases, N to be set when the set is built; the set must include the $49 vs $59 case), with a false-block cap on correct answers to be set from the same set. The judge half is advisory until κ ≥ 0.60; the async judge calibration lab is not yet complete, so no Cohen's κ has been measured. |
| Strategy | Safety First (max TPR) |
| **04 · Business Stakes** | A wrong price presented as verified breaks the core promise of verified intelligence. The most immediate exposure is a $50k+ contract, with wider brand damage if a Fortune 500 leader repeats the number to their own leadership. A false negative (a wrong price reaching the user) costs far more than a false block (a correct answer withheld), which is why the strategy is Safety First. |
| **05 · Owner** | Product Manager: accountable for the eval program and the threshold. Engineering owns the pipeline; monitoring is automated and enforced through CI gates. |

## Part 2 · Three Audience Messages

### A. For Engineering (Jira ticket)

```
GIVEN a candidate response R, before render
AND an extractor returns price claims C1…Cn
WHEN the gate evaluates each Ci, after normalizing currency, unit, and billing period
THEN
  IF no stats.db record matches (entity, field)         → FAIL: unsupported
  IF a record matches AND Ci.value ≠ record.value       → FAIL: contradicted
  IF record.last_verified is older than N days          → FAIL: outdated   [N = 1 day]
AND IF any Ci FAILs → do not render R; return fallback F1;
                      ALWAYS tag trace p0_fact_block {claim, db_value, reason, request_id}
AND ELSE → render R
AND the gate passes only if 0 FAIL-class claims render on the golden set
    (the golden set includes the $49 vs $59 case and also measures correct answers wrongly blocked).
```

### B. For UX / Design

When a pricing guardrail is tripped, show a graceful conversational message in place of the figure:

> "I couldn't verify [Competitor]'s current pricing against Ascend's data, so I've left that figure out rather than risk giving you a wrong number. The other figures below passed our checks. Want an analyst to send you the confirmed figure at {user.email}? Usually within {SLA}."

- No wrong figure or error code appears in the chat.
- Keep session context so the user can continue with follow-up questions.
- Do not surface the unverified claim; keep the verified content.
- `{SLA}` is a placeholder until the analyst turnaround is decided.

### C. For Leadership (bi-weekly update)

Ascend IQ will no longer show a price that doesn't match our verified pricing record: it withholds the figure and offers an analyst follow-up instead, protecting the $50k+ contracts that depend on trusting our numbers. Coverage starts with pricing; other fact types follow once the judge passes calibration. Tracking weekly: block rate, analyst-handoff volume, and wrong prices reaching users (target: zero).
