# Prompt: Refactor Proposals

## Purpose
Get options for improving a piece of code without changing what it does, along with the trade-offs
and risk of each. The output is a menu of proposals you choose from, not a rewrite the model applies
on its own. Behavior must be preserved; that is the whole contract of a refactor.

## When to use
- A file or module has grown hard to change and you want structured options.
- Before a larger change, to clean the ground first.
- To get a second opinion on how to decompose something complex.

## Prompt

```
Propose refactorings for the code below. A refactoring must preserve observable behavior. Give me
options with trade-offs; do not rewrite everything, and do not change functionality.

The code:
{{CODE}}

Context
- Language / stack: {{STACK}}
- Why this code is painful right now (what you are trying to make easier): {{PAIN_POINT}}
- Constraints (public API that must not change, performance limits, deadlines): {{CONSTRAINTS}}
- Test coverage that exists for this code: {{TEST_COVERAGE}}

Do the following:

1. Briefly describe what the code does now and the specific smells you see (long function,
   duplication, unclear naming, mixed concerns, deep nesting, hidden state). Cite lines.
2. Propose 2 to 4 distinct refactorings, ordered by value-to-risk. For each:
   - name and one-line description
   - what it improves and why it helps the pain point above
   - risk and blast radius (what could break, how far it reaches)
   - rough effort (small / medium / large)
   - whether existing tests cover the change or new tests are needed first
3. Recommend which to do first and which to skip, and say why.
4. For your top recommendation only, show a focused before/after of the key section. Keep behavior
   identical.

Rules
- Preserve behavior. If a proposal changes an observable behavior or a public contract, label it as
  a behavior change, not a refactor, and call out the risk.
- Do not propose broad rewrites. Prefer small, independently shippable steps.
- If the code lacks tests to make refactoring safe, say so and recommend adding characterization
  tests first.
- Flag anything you are unsure about rather than asserting it is safe.
```

## Placeholders
- `{{CODE}}` - the file, function, or module to consider.
- `{{STACK}}` - language, framework, runtime.
- `{{PAIN_POINT}}` - what you are actually trying to make easier (drives which options matter).
- `{{CONSTRAINTS}}` - APIs that cannot change, performance budgets, time limits.
- `{{TEST_COVERAGE}}` - what tests exist (or "none"), so the model can judge how safe changes are.

## How to adapt
- Be honest about `{{TEST_COVERAGE}}`. If there are no tests, the right first proposal is usually to
  add characterization tests, not to restructure blind.
- State the real `{{PAIN_POINT}}`. "This is ugly" is weak; "I cannot add a new payment type without
  editing five files" tells the model what to optimize for.
- Take one proposal at a time. Apply it, run the tests, commit, then reconsider. A batch of
  simultaneous refactors is hard to review and hard to revert.
- Keep the behavior-preservation rule strict. When a proposal crosses into changing behavior, treat
  it as a feature change with its own review and tests, not a cleanup.
