---
name: meta-test
description: Orchestrate writing tests for a change: test-first red-green-refactor, end-to-end coverage for flows, test-quality checks, and a changed-code risk scan. Use when the user invokes meta-test or asks for maximum-quality tests for a feature, bug fix, or diff.
---

# Meta test

Use the installed skills as one ordered pass that produces tests proving a
change works. Keep their scopes intact; do not replace their instructions with
a summary. If a skill below is not installed or does not apply to this
repository, say so and continue with the others.

## Workflow

1. Fix the scope: the exact behavior the tests must pin, the requirement it
   comes from, and the repository's existing test commands and conventions.
2. Load and apply `$tdd` for new behavior and bug fixes: write the failing test
   first, make it pass, then refactor. For a bug, the first test reproduces it.
3. If the change touches pages or flows and the repository has an end-to-end
   skill such as `$e2e-test-automator`, load it to cover the flow end to end.
4. Before and after writing mocks or test-only helpers, load
   `$testing-anti-patterns` and check each new test for tautology, mock-only
   assertions, and production pollution.
5. Load `$fallow` for a changed-code scan and report which changed lines have
   no test, which test is a duplicate of another, and which new code is unused.
6. Run the full test command of the repository and show the output. For each
   new test, show that it fails without the change, or say that you could not
   check it.
7. Before finishing, re-run the new tests against the final state.

For test-writing requests, do not change production code beyond what `$tdd`
requires for the red-green step. Report conflicts between a skill, project
rules, and the request instead of silently choosing one.
