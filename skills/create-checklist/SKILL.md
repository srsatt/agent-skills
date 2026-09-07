---
name: create-checklist
description: Generate structured QA checklists from specifications, issue descriptions, research reports, diffs, or verbal feature descriptions. Use when users ask what to test or request smoke, functional, exploratory, acceptance, or security checklists with trackable results.
---

# Create Checklist

Turn available feature evidence into goal-specific QA checklists. Keep checks
observable, traceable to provided information, and usable without implementation
knowledge.

## Establish scope

Identify requested goal types:

- `smoke`: critical happy paths;
- `functional`: described behavior, boundaries, failures, and exploration;
- `acceptance`: one check per explicit criterion or requirement;
- `security`: feature-specific authentication, authorization, validation, and
  exposure risks.

If goal choice materially changes the result and none was provided, ask which
goals to generate. Multiple goals produce separate checklists.

Gather evidence from the conversation and any sources the user supplied or
authorized: specifications, issue trackers, research reports, repository files,
diffs, or feature descriptions. Use available connectors when appropriate, but
do not require a particular tracker or tool.

Do not invent missing behavior. Record uncertainty when it affects test design;
otherwise omit unsupported checks.

## Analyze the feature

Build a short working analysis covering:

- feature purpose and users;
- functional areas and observable behavior;
- acceptance criteria and requirements;
- user flows;
- data, roles, and permission boundaries;
- edge cases, constraints, and error states;
- relevant configuration or feature-flag states;
- unresolved inconsistencies.

Keep this analysis in context unless the user requests it as an artifact.

## Generate each checklist

1. Read [goal definitions](references/goal-definitions.md) for selected goals.
2. Follow [checklist template](references/checklist-template.md).
3. Create one independently usable checklist per goal.
4. Start every item with an action and state an observable expected result.
5. Keep one verification per item and number items sequentially.
6. Describe what to verify, not how the implementation works.
7. Remove duplicates, contradictions, unsupported expectations, and overlapping
   cases that add no coverage.
8. Set initial status to not tested and calculate summary counts.

When files are requested, write them under `docs/checklists/` as
`YYYY-MM-DD-{feature-slug}-{goal}.md`. Otherwise return the checklist directly.

Report produced goals, item counts, file paths when applicable, and uncertainties
that testers must resolve. Do not post results to external systems unless the
user explicitly requests that mutation.
