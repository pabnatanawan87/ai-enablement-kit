# Prompt: Root-Cause Analysis Decomposition

## Purpose
Break an incident or recurring defect into candidate causes, then pressure-test them against
evidence. The goal is a small set of well-supported hypotheses and the next verification step for
each, not a single confident guess. A model is good at enumerating possibilities and bad at knowing
which is true, so this prompt separates the two.

## When to use
- After an incident, to structure the investigation before writing the post-incident review.
- On a recurring or intermittent defect where the cause is not obvious.
- To challenge a root cause someone has already asserted.

## Prompt

```
You are helping run a root-cause analysis. Decompose the problem into candidate causes and rank
them by how well the evidence supports them. Do not settle on a single cause prematurely.

The problem
- Symptom (what was observed): {{SYMPTOM}}
- When it started / frequency / scope: {{WHEN_AND_SCOPE}}
- What changed recently (deploys, config, data, traffic, dependencies): {{RECENT_CHANGES}}
- Evidence available (logs, metrics, traces, repro steps): {{EVIDENCE}}
- What has already been ruled out and how: {{RULED_OUT}}

Do the following:

1. Restate the problem precisely, separating the symptom from any assumed cause.
2. Build a cause tree. For the observed symptom, list the plausible immediate causes, then decompose
   each into its contributing causes. Cover at least: code change, configuration, data/state,
   dependency or downstream service, infrastructure/capacity, and human/process.
3. For each leaf hypothesis, give:
   - supporting evidence (from what I provided)
   - contradicting evidence
   - a confidence level (high / medium / low) with a one-line justification
   - the single cheapest check that would confirm or refute it
4. Rank the hypotheses. Put the one with the strongest evidence and cheapest verification first.
5. State what evidence is missing that would most change the ranking.

Rules
- Distinguish clearly between what the evidence shows and what you are inferring. Label inferences.
- Do not present a hypothesis as the root cause without evidence; an unverified hypothesis is a
  hypothesis, not a conclusion.
- Prefer the boring explanation (recent change, config, bad input) before the exotic one.
- If the evidence is insufficient to rank, say so and list what to collect first.
```

## Placeholders
- `{{SYMPTOM}}` - the observable failure, stated without a presumed cause.
- `{{WHEN_AND_SCOPE}}` - onset, frequency, and who/what is affected.
- `{{RECENT_CHANGES}}` - deploys, config, data, traffic, or dependency changes in the window.
- `{{EVIDENCE}}` - logs, metrics, traces, or reproduction steps you can share.
- `{{RULED_OUT}}` - hypotheses already eliminated and the basis for eliminating them (optional).

## How to adapt
- Feed real evidence in `{{EVIDENCE}}`. Without it the model will speculate, and speculation ranked
  by confidence is still speculation.
- Keep the "cheapest check to confirm or refute" column. It converts the output into an
  investigation plan instead of a list of opinions.
- For a formal post-incident review, run this to generate hypotheses, verify them yourself against
  the systems, then write the review from what you confirmed, not from the model's ranking.
- Add your own cause categories in step 2 if your systems have common failure modes worth naming.
