---
name: meta-test
description: Orchestrate writing tests for a change: choosing the test type (end-to-end, invariant and property, differential), test-first red-green-refactor, test-quality checks, and a changed-code risk scan. Use when the user invokes meta-test or asks for maximum-quality tests for a feature, bug fix, or diff.
---

# Meta test

Use the installed skills as one ordered pass that produces tests proving a
change works. Keep their scopes intact; do not replace their instructions with
a summary. If a skill below is not installed or does not apply to this
repository, say so and continue with the others.

## Choose the test type

Pick the type that is hardest to fool for this change:

1. **End-to-end first.** A test of a real user flow passes only if the whole
   feature works. A unit test can pass while the feature is broken, because the
   author and the test share the same misunderstanding of the task.
2. **Invariants and properties for logic.** State a fact that must never fail
   and let a generator (fast-check, Hypothesis, proptest) try to disprove it
   with many inputs. Generated edge cases do not share the author's bias.
   Look for these shapes:
   - an invariant, such as "a balance never goes negative";
   - a round trip, such as decode(encode(x)) equals x;
   - idempotency, such as running a migration, normalization, or retry twice
     gives the same result as running it once;
   - equivalence, such as the optimized code matching a slow, obviously correct
     version.
3. **Differential tests for rewrites and migrations.** Run the old and new code
   on the same inputs and treat every difference as a bug unless it is on a
   written list of approved differences. Normalize irrelevant noise such as
   whitespace.
4. **Plain unit tests** for the remaining pure code, once the above cannot
   express the behavior.

If the repository has a feature map (features from the user's view, with paths,
selectors, and expected results), use it as the list of what to cover.

## Workflow

1. Fix the scope: the exact behavior the tests must pin, the requirement it
   comes from, and the repository's existing test commands and conventions.
   Choose the test types above and state why.
2. Load and apply `$tdd` for new behavior and bug fixes: write the failing test
   first, make it pass, then refactor. For a bug, the first test reproduces it.
3. If the change touches pages or flows and the repository has an end-to-end
   skill such as `$e2e-test-automator`, load it to cover the flow end to end.
4. Before and after writing mocks or test-only helpers, load
   `$testing-anti-patterns` and check each new test for tautology, mock-only
   assertions, and production pollution.
5. Check each new test for the traits that keep an agent honest:
   - it fails with a message that says what is broken (the step, the value);
   - it is fast enough to run on every change, and deterministic;
   - it is hard to fool: either the whole behavior works or the test fails.
6. Load `$fallow` for a changed-code scan and report which changed lines have
   no test, which test is a duplicate of another, and which new code is unused.
7. Run the full test command of the repository and show the output. For each
   new test, show that it fails without the change, or say that you could not
   check it.
8. Before finishing, re-run the new tests against the final state.

## Do not weaken the check

- A change must not edit the test or check that judges it. Do not loosen an
  existing assertion, delete a failing test, or add a skip to make a run pass;
  report the failing test and why instead.
- A flaky test is never retried, padded with sleeps, or skipped. Find the root
  cause. If you cannot, delete the test and write down why.
- After three failed rounds on the same failure, stop and hand it to a human
  with the evidence so far.

## Keep the suite small

Heavier tests (end-to-end, generated, differential) belong before a commit or
pull request, and cheap checks (lint, build, fast tests) run first. A test that
no longer guards a behavior is removed, not kept: ask for `$test-audit` when
the user wants the suite pruned.

For test-writing requests, do not change production code beyond what `$tdd`
requires for the red-green step. Report conflicts between a skill, project
rules, and the request instead of silently choosing one.
