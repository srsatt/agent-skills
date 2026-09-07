---
name: avoid-ui-neuroslop
description: Prevent specification-leaking, redundant, or narrational UI text. Use when designing, implementing, or reviewing interfaces, frontend components, labels, helper text, empty states, badges, captions, notices, or error copy.
---

# Avoid UI Neuroslop

Do not leak prompts, specifications, acceptance criteria, or implementation
reasoning into visible UI. Product requirements are not automatically
user-facing information.

For every visible string, ask:

> Would this still make sense and help someone who has never seen the prompt,
> specification, or implementation notes?

If not, remove it.

## Make every string serve a user need

Keep text only when it helps users:

1. understand something not already obvious from the interface;
2. make a decision;
3. perform an action;
4. recover from an error or unexpected state.

Do not add text to fill space, narrate visible UI, restate headings or controls,
confirm that a requirement was implemented, or explain why something was
omitted. Prefer deleting a string over shortening it. Prefer a self-explanatory
interface over helper text.

## Represent absence with absence

Treat requirements such as `do not show X`, `X is unavailable`, `X is
unsupported`, `this screen has no X`, or `omit X in this state` as instructions
to omit X silently. Do not turn them into copy such as `No X`, `X is
unavailable`, `X is disabled for this item`, or `This view does not contain X`.

Mention an absence only when users need it to make a decision, understand an
unexpected state, take an action, or recover from an error.

## Use empty states only when needed

Do not create an empty-state message merely because content is absent. An empty
area may remain empty. Add an empty state only when users could reasonably
interpret the absence as loading, an error, a filtering problem, missing
configuration, or a situation requiring action. State only what they need to
know or do.

## Audit before finishing

Inspect every visible string individually and ask:

1. What user need does this serve?
2. Would someone who never saw the prompt understand why it is here?
3. Does the interface already convey the same information?
4. Does this expose a requirement or reasoning from the specification?
5. What breaks if this string is deleted?

If deleting it breaks nothing for the user, delete it.
