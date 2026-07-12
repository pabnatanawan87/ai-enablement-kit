# Change Management

Adoption is not a rollout email. Agentic workflows spread the way any real practice change spreads:
a few people try it, it visibly helps, the friction gets removed, and the skeptics come along because
staying skeptical starts to cost them. Your job is to engineer that sequence, not to announce it.

This is the short, candid version. The one rule that governs all of it: never mandate before you have
demonstrated. A mandate on an unproven workflow buys you resentment and malicious compliance. Earn the
adoption first.

---

## Start with willing early adopters

Do not start with the whole team, and do not start with the loudest skeptic hoping to convert them.
Start with the two or three people who already want to try this. They exist on every team - find them
and give them the starter kit, the office hours, and your attention.

Early adopters do two things you cannot do from the front of the room: they find the real friction
before it hits everyone else, and they become the peers who vouch for the workflow. Engineers trust a
teammate who says "this saved me an afternoon" far more than they trust a slide. Pick people whose
judgment their peers already respect - a credible early adopter is worth more than an enthusiastic
one.

---

## Make wins visible

A win nobody sees did not happen, as far as adoption is concerned. When the pilot produces a real
result, show it - concretely and honestly.

- Show the actual artifact: the PR the adversarial gate caught before it shipped, the test suite
  generated from a spec in an afternoon, the RCA that came together in an hour instead of a day.
- Tie it to the metrics in `03-metrics-framework.md`. "Rework cycles on Team A dropped over the
  pilot window" lands harder than "the AI is great."
- Keep it truthful. Report the misses too. One inflated claim and every future number you show is
  suspect. Credibility is the whole asset here; do not spend it on a demo.

Small and real beats big and hypothetical. A concrete afternoon saved, shown to the team, does more
than a projected annual figure nobody believes.

---

## Remove friction

Every step between an engineer and a working tool is a place adoption dies. Enthusiasm has a short
half-life; if the first attempt requires a key they do not have and an access request that takes
three days, they are gone. Do the unglamorous work:

- Access and keys provisioned before you ask someone to try it, not after they hit the wall.
- Starter prompts ready to copy (`06-starter-prompts/`) so nobody starts from a blank box.
- The approved-tools list (`04-guardrails.md`) published, so "am I allowed to use this?" has an
  instant answer instead of becoming a blocker.
- Office hours and a chat channel where a stuck engineer gets unstuck in minutes, not days.

Measure your own friction: if onboarding a new pilot user takes more than an hour, that is the bug to
fix before you recruit the next one.

---

## Address the two real objections directly

Two objections are real and are usually unspoken. If you do not name them, they do not go away - they
just turn into quiet non-adoption. Name them first, honestly.

### "This is going to take my job."

Do not wave this away; it is a rational fear and dismissing it destroys trust. Be straight about what
the tooling does and does not change. The honest framing: this removes the tedious parts of the work -
boilerplate, first-draft tests, routine review passes - so the engineer spends more time on the
judgment the tools cannot do. Accountability stays with the human author (`04-guardrails.md`); the
work of deciding what is correct, secure, and worth building does not transfer to a model. An engineer
who is more effective is more valuable, not less. Say what you actually mean about roles rather than
issuing a vague reassurance, because people can tell the difference.

### "The output is garbage / I can't trust it."

This one is often correct, and the honest answer is: right, which is why unverified AI output never
merges. The entire guardrails doc exists because the model is confidently wrong on a regular basis.
The workflow is not "trust the AI"; it is "let the AI draft, then verify hard." The adversarial-review
gate and secure-coding review are the answer to the distrust - they assume the output is wrong until
checked. A skeptic is not an obstacle to this program; a good skeptic is the person you want running
the refutation pass. Point their distrust at the output, not at the initiative.

---

## Never mandate before demonstrating

Stated once more because it is the rule people break under pressure to show progress.

- Pull, do not push. Make the workflow good enough and visible enough that people ask for access.
  Demand that outpaces supply is the signal you are ready to widen the pilot.
- A mandate on an unproven tool teaches people to game the metric, not to do the work. You will get
  reported adoption and no real change.
- The time to standardize is after the data supports it - the Run phase of `01-90-day-plan.md`, with a
  go/no-go recommendation and numbers behind it. Standardize what already works; do not mandate what
  you hope will.
- If the pilot does not produce a real win, that is a finding, not a failure to spin. Kill or adjust
  the workflow honestly. A program that will admit a miss is one people will trust with the next
  proposal.

The whole approach is evidence-first: demonstrate, measure, make it easy, tell the truth about what
you find. Adoption that is earned this way holds up. Adoption that is mandated evaporates the moment
attention moves on.
