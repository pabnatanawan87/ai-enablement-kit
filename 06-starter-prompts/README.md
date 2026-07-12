# Starter Prompts

A small, vendor-neutral library of prompt templates for common engineering tasks. Each file is a
self-contained prompt you can paste into any capable model (hosted or local) through your team's
approved tool. Nothing here is tied to a specific vendor, IDE, or internal workflow.

## What is here

| File | Task | Use it when |
|------|------|-------------|
| `pr-review.md` | PR / diff review | You want a structured second read of a change before merge. |
| `spec-to-tests.md` | Spec to tests | You have a spec or acceptance criteria and want a test list first. |
| `commit-message.md` | Commit message drafting | You have a staged diff and want a clear conventional message. |
| `rca-decomposition.md` | Root-cause analysis | An incident or recurring defect needs to be broken down. |
| `refactor-proposals.md` | Refactor proposals | A file or module smells and you want options, not a rewrite. |

## Conventions used by every template

- Placeholders look like `{{THIS}}`. Replace every one before sending. If a placeholder does not
  apply, delete the line rather than leaving it blank.
- Each file has the same shape: Purpose, When to use, Prompt, Placeholders, How to adapt.
- The prompts ask the model to show its reasoning and to flag uncertainty. That is deliberate: an
  answer you cannot audit is not useful on a code change.
- Output is advisory. A human author stays accountable for anything merged. See `../04-guardrails.md`.

## How to adapt (applies to all of them)

1. Read the "How to adapt" note at the bottom of each file first.
2. Swap in your stack, your review standards, and your definitions of done.
3. Keep the "refute / find what is wrong" framing. The value of these prompts is that they push the
   model to challenge the work, not to praise it.
4. If a prompt gets long, split the fixed instructions (role, rules) from the per-task inputs so you
   can reuse the fixed part as a saved prompt or system message.

## Data and IP boundary

Do not paste secrets, credentials, customer data, or proprietary code into a prompt on a
non-approved endpoint. Confirm the tool you are using is on the approved list before pasting any
real diff. See `../04-guardrails.md`.
