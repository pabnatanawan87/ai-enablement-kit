# Metrics Framework

A small, generic set of delivery-quality metrics for judging an agentic-workflow pilot, plus the
cheapest honest way to collect them. The goal is evidence, not a dashboard project. A weekly export
into a spreadsheet is enough for a pilot; if measurement grows into its own initiative, you have
overbuilt it.

Two rules run through everything here:
1. Measure the SAME team over the SAME window length, baseline versus post. A comparison across
   different teams or different-length windows is not a comparison.
2. There are NO real numbers in this document. Every value you see is a placeholder. Fill in your
   own, and never paste a real metric from any prior employer into a template.

---

## The metrics

Four metrics cover most of what a first pilot needs. Resist adding more; each extra metric is another
thing to instrument, another confounder, and another way to lose the plot.

### 1. Rework cycles per change
- What: the number of review round-trips a change goes through before it merges. A round-trip is
  "reviewer requests changes -> author pushes a revision."
- Why: it is a direct read on how much a change churns before it is good enough to ship. It is
  cheap to pull and hard to game.
- Direction: lower is better (fewer round-trips to reach merge).
- How to read it: a fall suggests changes arrive closer to correct. Watch that it is not falling
  because review got laxer - pair it with defect density.

### 2. Defect density (with escaped-vs-caught split)
- What: defects attributable to a change, divided by a unit of change (per change, per merged PR, or
  per some fixed unit of code volume - pick one and keep it fixed).
- The split that matters: track two series separately.
  - Caught in review: defects found before merge / before production. More of these can be a GOOD
    sign - the review gate is working.
  - Escaped to production: defects found after release. Fewer of these is the real prize.
- Why the split: a single blended defect number hides the thing you care about. A workflow that
  catches more in review AND lets fewer escape is winning even if its total defect count looks flat.
- Direction: escaped-to-production lower is better; caught-in-review is context, not a target to
  minimize.
- How to read it: judge the pilot primarily on escaped defects. Rising caught-in-review with falling
  escaped defects is a healthy pattern, not a regression.

### 3. Cycle time
- What: elapsed time from idea/ticket opened to the change merged or deployed.
- Why: it is the metric a sponsor feels. It captures the whole path, not just coding.
- Direction: lower is better, within reason. Do not chase it at the expense of escaped defects.
- How to read it: decompose if you can (ticket-open to first-commit, first-commit to PR-open,
  PR-open to merge). Agentic assist usually moves the coding and review segments, not the waiting
  segments, so a blended number can understate or overstate the effect.

### 4. Review throughput and latency
- Throughput: number of PRs reviewed in the window.
- Latency: time-to-first-review (PR opened to first substantive review comment).
- Why: review is the usual bottleneck, and an assist workflow can either relieve it or flood it.
- Direction: throughput higher is generally better; latency lower is better.
- How to read it: watch for the failure mode where throughput rises because reviews got shallower.
  Cross-check against escaped defects.

---

## Cheap instrumentation

You already have the data sources. You do not need a new tool.

| Metric | Primary source | How to pull it |
| --- | --- | --- |
| Rework cycles per change | Git host / PR API | Count "changes requested" review events, or revision pushes, per PR. |
| Defect density - caught | PR API + issue tracker | Count defect-labeled review comments or pre-merge defect tickets per change. |
| Defect density - escaped | Issue tracker | Count defect tickets opened after release, linked back to the change. |
| Cycle time | Issue tracker + git host | Ticket-created timestamp to merge/deploy timestamp. |
| Review throughput | PR API | Count reviewed PRs in the window. |
| Review latency | PR API | PR-opened timestamp to first-review timestamp. |

Practical notes:
- A weekly export to CSV, dropped into one spreadsheet with a baseline column and a post column, is
  sufficient. Do not build a pipeline for a 6-week pilot.
- Define each metric once, in writing, before you collect anything. The most common measurement bug
  is quietly changing a definition between baseline and post.
- Pick fixed units and keep them fixed. If defect density is "per merged PR" at baseline, it is "per
  merged PR" at post.
- Prefer counts you can pull from an API over anything a human has to tally by hand. Hand-tallied
  metrics decay the moment the pilot gets busy.

---

## Baseline-versus-post discipline

- Capture the baseline BEFORE any tooling is introduced. A baseline captured after the fact is not a
  baseline.
- Use the same team and the same window length for both. If baseline is 4 weeks, post is 4 weeks.
- Report both numbers side by side, always. A post number alone means nothing.
- Pre-commit to which metric is primary (see `02-pilot-proposal-template.md`). Deciding after the
  results are in which metric "really mattered" is how pilots lie to themselves.

Example layout (values are placeholders):

| Metric | Baseline (4 wk) | Post (4 wk) | Change |
| --- | --- | --- | --- |
| Rework cycles per change | [value] | [value] | [direction] |
| Defect density - escaped | [value] | [value] | [direction] |
| Defect density - caught | [value] | [value] | [direction] |
| Cycle time | [value] | [value] | [direction] |
| Review latency | [value] | [value] | [direction] |

---

## Confounders - state them honestly

A pilot is not a controlled experiment. Name the things that could explain your result other than the
workflow, and say which ones you could not rule out. A report that lists its own confounders is more
credible than one that claims a clean win.

Common confounders in a short pilot:
- Sample size. A few weeks and a handful of engineers is a small sample; treat movement as a signal
  to investigate, not proof.
- Novelty and Hawthorne effects. People measured, and people using a new tool, behave differently for
  a while. Early gains may fade; early friction may too.
- Selection bias. A willing early-adopter team is not the average team. Good for a first read, not
  for extrapolating org-wide.
- Work-mix shift. If the kind of work changed between baseline and post (more features, fewer bug
  fixes, a big refactor), the metrics shift with it regardless of tooling.
- Team changes. People joining, leaving, or being pulled onto incidents move every metric.
- Definition drift. Covered above; the most self-inflicted confounder. Lock definitions up front.
- Gaming and shallow review. Metrics that reward speed can quietly erode review depth. Always read
  the speed metrics against escaped defects.

How to handle them: you usually cannot eliminate confounders in a pilot, so do not pretend to. List
them, note which plausibly affected the result, and let the outcome reviewer weigh them. If the
signal only holds by ignoring a confounder, it is not a signal yet.

---

Related documents:
- `02-pilot-proposal-template.md` - where baselines and targets get recorded per pilot.
- `04-guardrails.md` - the quality gates that keep the speed metrics from being gamed.
