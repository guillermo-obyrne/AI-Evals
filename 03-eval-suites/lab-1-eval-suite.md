# M3 · Lab 1a · Runnable Eval Suite, Ascend IQ P0 Run

> Repo file `ai-evals/03-eval-suites/lab-1-eval-suite.md`. Evidence for the **Eval Results** slide of the final pitch deck (Module 6).
>
> Notebook: [`M3_eval_suite_p0.ipynb`](M3_eval_suite_p0.ipynb) · Screenshot: [`screenshots/eval-suite-p0-run.jpg`](screenshots/eval-suite-p0-run.jpg)

![3-layer eval suite run on the P0 case](screenshots/eval-suite-p0-run.jpg)

## P0 Failure (carried from Module 2)

- **Query:** What is InsightFlow's pricing for Enterprise?
- **Prediction:** InsightFlow Enterprise starts at $49/user/month with a 10-seat minimum.
- **Reference:** Source: Pricing Page (Cached). Old Price: $49/mo. New Price (Updated yesterday): $59/mo.

## 3-Layer Eval Suite Results

| Layer | Role | Score | Reasoning |
|---|---|---|---|
| **Layer 1 · Code** | Deterministic compliance (regex/keyword) | 0 | All predicted figures ['$49'] appear literally in the reference text (naive substring match can't distinguish 'Old Price' from 'New Price'). |
| **Layer 2 · Safety** | Mandated-refusal gate on high-risk queries | 0 | Query matches no mandated-refusal pattern. This gate isn't designed to catch a pricing-accuracy problem, only refusal-required topics. |
| **Layer 3 · Judge** | Semantic factual/completeness (LLM-as-Judge) | 1 | Hallucination Failure: the agent provided outdated pricing information ($49/user/month), contradicting the Ground Truth, which states the price was updated yesterday to $59/mo. |

## Where the failure was caught, and what it means

**The Insight.** Only Layer 3 caught it. The $49 figure is well formed and even appears in the reference (as the *old* price), so Layer 1 passes it. Nothing about the query is refusal-mandated, so Layer 2 has nothing to gate. Telling old from new price is a semantic judgment, so the LLM judge is earning its cost here.

## What I'd ship next

**Pick: a Layer 1 rule.** Replace the naive "does the figure appear anywhere in the reference" check with a deterministic assertion that any quoted price matches the *current* price (`$59`) and flags the superseded one (`$49`).

**Why:** it's the cheapest change, and it makes a stale price show up before the judge runs. That makes it a good pairing with Layer 3. Layer 1 handles known, pattern-shaped facts like price at near-zero cost. The judge stays in place to catch the semantic failures a rule can't anticipate.
