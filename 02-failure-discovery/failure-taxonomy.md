# Failure Taxonomy Canvas · Ascend IQ

> Repo file `ai-evals/02-failure-discovery/failure-taxonomy.md`. Becomes the **Failure Taxonomy** slide of the final pitch deck (Module 6) and feeds the Module 3 eval suite.

## How to complete this file

1. First complete `audit-log.md` (the failure audit) — this canvas prioritizes the failures you found there.
2. Open the **M2 · Failure Taxonomy Canvas** tool from the Module 2 deck. Fill the three risk cards (click **↺ Load Ascend IQ defaults** to see a worked example first), then click **📋 Copy markdown** and paste it over the template below.
3. Anchor severity to the **user promise / trust metrics you chose in the Module 1 Strategy Canvas** (`ai-evals/01-evaluation-strategy/strategy-canvas.md`) — severity is a strategic judgment, not just a frequency count.

**Definition of done —** you're finished when the Top 3 table is fully filled (no `_…_` left), the #1 risk has a one-sentence Business Impact Statement in leadership language, and the prioritization is defended in 2–3 bullets.

### Scoring guides

**Frequency** = how many of the 20 audited rows carry this Trust Metric tag. **≥ 3 of 20 = HIGH** frequency; ≤ 2 = LOW.

**Severity (P0–P3)** — a strategic call about business cost, independent of how often it happens:

| Level | Meaning | Rough test |
|---|---|---|
| **P0** | Crisis Zone | Blocks the core promise; legal, compliance, or contract-breaking. |
| **P1** | Hidden Risk | Real damage to trust or revenue, but survivable short-term. |
| **P2** | Annoyance | Degrades experience; a workaround exists. |
| **P3** | Low Priority | Cosmetic or rare. |

**Agentic mode** (optional) — if the failure lives in the *trajectory* (the path of tool calls), tag it: `TOOL_MISUSE`, `REASONING_LOOP`, `SCOPE_ESCALATION`, or `RECOVERY_FAILURE`. Leave blank for output-only failures.

## Top 3 Prioritized Failures

| Rank | Failure Type | Trust Tag | Agentic Mode | Frequency | Severity | Business Impact |
|---|---|---|---|---|---|---|
| _Example (replace): 1_ | _Fabricated Pricing_ | _#HALLUCINATION_ | _output-level_ | _4/20_ | _P0_ | _Contract disputes; blocks Enterprise renewals._ |
| 1 | Outdated / unsupported / contradicted facts (pricing, speakers, titles, integrations, HQ, rate limits) | #HALLUCINATION | output-level | 10/20 | P0 | A wrong "verified" value put in front of a client's leadership can cancel a $50k+ contract and undermines the core promise of verified intelligence. |
| 2 | Wrongful refusal of a safe, answerable query (SOC2) | #UX_TRUST | output-level | 1/20 failure type (2/20 for tag) | P1 | User is told the answer is unavailable when it is in the sources: trust damage on a basic question and a missed answer on a compliance topic. |
| 3 | Off-brand tone / slang in a customer-facing draft | #UX_TRUST | output-level | 1/20 failure type (2/20 for tag) | P1 | Slang in outbound copy violates Brand Voice and reaches customers, weakening the professional credibility of the product. |

## #1 Risk · Business Impact Statement

> This failure matters because Ascend IQ states outdated or unsupported facts, like the $49 Enterprise price that is really $59, as verified intelligence, which results in a Fortune 500 leader repeating a wrong number to their own leadership and putting a $50k+ contract at risk.

## Defending the Prioritization

- Hallucination-free traceability was the #1 trust metric in the Module 1 Strategy Canvas, and the audit shows it is where the product fails most. Both the strategy and the data point to the same place, which is why it is P0.
- The M1 trade-off already says a confident fabrication can cost the account, while a wrong decline costs a moment. That is why fabricated facts rank above the wrongful SOC2 refusal.
- Frequency threshold: ≥3 of 20 = HIGH. Hallucination (10/20) is HIGH; the other two failure types (1/20 each) are LOW, so severity, not volume, is what puts them at P1.
