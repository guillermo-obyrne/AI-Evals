# Lab 1, Ascend Analytics Coverage Matrix

> Repo file `ai-evals/05-scale/lab-1-coverage-matrix.md`. Feeds the **Coverage / scale** slide of the final pitch deck (Module 6).
>
> Fill this with the **Coverage Matrix Tool**, then **Copy markdown** and paste it over this file. The headings below mirror the tool's output exactly. **Force ≥2 ❌ gaps** — an all-green matrix isn't real.

**Product:** AI-Powered Report Summaries (External). Generates executive summaries from confidential research reports; output goes directly to clients via email/web. (Product 01 of 4 in the Ascend Analytics portfolio.)

_Evidence note: the Hallucination method and thresholds are reused from the Ascend IQ evals (M3/M4) as the closest sibling product. They have not yet been run on Report Summaries._

## Coverage row

| Product | Hallucination | Bias | Latency | Toxicity | Drift Monitoring |
|---|:---:|:---:|:---:|:---:|:---:|
| AI-Powered Report Summaries (External) | ⚠️ | ❌ | ✅ | ❌ | ⚠️ |

## Method + Ground Truth (two ✅/⚠️ cells)

### Hallucination Rate (⚠️)
- **Method:** Frozen regression golden set (≥30 cases) replayed on every PR, scoring faithfulness (grounding) with the deterministic claim check plus the LLM-as-Judge grounding rubric from M3. Blocking CI gate per M4.
- **Ground truth:** The source research report. Pass = 0 stale, unsupported or contradicted claims rendered on the golden set (any claim not traceable to the source report fails the summary), with faithfulness at or above the floor of 95 and no more than 3 points of regression against `main`.
- **Why ⚠️, not ✅:** The judge is not yet calibrated (κ 0.33 on the first rubric; the later improvement was a stand-in re-measure, and κ for the real judge is unmeasured), and the evidence comes from Ascend IQ pricing, not summaries. Hallucination is still the top-priority risk and is already gated, which is why it is not the critical gap below.

### Latency (✅)
- **Method:** Automated logging of report-generation completion time, including the automated pre-send checks, compared against each report's scheduled send time. Reports held for human review are tracked separately.
- **Ground truth:** Pass = ≥95% of reports finished ≥30 minutes before the scheduled send.
- **Assumptions:** (1) The pipeline already logs job completion time automatically. (2) Reports are distributed proactively on the report's publish date, not requested on demand, so clients wait for nothing and "fast" means on time with a safe buffer. If on-demand summaries are added, a chat-style p95 response target would be needed.

## Strategic acceptance

**Accepted gap:** Bias (Fairness)

> We're willing to accept this risk for now because the product summarizes documents rather than making decisions about people, we have seen no skew in our audit so far, and a dedicated fairness eval needs a costly human-annotated dataset we don't have yet. **Kill criterion:** 2 confirmed complaints or escalations (or 1% of accounts, whichever is fewer) citing skewed or omitted content in a month, **or** skewed or omitted content found in 1 of 20 sampled summaries. The sample comes from the PM's sampled human review of outgoing summaries, which uses a dual rubric (Toxicity and Bias) in month one. Once Layer 2 ships, the Toxicity gate takes over that check and the PM's review continues on Bias only, so the sample keeps running. If either trips, the acceptance ends and Bias becomes the critical mitigation, with an owner and a date set at that point.

## Critical mitigation

- **Critical gap:** Toxicity (UX Trust)
- **Why critical:** Summaries go straight to clients by email. An email can't be recalled and can be forwarded, and it carries our brand. The M2 audit on Ascend IQ also found off-brand slang reaching customer-facing drafts (UX_TRUST, P1), so this is an observed failure type, not a hypothetical. Because summaries are prepared before sending, a pre-send gate is cheap and effective.
- **Mitigation plan:** A two-layer pre-send gate on every summary. **Layer 1:** a regex/blocklist scan. **Layer 2:** an LLM-as-Judge tone rubric calibrated on a 50-case gold set (clear-toxic and borderline cases, including the M2 slang examples, synthetic ones labeled as such) and required to reach κ ≥ 0.60. Anything flagged is held for human review before it is sent. **Interim control:** until Layer 2 ships, the PM runs a sampled human review of outgoing summaries with a dual rubric (Toxicity and Bias). **Owner and timeline:** the PM is accountable and builds the gold set, rubric and κ check within 2 weeks; Engineering ships the Layer 2 pre-send hold within 4 weeks.
