---
name: avoid-ui-neuroslop
description: Remove purposeless UI copy, decoration, and false status signals. Use when designing, implementing, or reviewing interfaces, visual styling, labels, badges, empty states, or error copy.
---

# Avoid UI Neuroslop

Do not leak prompts, specifications, acceptance criteria, or implementation
reasoning into visible UI. Product requirements are not automatically
user-facing information.

For every visible string, ask:

> Would this still make sense and help someone who has never seen the prompt,
> specification, or implementation notes?

If not, remove it.

Apply the same test to visual elements: what does each color, effect, icon,
badge, or card help the user understand or do?

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

## Make visual choices earn their place

Avoid defaulting to gradients (especially purple), unrelated accent colors,
glowing or pulsing badges, pill-shaped cards, glass effects, and decorative
emoji to make an interface feel finished. Use color, shape, and motion
consistently to show hierarchy, grouping, interaction, or real state. If
removing an effect changes none of those, remove it. These styles are not
forbidden when they serve a clear product purpose.

Choose type for the product and its content instead of reflexively using Inter
or a programming font. Do not sprinkle `//`, terminal cues, or code styling
around unrelated text to make it look technical. Check that icons, SVGs, ASCII
art, text, and controls align with their surrounding content at actual screen
sizes; a polished effect does not excuse sloppy layout.

Treat status labels such as `Active` and `Verified` as claims about the system.
Show them only when backed by a real state or check, when a different state is
possible, and when users benefit from seeing the distinction. Never animate a
status merely to suggest activity. Keep useful loading and progress feedback.

Replace generic slogans, hype words, and automatic grey subtitles with concrete
information users can act on. A functional screen does not need landing-page
copy or a personalized welcome by default. Do not put words from a design brief,
chat prompt, or implementation detail on screen just because they appeared in
the brief.

## Audit before finishing

Inspect every visible string and decorative or status element individually and ask:

1. What user need does this serve?
2. Would someone who never saw the prompt understand why it is here?
3. Does the interface already convey the same information?
4. Does this expose a requirement or reasoning from the specification?
5. What breaks if this string is deleted?

For a badge or status, also ask what produces it and whether it can change. If
deleting an element breaks nothing for the user, delete it.

## Source

- [10 tells of a slop ui](https://hereticpleb.vercel.app/blog/10-tells-of-slop)
