# Prompt: Commit Message Drafting

## Purpose
Draft a clear, honest commit message from a staged diff. A good message says what changed and why,
so the next reader (often you, in six months) does not have to reverse-engineer intent from code.

## When to use
- Any non-trivial commit where the "why" is not obvious from the diff.
- To keep history readable and greppable across a team.
- As a quick summary you edit, not accept blindly.

## Prompt

```
Write a commit message for the staged change below. Describe what changed and why. Do not oversell
it and do not describe changes that are not in the diff.

Context
- Why this change was made (the problem or goal): {{WHY}}
- Related issue or ticket reference, if any: {{ISSUE_REF}}
- Commit message format the repo uses: {{FORMAT}}

The staged diff:
{{DIFF}}

Produce:
1. A subject line: imperative mood, {{SUBJECT_LIMIT}} characters or fewer, no trailing period.
2. A blank line, then a body that explains the motivation and the approach, wrapped at 72
   characters. Focus on why over what; the diff already shows the what.
3. If the change is a breaking change, a "BREAKING CHANGE:" footer describing the impact and the
   migration path.
4. The issue reference in a footer if one was provided.

Rules
- Base the message only on the diff and the context given. If the "why" is unclear from what I
  gave you, ask for it rather than guessing.
- No filler ("various fixes", "misc changes", "update code"). Be specific.
- Do not claim tests were added, bugs fixed, or performance improved unless the diff shows it.
```

## Placeholders
- `{{WHY}}` - the motivation for the change in one or two sentences.
- `{{ISSUE_REF}}` - ticket or issue id (optional; delete the line if none).
- `{{FORMAT}}` - your convention, for example Conventional Commits (`type(scope): subject`) or plain.
- `{{SUBJECT_LIMIT}}` - subject length cap your team uses (50 and 72 are both common).
- `{{DIFF}}` - the staged diff.

## How to adapt
- If you use Conventional Commits, put the allowed types (feat, fix, docs, refactor, test, chore) in
  `{{FORMAT}}` so the model picks a valid one.
- Provide the "why" yourself. A model can summarize a diff, but it cannot know the motivation unless
  you tell it; the most valuable line in a commit message is the one it cannot infer.
- Treat the output as a draft. Read it against the diff and cut any claim the diff does not support.
- To automate, this prompt can be wired into a commit-message hook, but keep the human edit step;
  do not auto-commit generated text.
