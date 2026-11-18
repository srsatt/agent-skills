---
name: metaskill-creator
description: Create a `<domain>-max` meta-skill that orchestrates several installed skills into one ordered workflow, modeled on ui-max. Use when the user asks to create, extend, or review a meta-skill, a `*-max` skill, or a skill that chains other skills.
---

# Metaskill creator

A meta-skill adds order and gates to skills that already exist. It never copies
their instructions: copies drift and conflict with the originals.

## 1. Pick the members

1. State the domain and the outcome in one sentence.
2. List installed skills that already cover a step of that outcome. Read each
   `SKILL.md` description, not just the name.
3. Drop a skill that overlaps another member's trigger. Keep the one with the
   narrower, clearer trigger.
4. Keep three to five members. More than five means two domains: split them.
5. Note which members are bound to which harness and profile. A member absent
   from a target is skipped there, so the workflow must still work without it.

## 2. Write the workflow

Create `agent-skills/skills/<domain>-max/SKILL.md` with:

- Frontmatter `name` and a `description` that names the members' domain and the
  trigger words ("invokes `<domain>-max` or asks for maximum-quality ...").
- One short intro: members keep their own scope, and their instructions are not
  replaced by a summary.
- A numbered workflow. Each step names exactly one member as `$skill-name`, says
  when it runs (before writing, after changes, before finishing), and says what
  it must produce.
- A gate step near the end: re-run the cheapest checks of earlier steps on the
  final result.
- A review-only clause: no file changes unless the user also asks for them.
- A conflict clause: report disagreements between member guidance, project
  rules, and the request instead of silently choosing.
- A missing-member clause: if a member is not installed, say so and continue
  with the others.

Do not add rules of your own that no member contains. A new rule belongs in a
member skill or in the policy, not in the orchestrator.

## 3. Bind it

Add the skill to the catalog with a `bind` that matches its members' targets.
Regenerate, run the config check, and read the emitted matrix to confirm the
skill and each member reach the same targets.

## 4. Test it

Replay two real tasks in the domain, one with the meta-skill and one without.
Keep the meta-skill only if it finds something the plain run missed or it
prevents a step from being skipped. Otherwise delete it.
