# Prompt: PR / Diff Review

## Purpose
Get a structured, skeptical second read of a change before it merges. The model is asked to find
what is wrong, not to approve. Its output is input to a human reviewer, never a replacement for one.

## When to use
- Before requesting human review, to catch the obvious issues yourself.
- As the second opinion in an adversarial-review gate (see `../04-guardrails.md`).
- On a change you did not write and need to understand quickly.

## Prompt

```
You are reviewing a code change. Your job is to find defects, risks, and unclear intent, not to
praise the work. Assume the change is guilty until the diff proves otherwise.

Context
- What the change is supposed to do: {{CHANGE_INTENT}}
- Language / stack: {{STACK}}
- Relevant standards or conventions this repo follows: {{CONVENTIONS}}
- Anything already known to be out of scope: {{OUT_OF_SCOPE}}

The diff:
{{DIFF}}

Review it in this order and use these headings:

1. Correctness. Logic errors, off-by-one, wrong conditionals, unhandled cases, incorrect
   assumptions about inputs or state. Name the exact line and describe the failing scenario.
2. Security. Injection, missing authz/authn checks, secret or credential handling, unsafe
   deserialization, unvalidated input crossing a trust boundary. Do not assume the author got
   security right.
3. Edge cases and error handling. Null/empty/large inputs, concurrency, partial failure, retries,
   timeouts, resource cleanup.
4. Tests. Are the changed paths covered? What test is missing that would have caught a regression?
5. Readability and maintainability. Naming, dead code, duplicated logic, misleading comments.
6. Scope. Anything in the diff that does not belong to the stated intent.

Rules
- Cite the specific line or hunk for every point.
- Rank findings: BLOCKER, SHOULD-FIX, NIT. Put blockers first.
- If you are unsure, say so and say what you would need to confirm it. Do not invent line numbers
  or behavior you cannot see in the diff.
- End with a one-line verdict: BLOCK, REQUEST CHANGES, or LOOKS REASONABLE (pending human review).
```

## Placeholders
- `{{CHANGE_INTENT}}` - one or two sentences on what the change should do.
- `{{STACK}}` - language, framework, runtime.
- `{{CONVENTIONS}}` - style guide, lint rules, architectural patterns the repo follows.
- `{{OUT_OF_SCOPE}}` - anything the reviewer should not flag (optional; delete if none).
- `{{DIFF}}` - the unified diff or the full changed files.

## How to adapt
- Paste your real review checklist into the numbered list so the model reviews against your bar, not
  a generic one.
- For large diffs, review file by file; models get less reliable as the diff grows. Split it.
- If your team gates on the generate-then-refute pattern, run this prompt with a different model or a
  fresh session than the one that wrote the code, so the reviewer is not defending its own work.
- Keep the "guilty until proven otherwise" framing. Dropping it turns the review into flattery.
