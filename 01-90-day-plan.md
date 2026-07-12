# 90-Day Plan: Crawl, Walk, Run

This is the operating plan behind the pilot in `00-exec-pitch.md`. It runs one team through three
30-day phases. Each phase has explicit entry criteria (what must be true to start) and exit criteria
(what must be true to advance). If a phase misses its exit criteria, we do not advance on schedule;
we hold, diagnose, and either extend or stop. That discipline is the point.

The single rule that governs everything below: **measure the baseline before we change anything.**
A pilot with no baseline cannot prove a result. It can only produce anecdotes, and anecdotes are how
these programs get cancelled by the next reorg. We spend the first days of Crawl instrumenting the
team as it works today, unassisted, so that every later claim has a before-and-after behind it.

The metric definitions and the cheap way to collect them live in `03-metrics-framework.md`. The
guardrails referenced throughout live in `04-guardrails.md`. This document is the schedule and the
gates; those documents are the substance.

---

## Phase 0: Baseline (Days 0 to 30, front half of Crawl)

Baselines come first, inside Crawl, before any tool is put in front of the team for real work. This
is not a separate phase on the calendar; it is the non-negotiable opening of Crawl, called out on
its own because skipping it is the most common way these pilots fail.

### What we measure

Same four metrics we will report on at the end, defined once in `03-metrics-framework.md`:

- Rework cycles per change (review round-trips before merge).
- Defect density, with escaped-to-production tracked separately from caught-in-review.
- Cycle time (ticket accepted to merged or deployed).
- Review throughput and latency (PRs reviewed per week, time-to-first-review).

### How we capture it

- Pull four to eight weeks of recent history from the git host / PR API and the issue tracker for
  this exact team. No new tooling. A weekly export into a spreadsheet is enough.
- Record the window length and team composition. Every later comparison uses the same team and the
  same window length so we are not comparing a two-week sprint to a six-week one.
- Write down the known confounders now, before we have a result to defend: team size changes, a
  holiday, an unusually large migration, a reorg. Confounders noted in advance are honest; the same
  note written after the fact looks like an excuse.

### Exit criteria for the baseline (must all be true before the pilot workflow goes live)

- [ ] A documented baseline exists for all four metrics, with the window length and team recorded.
- [ ] Known confounders are written down and shared with the reviewer.
- [ ] The pilot proposal (`02-pilot-proposal-template.md`) is filled in and signed off: one team,
      one workflow, target delta, timebox, guardrails, success and kill criteria, named reviewer.

If the baseline cannot be captured (data is not available, the team is mid-crisis), the pilot does
not start. We fix that first.

---

## Phase 1: Crawl (Days 0 to 30)

One team. One high-frequency workflow. Prove the mechanics work and that nothing breaks, before we
add any surface area.

### Entry criteria

- [ ] Baseline captured and confounders documented (Phase 0 exit criteria met).
- [ ] Approved-tools list published and the pilot's tool is on it (`04-guardrails.md`).
- [ ] Data and IP boundaries communicated to the team: what must never be pasted into a prompt.
- [ ] Access provisioned for every team member (keys, seats, permissions) so no one is blocked on
      logistics in week one.

### What happens

- Pick one high-frequency, low-blast-radius workflow. Good first choices: PR review assist, or
  spec-to-tests. High frequency means we get signal fast; low blast radius means an early miss is
  cheap and reversible.
- Hand the team a starter kit: vendor-neutral prompt templates from `06-starter-prompts/` and a
  short "how to run it here" note. People should be productive on day one, not authoring prompts
  from scratch.
- Run weekly office hours: a standing 30-minute slot to unblock, collect what is working and what is
  friction, and fix the friction fast. This is where adoption is won or lost.
- Keep collecting the same four metrics on the same cadence. Do not stop instrumenting when the tool
  goes live; that is the "after" half of before-and-after.
- The human author remains accountable for every merged change (`04-guardrails.md`). Crawl does not
  relax that; nothing does.

### Exit criteria (must all be true to advance to Walk)

- [ ] Baseline captured and documented (carried forward from Phase 0).
- [ ] The pilot workflow is live and used by the majority of the team on real work, not demos.
- [ ] No security incident and no IP-boundary violation attributable to the pilot.
- [ ] At least three weeks of post-adoption metric data collected on the same cadence as baseline.
- [ ] Friction log exists with the top issues either resolved or triaged.

Note that Crawl's exit does not require a metric to have moved yet. Three weeks is too short to
trust a delivery-quality delta. Crawl proves the mechanics and the safety; Walk proves the value.

---

## Phase 2: Walk (Days 31 to 60)

