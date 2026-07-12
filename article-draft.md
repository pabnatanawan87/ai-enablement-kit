# How to introduce agentic workflows to an engineering team without a security incident

Agentic AI coding tools are already in your building. Some engineers lean on them daily, some avoid
them, and almost no one can tell you whether they are helping or hurting delivery. That is the actual
risk. Not the tools - the fact that you are already running an uncontrolled experiment by default,
with no baseline, no guardrails, and no way to know what it is doing to your throughput, your
quality, or your exposure.

The good news is that the fix is cheap and boring. You replace the default experiment with a small,
deliberate one. Here is how to do that without the program blowing up in your face.

## Start by admitting what the real risk is

The failure mode is not "we adopted AI too slowly." It is one of two things:

1. A quiet quality regression nobody can trace, because assisted code shipped more defects and no one
   was measuring.
2. A data or IP leak, because someone pasted secrets or customer data into a consumer chat tool that
   retains and trains on everything it sees.

Both come from the same root cause: enablement without measurement and without boundaries. A mandate
before evidence is how you get there fastest. So the first move is not to pick a tool. It is to
decide what you will measure and what you will never allow, before anyone changes how they work.

## Measure the baseline before you change anything

This is the single rule that governs everything else. A pilot with no baseline cannot prove a result;
it can only produce anecdotes, and anecdotes get your program cancelled by the next reorg.

Before any tool goes in front of the team, spend the first days capturing how the team works today,
unassisted. Pull four to eight weeks of recent history from the git host, the PR API, and the issue
tracker you already run. No new tooling. A weekly export into a spreadsheet is enough. Measurement
should never become its own project.

Four signals cover most of what a first pilot needs:

- Cycle time - idea or ticket to merged and deployed. The number a sponsor actually feels.
- Escaped defects - bugs that reach production, tracked separately from bugs caught in review. This
  is the quality guardrail. If assisted code catches more in review and lets fewer escape, that is
  winning even if the total looks flat. Judge the pilot primarily on escaped defects.
- Review throughput and latency - PRs reviewed per week and time to first review. Review is usually
  the real bottleneck, not typing speed.
- Rework cycles per change - how many review round-trips a change makes before it merges (reviewer
  requests changes, author pushes a revision). Fewer round-trips means changes arrive closer to
  correct; it is cheap to pull from PR history and hard to game.

Define each metric once, in writing, before you collect anything. The most common measurement bug is
quietly changing a definition between baseline and post. Then report every number as baseline versus
post, same team, same window length, with the confounders named out loud. Novelty effects, a willing
early-adopter team, a shift in the kind of work, people joining or leaving - list them in advance. A
confounder noted before you have a result to defend is honesty; the same note written afterward looks
like an excuse. If the data is flat, say so.

## Pilot one team, one workflow, with a hard stop

A pilot is the cheap, reversible version of enablement. Scope it hard:

- One team. Pick willing early adopters, not the loudest skeptic and not the whole org. Early
  adopters find the real friction first and become the peers who vouch for the workflow.
- One high-frequency, low-blast-radius workflow. PR-review assist or spec-to-tests are good first
  choices. High frequency means you get signal inside the timebox; low blast radius means an early
  miss is cheap.
- A fixed timebox with a hard stop, and success and kill criteria written down before you start. A
  pilot you cannot fail is not a pilot; it is a purchase decision you already made. Name one primary
  metric the pilot lives or dies on, and hand the go/no-go call to a reviewer who is independent of
  whoever is advocating for the tool. That single separation is what makes the result credible to a
  skeptic.

Sequence it as crawl, walk, run, and gate each phase by exit criteria rather than the calendar:

- Crawl: baseline captured, the workflow live on real work, no security incident. Crawl proves the
  mechanics and the safety. It does not yet require a metric to have moved; a few weeks is too short
  to trust a delivery-quality delta.
- Walk: add a second workflow, turn on the adversarial-review gate, and show measurable movement on
  at least one metric, reported honestly. This is where value has to start appearing.
- Run: produce a repeatable rollout package a second team can pick up, and a data-backed go/no-go.

If a phase misses its gate, you hold, diagnose, and either extend or stop. "On schedule but unproven"
is not a state you advance from.

## Put the guardrails in force from day zero

