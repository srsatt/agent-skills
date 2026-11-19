---
name: meta-verify
description: Orchestrate acceptance-criteria verification of a change: independent verifier, running the real app, end-to-end tests, failure diagnosis, and test-quality checks, ending in a pass/fail table with evidence. Use when the user invokes meta-verify or asks to verify, prove, or acceptance-check that a change works.
---

# Meta verify

Use the installed skills as one ordered pass that decides whether a change
meets its requirement. Keep their scopes intact; do not replace their
instructions with a summary. If a skill below is not installed or does not apply
to this repository, say so and continue with the others.

## Independence

An author who checks their own work grades it leniently. Run this workflow in a
fresh context:

- If you wrote or directed the change in this session, delegate to the
  `verifier` role instead of verifying inline.
- The verifier receives only the requirement, the diff or its base, and how to
  start the app. Do not pass the author's summary, rationale, or conclusions.
- The verifier reports failures and never fixes them. Fixing is a separate step
  after the table is delivered.

## Workflow

1. Write the acceptance criteria as a numbered list from the stated requirement
   alone: each one observable and checkable with a command or a visible result.
   If the requirement is too vague to produce checkable criteria, ask for it
   instead of inventing them.
2. If the change has a runnable surface, load `$run` and start the real app or
   CLI. Exercise each criterion through that surface and record what you saw.
3. If the change touches pages or flows and the repository has an end-to-end
   skill such as `$e2e-test-automator`, load it to run or add the end-to-end
   test that covers the criterion.
4. When a criterion fails, load `$diagnose` and find the cause before reporting.
   Report the cause; do not repair it.
5. If tests were added or changed as evidence, load `$testing-anti-patterns` and
   discard evidence from tautological or mock-only tests.
6. Deliver one table: criterion, status (`pass`, `fail`, or `not checked`), and
   the evidence for it (command and output, or what was seen). A criterion with
   no evidence is `not checked`, never `pass`.
7. Before finishing, re-run the cheapest check of every `pass` row against the
   final state of the change.

For verification-only requests, do not change files unless the user also asks
for fixes. Report conflicts between a skill, project rules, and the request
instead of silently choosing one.
