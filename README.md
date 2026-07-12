# ai-enablement-kit

A documents-and-templates playbook for introducing agentic AI coding workflows to an engineering
team without turning enablement into a security incident. It is opinionated and evidence-first:
measure the baseline before changing anything, pilot on one team before any rollout, and keep the
guardrails in force from day zero.

Every file stands alone and is adaptable to a new org in under an hour. Nothing here is tied to a
vendor, an IDE, or a specific employer. All example values are placeholders - there are no real
metric numbers anywhere in the kit.

## What this is (and is not)

This is the management-and-methodology half of agentic enablement: the pitch, the plan, the metrics,
the guardrails, the change-management playbook, and a starter prompt library. It is the paperwork a
lead uses to get a pilot approved, run it honestly, and produce a defensible go/no-go.

It is not the runnable tooling. The one piece of runnable software the kit points to is the
adversarial-review gate, which lives in the sibling project `agentic-code-review` (see below). The
kit tells you what policy to write; that project is the working generate-then-refute loop over a
real diff.

## Who uses it

- An engineering lead, TPM, or enablement owner who has been asked to "figure out AI" and wants to
  do it in a way that survives a skeptical review.
- A VP or budget owner who needs a one-page case and a bounded, reversible ask before funding
  anything.
- A pilot team that needs starter prompts, guardrails, and a metrics setup they can run themselves.

## The order to read them

Read top to bottom. The numbering is the intended sequence.

| # | File | What it gives you |
|---|------|-------------------|
| 00 | `00-exec-pitch.md` | The one-page business case. Frames the real risk as the uncontrolled default experiment already running, names the four signals worth measuring, and ends with a single small ask: approve a 6-week pilot for one team. |
| 01 | `01-90-day-plan.md` | The operating plan behind the pitch. Crawl / Walk / Run with explicit entry and exit criteria per phase, a Phase 0 baseline called out on its own, and stop-the-pilot conditions. Advancement is gated by exit criteria, not the calendar. |
| 02 | `02-pilot-proposal-template.md` | A fill-in, one-page proposal. Forces the discipline: pick ONE workflow, capture the baseline first, name one primary metric, set a hard stop, and pre-commit success and kill criteria with an independent reviewer. |
| 03 | `03-metrics-framework.md` | The four generic delivery-quality metrics (rework cycles, defect density with an escaped-vs-caught split, cycle time, review throughput and latency), how to instrument them cheaply from the git host / PR API / issue tracker, and an honest confounders section. |
| 04 | `04-guardrails.md` | The four non-negotiable guardrails: adversarial-review gate, secure-coding review, human-in-the-loop accountability, and data/IP boundaries with an approved-tools list. The data/IP section is the one that keeps enablement from becoming an incident; it is deliberately firm. |
| 05 | `05-change-management.md` | How adoption actually spreads: willing early adopters, visible and honest wins, friction removed, and the two real objections (job fear, quality distrust) addressed directly. The rule: never mandate before demonstrating. |
| 06 | `06-starter-prompts/` | A small, vendor-neutral prompt library (PR review, spec-to-tests, commit messages, RCA decomposition, refactor proposals). Each is a paste-ready template with placeholders and an adapt note. |

If you only have five minutes, read `00-exec-pitch.md` and `01-90-day-plan.md`. Those two are the
core of the case.

## How the pieces fit

```
00 pitch  ---->  01 plan  ---->  02 proposal (per pilot)
                    |                  |
                    |                  +-- 03 metrics (what you measure)
                    |                  +-- 04 guardrails (what stays in force)
                    |                  +-- 06 starter-prompts (what the team runs)
                    |
                    +-- 05 change-management (how adoption spreads across all phases)
```

The pitch sells the pilot. The plan sequences it into Crawl / Walk / Run. Each pilot is written up
from the proposal template, judged by the metrics framework, and fenced by the guardrails. The
starter prompts are what the team actually uses day to day, and change management is the connective
tissue that gets people to adopt any of it.

## The runnable adversarial gate

The guardrails doc and the 90-day plan both reference an adversarial-review gate: AI-assisted
changes get a verification pass whose explicit job is to refute the change, not to approve it, before
merge. This kit states that as policy. The working implementation is the sibling project:

- `../agentic-code-review` - a vendor-neutral, generate-then-refute code-review engine that runs
  over a real diff or PR. It generates candidate findings, then spawns skeptic passes that try to
  prove each finding is not real, and keeps only the survivors. It also has an RCA mode that forces
  each root-cause hypothesis to cite and survive verification. Point your Walk-phase pilot team at
  that project for the automated gate; use `04-guardrails.md` here to write the surrounding policy.

## Companion article

`article-draft.md` distills 00 through 05 into a single public article, "How to introduce agentic
workflows to an engineering team without a security incident." It is the public face of the kit and
is written for a general engineering-leadership audience, not as internal documentation.

## Principles this kit is built under

- Clean-room. Generic methods only, built from scratch. No employer names, ticket IDs, or real
  internal metric values - illustrative placeholders like "Team A" only.
- Vendor-neutral. No AI vendor is assumed or required. Every prompt and every metric works with
  whatever capable model your org has approved.
- Plain ASCII throughout. No em dashes, en dashes, smart quotes, or arrow glyphs.
- Legible over clever. Each document is meant to be read in one sitting and adapted in under an hour.

## License

MIT. See `LICENSE`.
