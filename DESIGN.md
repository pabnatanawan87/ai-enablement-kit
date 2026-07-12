# ai-enablement-kit - Design

Read `../BUILD_PRINCIPLES.md` first. This project produces a set of documents and light templates,
each individually usable. Clean-room: describe methods generically; use no employer names, ticket
IDs, or real internal metric values as examples (use illustrative placeholders like "Team A").

## The deliverables (each a file to author)

### 1. `00-exec-pitch.md` - one page
The case for agentic enablement in business terms: faster cycle time, fewer escaped defects, higher
review throughput, and engineer leverage, against a small, time-boxed, measured pilot. No hype. One
page a VP can read. Ends with a single ask (approve a 6-week pilot for one team).

### 2. `01-90-day-plan.md` - crawl / walk / run
- Days 0-30 (Crawl): one pilot team, one high-frequency workflow (e.g. PR review assist, or
  spec-to-tests). Establish baselines FIRST. Provide prompt/tool starter kits. Weekly office hours.
  Exit criteria: baseline captured, pilot workflow live, no security incident.
- Days 31-60 (Walk): add a second workflow, introduce the adversarial-review gate, start a shared
  prompt/pattern library, begin light metrics reporting. Exit: measurable movement on one metric,
  two workflows adopted by a majority of the pilot team.
- Days 61-90 (Run): template the rollout for a second team, document guardrails as standards, hand
  the pilot team a self-serve runbook. Exit: a repeatable rollout package + a go/no-go
  recommendation with data.

### 3. `02-pilot-proposal-template.md`
Fill-in template: team, workflow chosen and why, baseline metrics, target delta, timebox, guardrails
in force, success/kill criteria, and who reviews the outcome. The discipline this enforces: pick ONE
thing, measure before and after, and pre-commit to what success and failure look like.

### 4. `03-metrics-framework.md`
The generic version of a delivery-quality metric set (this is where your instinct for measurable
engineering quality lives, described generically, with NO real numbers from any employer):
- Rework cycles per change (review round-trips before merge). Lower is better.
- Defect density (defects attributable to a change / units of change). Track escaped-to-production
  separately from caught-in-review.
- Cycle time (idea/ticket to merged/deployed).
- Review throughput and latency (PRs reviewed, time-to-first-review).
How to instrument cheaply: pull from the git host / PR API and the issue tracker; a weekly export
and a spreadsheet is enough for a pilot - do not turn measurement into its own project. Always show
baseline vs post, same team, same window length. Call out confounders honestly.

### 5. `04-guardrails.md`
- Adversarial-review gate: AI-authored or AI-assisted changes get a verification pass (human or a
  second model instructed to refute) before merge. Generic statement of the generate-then-refute
  principle; the runnable version is the sibling `agentic-code-review` project.
- Secure-coding review: AI output is reviewed for the usual injection/authz/secret-handling classes;
  never assume the model got security right.
- Human-in-the-loop: who is accountable for merged AI-assisted code (the human author, always).
- Data and IP boundaries: what must never be pasted into a prompt (secrets, customer PII, proprietary
  data on non-approved endpoints), and an approved-tools list. This section is the one that keeps
  enablement from becoming a security incident.

### 6. `05-change-management.md`
How adoption actually spreads: start with willing early adopters, make wins visible, remove friction
(keys, access, starter prompts), address the two real objections (job fear and quality distrust)
directly, and never mandate before demonstrating. Short and candid.

### 7. `06-starter-prompts/` - a small, generic library
Reusable, vendor-neutral prompt templates for common engineering tasks: PR review, spec-to-tests,
commit-message drafting, RCA decomposition, refactor proposals. Each is a `.md` with placeholders and
a note on how to adapt. Clean-room and generic; no employer workflow specifics.

## Format and principles
- All Markdown. Plain ASCII. Illustrative placeholders only - never a real employer metric value.
- Each doc stands alone and is adaptable in under an hour to a new org.
- Portfolio use: the kit can be shown (sanitized) as evidence of an enablement playbook. The pitch
  and 90-day plan are the strongest single artifacts for the AI Enablement Lead / TPM-AI interview.

## Milestones
- M1: `00-exec-pitch.md` + `01-90-day-plan.md` (the interview centerpiece).
- M2: `02-pilot-proposal-template.md` + `03-metrics-framework.md`.
- M3: `04-guardrails.md` + `05-change-management.md`.
- M4: `06-starter-prompts/` library + a top-level README tying them into a narrative.

## Companion article (feeds LinkedIn Featured / Phase 2 of the war plan)
Once M1-M3 exist, distill them into one public article: "How to introduce agentic workflows to an
engineering team without a security incident." That article is the public face of this kit.
