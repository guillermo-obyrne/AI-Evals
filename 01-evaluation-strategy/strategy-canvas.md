# AI Evaluation Strategy Canvas

> Repo file `ai-evals/01-evaluation-strategy/strategy-canvas.md` (the repo is your submission).
> Becomes the **Strategy Canvas** slide of the final pitch deck you assemble in Module 6.
>
> Fill this with the **AI Evaluation Strategy Canvas** tool, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly.

## 1. Product Strategy, The Context

- **Target user:** VP-level strategists and product leaders at Fortune 500 companies.
- **Key use case:** Turning a natural-language request (e.g. "compare Competitor X and Y's enterprise pricing" or "give me a digest of recent reviews for Competitor Z") directly into a market insight — no forms to fill, no report to build by hand.
- **Value proposition:** Reliable, verified market intelligence on other enterprises without hours of manual research, analysis, and prep — enabling faster, more accurate decisions in high-stakes environments.

## 2. Measurements, The Execution

- **User promise.** _For VP-level strategists and product leaders at Fortune 500 companies, Ascend IQ promises to turn natural-language market intelligence requests into reliable, verified competitive insight, so that leaders make faster, more accurate decisions in high-stakes environments._
- **Top 3 trust metrics:**
  - **_Hallucination-free traceability_** — every factual claim in a response (a price, a review sentiment, a stat) is backed by a citation that actually supports the value stated. _Signal: % of responses containing at least one factual claim that is either (a) unsourced, or (b) sourced but not matching the cited source's value — target is to drive this defect rate toward zero._
  - **_Latency_** — time from prompt submission to a completed response; the primary usability blocker today, since VP users won't wait or dig through supporting material themselves. _Signal: p95 time-to-completion, tracked separately for simple look-ups vs. multi-competitor comparison/synthesis queries (to avoid a bimodal distribution masking regressions in either mode)._
  - **_Robustness to messy/out-of-scope input_** — on adversarial, ambiguous, or out-of-scope questions, the system declines or flags the data gap instead of fabricating an answer. _Signal: % of a held-out adversarial/out-of-scope test set where the system correctly declines or flags insufficient data, measured pre-release._
- **Why these three:** Hallucination-free traceability protects the core promise directly — a wrong cited value could get a contract cancelled and destroy trust in "verified" intelligence, which is our whole differentiator. Latency protects adoption — the promise is worthless if VP users route around a slow tool. Robustness is the other side of the hallucination coin: it catches the failure mode traceability alone can't (confidently fabricating on questions the system shouldn't answer at all), and it's testable pre-release against a fixed adversarial set.

## 3. Strategic Trade-Offs, The Cost

### Trade-off 1 · _Metric A ↔ Metric B_

_We prioritize A over B because … (business justification)._

### Trade-off 2 · _Metric C ↔ Metric D_

_We prioritize C over D because … (business justification)._

---
_Generated from the AI Evaluation Strategy Canvas, M1 lab tool, AI Evals Certification._
