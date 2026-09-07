---
name: ui-design-foundations
description: Apply source-grounded interaction heuristics, readable content layout, predictable keyboard and focus behavior, and clear visual hierarchy beyond code-level web checks. Use when designing or reviewing UI flows, navigation, layouts, focus behavior, error handling, or visual organization.
---

# UI Design Foundations

Complement code-level accessibility and performance checks with interaction and
visual-design judgment. Prefer project requirements and applicable WCAG success
criteria over heuristics; use familiar platform conventions before inventing a
new interaction.

## Design the interaction model

- Keep users informed about meaningful state, progress, and action outcomes
  without narrating obvious stable UI.
- Use users' language and real-world task order. Keep the same concept, control,
  and action behaving consistently throughout the product.
- Preserve control: provide clear exits, Back or Cancel where expected, and Undo
  for reversible work. Confirm only consequential actions that cannot be safely
  undone.
- Prevent errors with constraints, useful defaults, previews, and validation
  before relying on error messages.
- Prefer recognition over recall: keep choices, context, constraints, and
  recently used information visible at the point of use.
- Support novice discovery and expert efficiency without creating two different
  interaction models.
- When errors occur, preserve user input, identify the affected item, explain
  the corrective action, and restore a usable state.
- Remove irrelevant information and controls. Add help only where the task
  cannot be made self-explanatory.

## Constrain and organize layout

- Start prose at roughly `max-width: 75ch`; treat this as a readable cap, not a
  rigid target. Let tables, code, media, and task-specific workspaces use wider
  layouts deliberately.
- Use a small, consistent spacing scale. Align related content, group by
  proximity, and use whitespace to separate concepts rather than decoration.
- Make the primary task and primary action visually dominant. Express hierarchy
  with position, size, weight, spacing, and alignment; avoid competing accents.
- Preserve reading order and hierarchy at narrow and wide viewports. Do not let
  a large screen turn prose into a full-width column.
- Reveal secondary complexity progressively while keeping current context and a
  clear route back.

## Make keyboard and focus behavior predictable

- Start with native elements and established WAI-ARIA APG interaction patterns.
  Do not invent keyboard behavior for a familiar component.
- Keep DOM, reading, and focus order logical. Avoid positive `tabindex` and CSS
  reordering that makes keyboard movement diverge from the visual layout.
- Use `Tab` to move between components and APG-prescribed keys within composite
  widgets. Ensure every pointer action has a keyboard path.
- On opening an overlay, move focus to its meaningful initial target. Trap focus
  only in modal contexts. On close, return focus to the trigger or a logical
  successor if that trigger no longer exists.
- Keep focus visible, distinguishable, and unobscured. Test forward and backward
  keyboard movement, dismissal, recovery after validation, and focus after
  dynamic updates.

## Sources

- [Nielsen Norman Group: 10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)
- [GOV.UK Design System: Layout](https://design-system.service.gov.uk/styles/layout/)
- [WAI-ARIA APG: Developing a Keyboard Interface](https://www.w3.org/WAI/ARIA/apg/practices/keyboard-interface/)
- [WCAG 2.2](https://www.w3.org/TR/WCAG22/)
- [Apple Human Interface Guidelines: Layout](https://developer.apple.com/design/human-interface-guidelines/layout)