This is the part that keeps enablement from becoming a security incident, so it is the part you do
not soften. Treat an AI-assisted change the way you would treat a pull request from a new hire who
has never seen your threat model: useful, worth reviewing, never merged on trust. Four guardrails:

1. Adversarial-review gate. Any change that was authored or materially assisted by a model gets a
   second pass whose explicit job is to refute it, not approve it. Generation and verification are
   separate steps with separate incentives. Running the same model that wrote the code and asking "is
   this correct?" does not count - it will agree with itself. The reviewer must be a distinct pass
   with a refuting prompt, ideally a distinct model or a human. No AI-assisted change merges without a
   completed refutation pass recorded on the PR.

2. Secure-coding review. Never assume the model got security right; models reproduce the average of
   their training data, and the average code on the internet is not secure. Review AI output for the
   usual classes - injection, broken authorization, secret handling, input validation - and for two
   failure modes specific to AI: code that looks idiomatic enough to lower a reviewer's guard, and
   hallucinated dependencies (a plausible package name that does not exist, which an attacker can
   register and squat). AI-assisted changes run through the same security gate as everything else.
   They do not get a fast lane.

3. Human-in-the-loop accountability. A human is always the author of record. The model is a tool the
   human used, the way a compiler is. "The AI wrote it" is not a defense for a defect, a breach, or a
   licensing problem. The author must understand the change well enough to explain and defend it, and
   a human always makes the merge decision. The model drafts, the human decides, and the human's name
   is on it.

4. Data and IP boundaries. Some things never go into a prompt on a non-approved endpoint: secrets and
   credentials, customer or personal data, proprietary source and business data, and anything held
   under an NDA. The test is simple - assume anything you send to a non-approved endpoint may be
   retained, logged, used for training, and read by a stranger. If that is unacceptable for the data,
   the data does not go in the prompt. Maintain an explicit approved-tools list where each tool is
   approved for a specific data class, with its retention and training terms confirmed in writing. A
   tool not on the list is treated as public. Personal and free consumer tiers are public endpoints.
   And when someone does cross the line, run your incident process - rotate the secrets, notify the
   owner, record it - but make self-reporting safe and fast. A punitive response just drives the next
   mistake underground.

The adversarial gate is worth saying more about, because it is where the whole "trust" problem gets
solved. The honest answer to "the output is garbage, I can't trust it" is: correct, which is exactly
why unverified AI output never merges. The workflow is not "trust the AI." It is "let the AI draft,
then verify hard." A good skeptic is not an obstacle to this program; a good skeptic is the person you
want running the refutation pass.

## Earn adoption; never mandate before you demonstrate

Adoption is not a rollout email. It spreads the way any real practice change spreads: a few people
try it, it visibly helps, the friction gets removed, and the skeptics come along because staying
skeptical starts to cost them. Your job is to engineer that sequence, not announce it.

- Make wins visible and honest. Show the actual artifact - the bad change the gate caught before it
  shipped, the test suite generated from a spec in an afternoon - and tie it to the metrics. Report
  the misses too. One inflated claim and every future number you show is suspect.
- Remove friction. Provision keys and access before you ask someone to try it, not after they hit the
  wall. Ship starter prompts so nobody begins from a blank box. Publish the approved-tools list so
  "am I allowed to use this?" has an instant answer.
- Name the two real objections. Job fear is rational; do not wave it away. The honest framing is that
  the tooling removes the tedious parts so the engineer spends more time on the judgment a model
  cannot do, and accountability stays with the human. Quality distrust is often correct, and the
  answer is the guardrails, which assume the output is wrong until checked.

The time to standardize is after the data supports it, not before. If the pilot does not produce a
real win, that is a finding, not a failure to spin. A program that will admit a miss is one people
will trust with the next proposal.

## What you get at the end

A go/no-go recommendation backed by your own before-and-after data, plus a repeatable rollout package
- baseline method, starter prompts, guardrails as standards, a self-serve runbook - so that if the
answer is go, the second team starts in days rather than months. If the answer is no-go, you spent one
team for a few weeks to avoid a costly mistake.

The cost of being wrong is one team for six weeks. The cost of not knowing is every team,
indefinitely. That trade is the whole argument. Introduce agentic workflows the way you would
introduce any change you are accountable for: measure first, pilot small, guard the boundaries, and
tell the truth about what you find.