Add the second workflow, turn on the adversarial-review gate, start sharing patterns, and begin
light metrics reporting. This is where the pilot has to start showing movement.

### Entry criteria

- [ ] All Crawl exit criteria met.
- [ ] The team is genuinely using workflow one (adoption confirmed, not assumed).
- [ ] The adversarial-review gate is understood by the team before it is enforced (see
      `04-guardrails.md`; the runnable version is the sibling `agentic-code-review` project).

### What happens

- Add a second workflow, chosen from what the team asked for during office hours. Demand-pulled
  adoption beats pushed adoption every time.
- Turn on the adversarial-review gate: AI-assisted changes get a verification pass (a human, or a
  second model instructed to refute the change) before merge. This is the generate-then-refute
  principle applied as a gate, not a suggestion.
- Start a shared prompt and pattern library, seeded from `06-starter-prompts/` and grown from what
  the team discovers. The library is how one person's good prompt becomes everyone's.
- Begin light metrics reporting: a short weekly note, baseline vs current, same team, same window
  length, confounders called out. Light. Measurement must not become its own project.
- Keep office hours running. Keep removing friction.

### Exit criteria (must all be true to advance to Run)

- [ ] Two workflows adopted by a majority of the pilot team.
- [ ] Measurable movement on at least one of the four metrics vs baseline, reported honestly with
      confounders named. Movement in the right direction on one metric is the bar; we are not
      claiming a transformation at day 60.
- [ ] Adversarial-review gate in force on AI-assisted changes, with no unreviewed AI-authored merge.
- [ ] Shared prompt/pattern library exists and has contributions from more than one person.
- [ ] Still no security incident or IP-boundary violation.

If no metric moves, we do not paper over it. We ask why (wrong workflow, too little adoption, a
confounder swamping the signal) and decide to extend Walk or stop. A flat pilot reported honestly is
worth more to the org than a glowing one built on anecdote.

---

## Phase 3: Run (Days 61 to 90)

Make it repeatable. The goal of Run is not to expand adoption on the pilot team; it is to produce a
package a second team can pick up, and a data-backed go/no-go for leadership.

### Entry criteria

- [ ] All Walk exit criteria met, including the one moved metric.
- [ ] The pilot team can operate the workflows without daily hand-holding.

### What happens

- Template the rollout for a second team: the pilot proposal, the starter kit, the guardrails, and
  the metrics setup, packaged so a new team can start Crawl on their own workflow.
- Promote the guardrails from pilot practice to written standards (`04-guardrails.md`): approved
  tools, data/IP boundaries, adversarial-review gate, human accountability. Standards, not lore.
- Hand the pilot team a self-serve runbook so they continue without the enablement lead in the room
  every week. If the program collapses the moment attention moves on, it was theater.
- Write the go/no-go recommendation from the data: baseline vs 90-day result across all four
  metrics, confounders named, cost noted, and a clear recommendation to expand, hold, or stop.

### Exit criteria (the deliverables of the whole 90 days)

- [ ] A repeatable rollout package exists: proposal template, starter kit, written guardrails,
      metrics setup. A second team could start from it without reinventing the pilot.
- [ ] A self-serve runbook is in the pilot team's hands and they have used it unaided at least once.
- [ ] A go/no-go recommendation, backed by baseline-vs-post data across all four metrics, delivered
      to the named reviewer.
- [ ] Guardrails ratified as standards.
- [ ] Zero security incidents and zero IP-boundary violations across the full 90 days.

---

## How the phases gate

```
Baseline ----> Crawl ----> Walk ----> Run
   |             |           |          |
 data +       workflow    one metric  repeatable
 proposal     live +      moved +     package +
 signed       no incident two flows   go/no-go
```

Advancement is by exit criteria, not by the calendar. A phase that misses its gate holds; we
diagnose and either extend or stop. "On schedule but unproven" is not a state we advance from.

## What would make me stop the pilot early

Honesty about failure is what makes the success credible. I would recommend stopping before day 90
if any of these hold:

- A security incident or IP-boundary violation attributable to the pilot. Non-negotiable.
- Adoption never reaches a majority of the team despite friction being removed. That means the
  workflow is wrong for this team, and pushing harder wastes goodwill.
- No metric moves by the end of Walk and no credible confounder explains it. The value is not there
  on this workflow; say so and stop.
- The measurement itself becomes a burden that slows the team. The instrumentation is supposed to be
  a weekly export, not a second job.

Stopping on this evidence is a successful pilot. It answered the question cheaply. That is the whole
point of doing it small, time-boxed, and measured.
