---
name: review-max
description: Orchestrate graph-grounded review, test-quality review, comment review, and UI copy review for a comprehensive pass over a diff or pull request. Use when the user invokes review-max or asks for a maximum-quality review of a change.
---

# Review Max

Use the installed review skills as one ordered pass over the same diff. Keep
their scopes intact; do not replace their instructions with a summary. If a
skill below is not installed, say so and continue with the others.

## Workflow

1. Fix the scope: the exact diff or pull request, and its base. Review only
   changed lines and the code they affect.
2. Load and apply `$fallow-review` first. Use its ranking to decide where to spend
   attention, and answer its judgment questions about new public contracts,
   boundary crossings, and dependencies.
3. If the diff touches tests, load `$testing-anti-patterns` and check each
   changed test for tautology, mock-only assertions, and production pollution.
4. If the diff adds or changes comments, load `$no-comments` and check them.
5. If the diff changes visible UI text or empty states, load `$avoid-ui-neuroslop`
   and check every changed string.
6. Merge the findings into one list ordered by severity. Give each finding a
   `file:line`, the failure scenario, and which skill raised it. Drop duplicates.
7. Before finishing, re-check that every finding still applies to the final diff.

For review-only requests, do not change files unless the user also asks for
fixes. Report conflicts between a skill, project rules, and the request instead
of silently choosing one.
