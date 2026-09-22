# Agent Skills

Reusable agent skills for product, engineering, and interface work.

## Included skills

- `avoid-ui-neuroslop` removes visible text that only narrates the interface or
  leaks implementation requirements.
- `create-checklist` generates smoke, functional, exploratory, acceptance, and
  security QA checklists from product evidence.
- `folio-report` creates durable standalone reports with repository evidence,
  media, charts, and review feedback.
- `ui-design-foundations` applies interaction heuristics, readable layout,
  predictable focus behavior, and clear visual hierarchy.
- `ui-max` combines both skills with modern web implementation guidance and a
  final interface audit.

## Install

Install every skill:

```sh
npx skills add srsatt/agent-skills
```

Or install one skill:

```sh
npx skills add srsatt/agent-skills --skill avoid-ui-neuroslop
```

## Upstream skills

This checkout pins upstream skills rather than republishing them:

```sh
npx skills add juliusbrussee/caveman --skill cavecrew caveman caveman-commit caveman-compress caveman-help caveman-review caveman-stats
npx skills add lirantal/gh-cp --skill nodejs-cli-best-practices
npx skills add GoogleChrome/modern-web-guidance --skill modern-web-guidance
npx skills add vercel-labs/agent-skills --skill web-design-guidelines
```

`ui-max` requires `modern-web-guidance` and `web-design-guidelines`.

### Benjamin-Plus

[JetBrains/benjamin-plus-skill](https://github.com/JetBrains/benjamin-plus-skill)
is intentionally prompt-injected rather than installed as a discoverable skill;
its benchmarks found that discovery erased the savings. Follow its upstream
Codex instructions and inject `injected-instruction.md` through `AGENTS.md`.

## Reproducible dependency list

`skills-lock.json` is the skills CLI equivalent of a package lock for installable
upstream skills used by this repository. From a checkout, restore them with:

```sh
npx skills experimental_install --agent codex --yes
```

The repository's own public skills remain discoverable under `skills/`.
