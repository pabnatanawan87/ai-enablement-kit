# Pilot Proposal Template

Copy this file, fill in every field, and get it reviewed before any tooling is turned on. One page
is the target. If a section is hard to fill in, that is the section to think hardest about.

The discipline this template enforces is simple: pick ONE workflow, measure it before and after, and
pre-commit in writing to what success and failure look like. A pilot you cannot fail is not a pilot,
it is a purchase decision already made.

How to use it:
- Fill in the bracketed placeholders. Delete the guidance text in parentheses.
- Keep it to one page. If you cannot state the pilot on one page, it is not scoped tightly enough.
- The reviewer signs off on the proposal BEFORE the pilot starts, and again on the outcome AFTER.
- All example values below are illustrative placeholders. Replace every one.

---

## Pilot Proposal: [short name, e.g. "Team A PR-review assist"]

### 1. Team
- Pilot team: [team name, e.g. "Team A"]
- Team size in pilot: [number of engineers participating]
- Sponsor: [manager or lead who owns the team's time]
- Start date / end date: [date] to [date]

(Pick a willing team, not a mandated one. Early adopters make a fairer first test than skeptics or
zealots. One team only.)

### 2. Workflow chosen, and why
- Workflow: [one high-frequency workflow, e.g. "PR review assist" or "spec-to-tests"]
- Why this one: [why it is a good first target]

(Choose a workflow that is high-frequency, has a measurable output, and where a bad result is caught
before it reaches a customer. High frequency means you get signal inside the timebox. One workflow.
Adding a second one belongs in a later phase, not this pilot.)

### 3. Baseline metrics (captured BEFORE any tooling)
Capture the same metrics you intend to judge the pilot by, over a representative window, before the
team changes anything. See `03-metrics-framework.md` for definitions and cheap instrumentation.

| Metric | Baseline value | Window | Source |
| --- | --- | --- | --- |
| Rework cycles per change | [value] | [e.g. prior 4 weeks] | [git host / PR API] |
| Defect density - caught in review | [value] | [window] | [PR API / issue tracker] |
| Defect density - escaped to production | [value] | [window] | [issue tracker] |
| Cycle time (ticket to merged) | [value] | [window] | [issue tracker + git host] |
| Time-to-first-review | [value] | [window] | [PR API] |

(Baseline FIRST is non-negotiable. Without it there is no pilot, only an anecdote. Use the same
window length you will use for the post measurement.)

### 4. Target delta
State the movement you expect, per metric, as a direction and a rough magnitude. Be honest that this
is a hypothesis, not a promise.

| Metric | Baseline | Target after pilot | Direction |
| --- | --- | --- | --- |
| [primary metric] | [baseline] | [target] | [lower / higher is better] |
| [secondary metric] | [baseline] | [target] | [direction] |

(Name ONE primary metric the pilot lives or dies on. Secondary metrics are context, not the verdict.
Do not target every metric at once; that is how you end up unable to say whether it worked.)

### 5. Timebox
- Duration: [e.g. 6 weeks]
- Checkpoints: [e.g. weekly office hours; mid-point review at week 3]
- Hard stop: [date the pilot ends regardless of result]

(A pilot with no end date becomes permanent by default and is never evaluated. Set the hard stop
now.)

### 6. Guardrails in force
Reference the standing guardrails and confirm each is active for this pilot. See `04-guardrails.md`.

- [ ] Adversarial-review gate: AI-assisted changes get a verification pass before merge.
- [ ] Secure-coding review: AI output reviewed for injection / authz / secret-handling classes.
- [ ] Human-in-the-loop: a named human author is accountable for every merged change.
- [ ] Data and IP boundaries: no secrets, customer PII, or proprietary data sent to non-approved
      tools; only tools on the approved list are used.
- Approved tool(s) for this pilot: [tool name and endpoint]
- Accountable human author(s): [names or roles]

(If any box is unchecked, the pilot does not start. These are not aspirations; they are entry
conditions.)

### 7. Success criteria (pre-committed)
The pilot is judged a success if ALL of the following hold at the hard stop:
- Primary metric moved in the intended direction by at least [threshold], same team and same window
  length as baseline.
- No security incident and no escaped defect attributable to unreviewed AI output.
- A majority of the pilot team chooses to keep using the workflow when asked.

(Write these before you start. If you find yourself wanting to edit them once results are in, that is
the bias this section exists to prevent.)

### 8. Kill criteria (pre-committed)
The pilot is stopped early, or judged a failure at the hard stop, if ANY of the following hold:
- A security incident or data/IP-boundary violation traced to the workflow.
- Primary metric moves the wrong way, or does not move, beyond the timebox.
- The guardrails prove impractical and are being routinely skipped.
- The team's own read is that it costs more than it returns.

(A named kill condition is what separates a pilot from a foregone conclusion. Honor it.)

### 9. Reviewer and outcome sign-off
- Outcome reviewer: [name or role - who reads the results and makes the go / no-go call]
- Reviewer is independent of: [confirm the reviewer is not the person advocating for the tool]
- Decision after pilot: [ ] scale to a second team  [ ] extend / adjust  [ ] stop
- Sign-off (proposal): [reviewer] on [date]
- Sign-off (outcome): [reviewer] on [date]

(The person who decides whether the pilot succeeded should not be the person whose idea it was.
That single separation is what makes the result credible to a skeptic.)

---

Related documents:
- `01-90-day-plan.md` - where this pilot sits in the crawl / walk / run rollout.
- `03-metrics-framework.md` - metric definitions and how to instrument them cheaply.
- `04-guardrails.md` - the full statement of the guardrails referenced in section 6.
