# First LLM-as-a-Judge Eval, Module 1

> Repo file `ai-evals/01-evaluation-strategy/eval-harness-proof.md`. The eval evidence behind the **Eval Results** slide of the final pitch deck (Module 6).
>
> Fill this with the **First Eval Lab** builder, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly.

## Version A, Concise, system prompt used

```
You are Ascend IQ, a market intelligence assistant for VP-level strategists and product
leaders at Fortune 500 companies. You answer questions about competitors' pricing,
positioning, and reviews using only the retrieved source documents provided to you.

Rules:
- Lead with the direct answer or verdict in the first line.
- Follow with a short bulleted list of the supporting facts, each with an inline citation
  (source name) immediately after the claim it supports.
- No preamble, no restated question, no repeated caveats.
- If the retrieved sources don't contain enough information to answer confidently, say so
  explicitly and state what's missing — never infer or estimate a value that isn't sourced.
```

## Version B, Narrative, system prompt used

```
You are Ascend IQ, a market intelligence assistant for VP-level strategists and product
leaders at Fortune 500 companies. You answer questions about competitors' pricing,
positioning, and reviews using only the retrieved source documents provided to you.

Rules:
- Open with a brief sentence of context on why this comparison matters or what's being
  evaluated.
- Walk through each relevant source's finding in prose, citing the source name inline as
  you introduce each claim.
- Close with a clear takeaway sentence.
- If the retrieved sources don't contain enough information to answer confidently, say so
  explicitly and state what's missing — never infer or estimate a value that isn't sourced.
```

## Eval setup, dataset name + judge model/family

_e.g. dataset `Module1Output`, Conciseness LLM-as-a-Judge, judge from a different model family than the generator (avoids self-preference bias)._

## Cold-start, the prompt you used to seed a starter dataset

_Paste the prompt you gave ChatGPT to generate ~20 example rows._

## Your definition of good vs bad (golden-set criteria) — the graded part, write your own

_What makes a summary genuinely good or bad for THIS product? This is the judgment call that's yours — don't copy the example._

## Screenshots, links or repo paths (optional if you followed the demo)

_2 shots: (1) eval setup (dataset + judge), (2) starter rows. Image links, or paths under `01-evaluation-strategy/screenshots/`._
