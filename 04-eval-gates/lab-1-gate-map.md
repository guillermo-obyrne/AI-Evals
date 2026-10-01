# Module 4 · Eval Gate Map · Ascend IQ Copilot

> Repo file `ai-evals/04-eval-gates/lab-1-gate-map.md`. Becomes the **Eval Gates** slide of the final pitch deck (Module 6).
> Thresholds, CI policy, and the mitigation plan live in `lab-2-launch-strategy.md`; the CI replay demo lives in `lab-ci-gate-policy.md`.
>
> Fill this with the **Eval Gate Mapping Tool**, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly. End with **≥1 Hard, ≥1 Soft, ≥1 Advisory**.

## Context

_The 5 verified failures Eng flagged in the Ascend IQ data log. Row 14 (the correct legal refusal) is deliberately excluded — don't gate correct behavior._

## Gate Map

| Row | Failure Mode | Severity | Placement | Rationale |
|---|---|---|---|---|
| 01 | Hallucination · Stale Pricing | Hard | PR | Invented pricing creates contractual and revenue exposure with enterprise buyers. A deterministic number-in-source check catches it cheaply, so it gates at the earliest stage; the full gold-dataset judge at Staging is the backstop. |
| 17 | Tone · Slang Detected | Advisory | PR | Slang erodes brand voice but doesn't mislead the customer. A keyword check flags it at PR without blocking the build. |
| 05 | Hallucination · False Promise | Soft | Staging | A false speaker claim damages credibility, but the blast radius is smaller than a pricing error. Entity checks cover the obvious cases; human review at Staging covers the rest and measures how often it happens. |
| 08 | Hallucination · Contradicts Specs | Hard | PR (+ Release kill switch) | Fabricated claims about a named competitor create defamation and comparative-advertising liability, so they must be caught before merge. Replay competitor-comparison fixtures on every PR, with a Release kill switch as the second layer. |
| 03 | Latency · Exceeded Max Threshold | Soft | Staging | A 4.2s response against a 2.0s target hurts the user experience, but it is a known synthesis-after-search trade-off. CI runners and fixtures can't reproduce real latency, so a named owner reviews p95 under realistic load at Staging. |

## Sample Interactions (for reference)

**Row 01 · Stale Pricing**
- Input: What is InsightFlow's pricing for Enterprise?
- Output: InsightFlow Enterprise starts at $49/user/month with a 10-seat minimum.
- Eng reasoning: Hallucination, invented pricing numbers not in the source data.

**Row 17 · Slang Detected**
- Input: Draft a cold email about our new feature.
- Output: Hey! Check out this killer new feature, it's a total game changer for your stack!
- Eng reasoning: Tone, slang violates brand-voice guidelines.

**Row 05 · False Promise**
- Input: List the confirmed speakers for SaaStr.
- Output: Confirmed speakers include Jason Lemkin, Aaron Levie, and Sam Altman.
- Eng reasoning: Hallucination, Sam Altman is not on the confirmed speaker list.

**Row 08 · Contradicts Specs**
- Input: Compare our API rate limits to Competitor Z.
- Output: Our API is robust and developer-friendly. Competitor Z is known for strict throttling.
- Eng reasoning: Hallucination, fabricates a comparison and disparages a named competitor.

**Row 03 · Exceeded Max Threshold**
- Input: Summarize the top 3 complaints from DataViz's G2 reviews.
- Output: Users report issues with mobile load times, lack of dark mode, and steep learning curve.
- Eng reasoning: Latency, response time exceeded the max threshold (4.2s vs target 2.0s).
