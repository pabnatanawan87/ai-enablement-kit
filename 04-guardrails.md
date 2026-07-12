# Guardrails

Enablement without guardrails is how a productivity program becomes a security incident. These four
guardrails are the non-negotiable part of the kit. Adopt them before you scale a single workflow to a
second team. They are deliberately generic so any org can adapt them in under an hour, but the
data/IP-boundary section is not up for softening.

The principle behind all of them: an AI-assisted change is a draft until a human has verified it. The
model is a fast, tireless, confidently-wrong contributor. Treat its output the way you would treat a
pull request from a new hire who has never seen your threat model - useful, worth reviewing, never
merged on trust.

---

## 1. Adversarial-review gate (generate-then-refute)

Any change that was authored or materially assisted by a model gets a second pass whose explicit job
is to refute it, not to approve it. Generation and verification are separate steps with separate
incentives. The generator is rewarded for producing a plausible change; the reviewer is rewarded for
finding the reason it is wrong.

The refutation pass asks, concretely:

- What input makes this break? Name the edge cases the author did not handle.
- What does this change assume about the rest of the system, and is that assumption true here?
- Does this introduce a security, correctness, or data-loss regression?
- Is there a test that would have caught the bug this change might contain? If not, that is the gap.

The reviewer can be a human or a second model instructed to argue against the change - the two are
not equivalent in authority (see guardrail 3), but both are better than a single-pass generate-and-
merge. Running the same model that wrote the code and asking "is this correct?" does not count; it
will agree with itself. The reviewer must be a distinct pass with a refuting prompt, and ideally a
distinct model or a human.

This is a generic statement of the principle. The runnable version - a two-role generate-then-refute
loop over a real diff - is the sibling `agentic-code-review` project. Point your pilot team at that
project for the automated gate; use the description here to write the policy.

Gate rule: no AI-assisted change merges without a completed refutation pass recorded on the PR. "I
read it and it looked fine" is not a refutation pass.

---

## 2. Secure-coding review

Never assume the model got security right. Models reproduce the average of their training data, and
the average code on the internet is not secure. AI output gets the same security review your most
junior contributor's output would get, at minimum, and specifically for these classes:

- Injection - SQL, command, template, LDAP, and prompt injection in anything that builds a query or a
  shell invocation from input.
- Authorization and authentication - missing or wrong access checks, privilege escalation, tenant or
  scope confusion, endpoints that trust a client-supplied identity.
- Secret handling - hard-coded credentials, secrets in logs, tokens in error messages, keys checked
  into the change.
- Input validation and output encoding - unvalidated input, missing encoding on output, unsafe
  deserialization.
- Dependency and supply chain - a model that invents a plausible package name (a hallucinated
  dependency an attacker can then register and squat) or pulls in an unvetted library.

Two failure modes are specific to AI output and worth calling out. First, models produce code that
looks idiomatic and reads as if it were written by someone who understood the security implications,
which lowers a reviewer's guard - review the behavior, not the polish. Second, models hallucinate
dependencies; verify that every imported package actually exists and is the one you meant, before it
is installed.

If your org has a secure-coding standard or a static-analysis gate already, AI-assisted changes run
through the same gate. They do not get a fast lane.

---

## 3. Human-in-the-loop accountability

A human is always the author of record. The model is a tool the human used, the way a compiler or an
IDE is a tool. Accountability does not transfer to the tool.

Concretely:

- The human who opens the PR owns the change. "The AI wrote it" is not a defense for a defect, a
  breach, or a licensing problem. If you would not sign your name to it, do not submit it.
- The author must understand the change well enough to explain it and to defend it in the adversarial
  review. Merging code you cannot explain is prohibited regardless of who or what wrote it.
- Approval authority stays with humans. A second model can flag issues in the adversarial pass, but a
  human makes the merge decision. Do not build a pipeline where a model approves and merges another
  model's code with no human in the loop.
- Attribution is honest. If a change was AI-assisted, that is fine and often expected; note it where
  your process notes such things so the metrics and the review load are visible. Hiding AI assistance
  defeats the measurement in `03-metrics-framework.md`.

The short version: the model drafts, the human decides, and the human's name is on it.

---

## 4. Data and IP boundaries

This is the section that keeps enablement from becoming a security incident. It is firm on purpose.
Do not weaken it to reduce friction; reduce friction elsewhere.

### What must never go into a prompt

The following are never pasted into, uploaded to, or referenced in a prompt sent to any model on any
endpoint that is not on the approved-tools list below:

- Secrets and credentials - API keys, tokens, passwords, private keys, connection strings, session
  cookies, anything that grants access.
- Customer or personal data - PII, PHI, payment data, or any customer content covered by a contract
  or regulation. If it would trigger a breach notification when leaked, it does not go in a prompt.
- Proprietary source and business data - source code, schemas, internal documents, financials, or
  roadmap material, except on an endpoint explicitly approved for that data class (see below).
- Third-party confidential material - anything you hold under an NDA or a license that forbids
  disclosure to a subprocessor.

The test is simple: assume anything you send to a non-approved endpoint may be retained, logged,
used for training, and read by a stranger. If that outcome is unacceptable for the data, the data
does not go in the prompt. When in doubt, leave it out and ask.

### Approved-tools list

Only tools on an explicit approved list may be used with any non-public data, and each tool is
approved for a specific data class - approval is not blanket. The list is maintained by [OWNER, e.g.
security + engineering leadership] and every entry records:

| Field | What it captures |
|-------|------------------|
| Tool / endpoint | The specific product and deployment (for example, a vendor API vs. a self-hosted model). |
| Approved data class | Public only / internal source / regulated data. Never assume higher than stated. |
| Data-retention terms | Whether prompts are retained, for how long, and whether they are used for training. Zero-retention or no-training terms must be in writing. |
| Tenancy | Shared vs. dedicated vs. self-hosted. |
| Owner and review date | Who approved it and when it is next reviewed. |

Rules for the list:

- A tool not on the list is treated as public - it may be used for public information and generic
  code that contains no proprietary or customer data, and nothing else.
- Adding a tool requires a review of its data-retention and training terms in writing before first
  use with non-public data. A marketing claim is not written terms.
- Personal accounts and consumer tiers of AI products are not approved for any non-public data unless
  explicitly listed with acceptable terms. The free tier of a chat product is a public endpoint.
- The list is reviewed on a schedule because vendor terms change. An approval is time-boxed, not
  permanent.

### If a boundary is crossed

Treat a confirmed paste of secrets or regulated data into a non-approved endpoint as a security
incident and run your existing incident process - rotate the exposed secrets, notify the owner, and
record it. Make reporting safe and fast: an engineer who self-reports a mistake early is protecting
the org, and the process should reflect that. The goal is containment, not blame; a punitive response
just drives the next mistake underground.

---

## How these fit the rollout

- The adversarial gate and secure-coding review are the two workflows that get introduced in the
  Walk phase of `01-90-day-plan.md`, once baselines exist.
- The data/IP boundaries and approved-tools list are prerequisites for Day 0 - they are in force
  before the first prompt, not added later.
- Every pilot proposal (`02-pilot-proposal-template.md`) states which guardrails are in force for
  that pilot. "All four" is the expected answer.
