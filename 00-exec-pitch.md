# Agentic Enablement: A Measured Pilot

**Audience:** VP of Engineering (or equivalent budget owner)
**Read time:** about one page
**The ask is at the bottom.** It is small.

## The situation

Agentic AI coding tools are already in the building, used unevenly and unmeasured. Some engineers
lean on them daily, some avoid them, and no one can say whether they help or hurt delivery. That is
the actual risk: not the tools, but the absence of a controlled way to find out what they do to our
throughput, our quality, and our exposure. We are running an uncontrolled experiment by default.

This proposes replacing the default experiment with a small, deliberate one.

## What we would measure

The pitch is not "AI is faster." It is "let us measure whether it is, on our work, with numbers we
already collect." Four business-relevant signals, all pulled from the git host, the PR API, and the
issue tracker we already run:

- **Cycle time** - idea or ticket to merged and deployed. The headline delivery number.
- **Escaped defects** - bugs that reach production, tracked separately from bugs caught in review.
  This is the quality guardrail. If assisted code ships more escaped defects, the pilot fails, and we
  want to know that cheaply and early rather than after a broad rollout.
- **Review throughput and latency** - PRs reviewed per week and time to first review. Review is
  usually the real bottleneck, not typing speed.
- **Engineer leverage** - how much of an engineer's week goes to net-new value versus rote work
  (boilerplate, first-draft tests, commit messages, mechanical refactors). Leverage is the point;
  speed is just one way it shows up.

Every number is reported as baseline versus post, same team, same window length, with confounders
called out honestly. No vanity metrics. If the data is flat, we say so.

## Why a pilot and not a rollout

A mandate before evidence is how enablement turns into a security incident or a quality regression
that nobody can trace. A pilot is the cheap, reversible version:

- **Scoped:** one team, one high-frequency workflow (for example, PR-review assist or spec-to-tests).
- **Guarded:** every assisted change gets an adversarial verification pass before merge; a human
  author is always accountable for merged code; secrets, customer data, and proprietary code never
  leave approved tools. These guardrails ship with the pilot, not after it.
- **Reversible:** a fixed timebox and pre-committed kill criteria. If it does not move a metric
  without hurting quality, we stop. Nothing is locked in.

The cost of being wrong is one team for six weeks. The cost of not knowing is every team, indefinitely.

## What you get at the end

A go / no-go recommendation backed by our own before-and-after data, plus a repeatable rollout
package (baseline method, starter prompts, guardrails as standards, a self-serve runbook) so that if
the answer is "go," the second team starts in days, not months. If the answer is "no-go," we have
spent one team-window to avoid a costly mistake.

## The ask

**Approve a 6-week pilot for one team.** One team, one workflow, baselines captured first, guardrails
in force, a fixed kill switch, and a data-backed recommendation at the end. No new headcount, no new
platform commitment, no broad rollout until the numbers are in.
