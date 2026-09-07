# Agent Skills

Reusable agent skills for high-quality interfaces.

## Included skills

- `avoid-ui-neuroslop` removes visible text that only narrates the interface or
  leaks implementation requirements.
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

`ui-max` also requires two upstream skills:

```sh
npx skills add GoogleChrome/modern-web-guidance --skill modern-web-guidance
npx skills add vercel-labs/agent-skills --skill web-design-guidelines
```

## Reproducible dependency list

`skills-lock.json` is the skills CLI equivalent of a package lock for the
upstream skills used by this repository. From a checkout, restore them with:

```sh
npx skills experimental_install --agent codex --yes
```

The repository's own public skills remain discoverable under `skills/`.
