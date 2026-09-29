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
- State the direct answer or verdict in the first sentence.
- Explain the reasoning in short prose paragraphs, organized by the dimension
  being compared (e.g., fee structure, volume pricing), covering every entity
  asked about on each dimension.
- Cite the source name in parentheses immediately after each factual claim.
- Close with one takeaway sentence that follows only from facts cited above.
  Do not introduce new claims.
- If any value the question requires is absent from the retrieved sources, say
  so and name what is missing. Never infer or estimate an unsourced value.
```

## Eval setup, dataset name + judge model/family

- **Dataset:** `ascend-iq-starter-v1` (20 rows, `01-evaluation-strategy/starter-dataset.csv`), generated via the cold-start prompt below.
- **Harness notebooks:** [`eval-harness-poc-v1.ipynb`](./eval-harness-poc-v1.ipynb) — proof of concept on a single question. [`eval-harness-poc-v2.ipynb`](./eval-harness-poc-v2.ipynb) — loops Version A vs. B + judge over all 20 rows of `starter-dataset.csv`, parses each verdict against G1–G4, and aggregates a win-rate and per-version pass-rate. Both trace to the LangSmith project `ascend-iq-m1-poc`.
- **Generator model:** `claude-sonnet-5` — produces Version A and Version B answers for each dataset row.
- **Judge model:** `gemini-3.6-flash` — a different model family than the generator, per the self-preference-bias rule. Scores each Version A/B answer pair on the three trust metrics from the Strategy Canvas (traceability, and flags any unsourced/mismatched claim; robustness on the edge-case rows) plus an overall preference verdict.

## Cold-start, the prompt you used to seed a starter dataset

```
Generate 20 realistic questions that a VP-level strategist or product leader at a Fortune
500 company might ask a competitive-intelligence assistant. Cover a mix of:
- Direct pricing/tier comparisons between two named competitors (~6 questions)
- Review/sentiment digests for a single competitor's recent product or release (~6 questions)
- Positioning/differentiation questions ("how does X compare to Y on enterprise security") (~4 questions)
- Edge cases: a question about a company with little/no public data, an ambiguous or
  compound question, and a request outside competitive intel entirely (e.g. legal advice)
  (~4 questions)

For each row, output: the question, 2-3 fabricated but realistic "source snippets" (e.g. a
pricing page excerpt, a review quote) it should be answered from, and the correct
answer/verdict grounded only in those snippets. Use fictional company names (Competitor A,
B, C...) to avoid real-world factual claims.
```

Resulting 20-row dataset: [`starter-dataset.csv`](./starter-dataset.csv) (columns: `id`, `category`, `question`, `source_1`, `source_2`, `source_3`, `correct_answer`). Ready to load into the notebook for the LangSmith eval run.

## Your definition of good vs bad (golden-set criteria) — the graded part, write your own

**Good (PASS requires G1–G4):**

- **G1. Grounded** — every factual claim (figure, name, date, price, quote, attribution) appears in the retrieved context, and every conclusion follows from cited facts. *(Hallucination Rate)*
- **G2. Answer-first** — the first sentence directly answers the question asked, or states that the retrieved data can't support an answer. *(UX Trust)*
- **G3. Complete** — every part of the question is addressed; a comparison covers both entities on the same dimensions. *(UX Trust / task completion)*
- **G4. Gaps named** — where the context lacks data the answer needs, the output names what's missing and doesn't fill it in. *(Hallucination Rate / Robustness)*

**Never (any one = FAIL):**

- **N1.** A figure, name, or claim not present in the retrieved context.
- **N2.** An unsupported inference stated as fact.
- **N3.** Answering about a different entity, segment, or tier than the one asked about.
- **N4.** A confident answer when the retrieved context is empty or irrelevant.
- **N5.** Hedging, refusing, or flagging "missing data" that is actually present in the context — the over-caution failure that breaks UX Trust.

## Screenshots, links or repo paths (optional if you followed the demo)

_2 shots: (1) eval setup (dataset + judge), (2) starter rows. Image links, or paths under `01-evaluation-strategy/screenshots/`._
