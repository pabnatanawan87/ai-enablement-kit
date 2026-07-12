# Prompt: Spec to Tests

## Purpose
Turn a spec or set of acceptance criteria into a concrete test list before any implementation. The
point is to surface ambiguity and edge cases early, while they are cheap to fix. Test cases first;
test code second and only after you agree with the list.

## When to use
- At the start of a change, to pin down what "done" means.
- When a spec feels vague and you want the gaps made visible.
- To seed test-driven work: agree the list, then generate the code.

## Prompt

```
You are turning a specification into a test plan. First list the cases; do not write test code yet.

Specification / acceptance criteria:
{{SPEC}}

Context
- Unit under test: {{UNIT}}
- Language and test framework: {{STACK_AND_FRAMEWORK}}
- Existing test conventions to match: {{TEST_CONVENTIONS}}

Do the following:

1. Restate the intended behavior in your own words in 2 to 4 bullet points. If the spec is
   ambiguous or contradictory, list the specific open questions before anything else and stop for
   those you cannot resolve from the spec alone.
2. Produce a table of test cases with columns: ID, category, input / precondition, action,
   expected result. Cover:
   - happy path(s)
   - boundary values (empty, zero, min, max, off-by-one)
   - invalid input and error handling
   - state and idempotency (repeat calls, ordering) where relevant
   - security-relevant inputs (injection, oversized, unauthorized) where relevant
3. Mark any case whose expected result is not determined by the spec as ASSUMPTION and state the
   assumption explicitly.
4. Call out behavior the spec does not mention but a real implementation must decide.

Rules
- Do not invent requirements. If the spec is silent, mark it as an assumption or an open question.
- Prefer many small, independent cases over a few broad ones.
- Only after I confirm the list, generate the test code in {{STACK_AND_FRAMEWORK}}, one test per
  case, using the IDs from the table.
```

## Placeholders
- `{{SPEC}}` - the requirement text, ticket description, or acceptance criteria.
- `{{UNIT}}` - the function, endpoint, component, or module under test.
- `{{STACK_AND_FRAMEWORK}}` - language plus test framework (for example, "Python with pytest").
- `{{TEST_CONVENTIONS}}` - naming, structure, fixtures, or assertion style to match.

## How to adapt
- Keep step 1 (restate and list open questions). Most bad tests come from an unstated assumption;
  this step drags it into the open.
- If your team writes given/when/then or table-driven tests, say so in `{{TEST_CONVENTIONS}}` and the
  generated code will follow.
- Review the case list yourself before generating code. The list is the artifact with real value; a
  wrong list produces confidently wrong tests.
- For integration or end-to-end specs, add environment and data-setup constraints to the context so
  the model does not assume a clean in-memory unit.
