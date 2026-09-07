# Goal Type Definitions

Each goal type determines what to extract from the feature analysis, how to group items, how many items to target, and what tone to use.

---

## smoke

**Purpose**: Verify critical paths work end-to-end. A smoke checklist answers: "Can a user complete the core workflow without hitting a blocker?"

**Scope rules**:
- Include ONLY happy-path flows through the main user journeys
- Skip edge cases, error handling, and secondary features
- Target **10–15 items** maximum — if you have more, you're including too much
- One item per critical path step, not per micro-interaction

**What to draw from the feature analysis**:
- User flows → one check per critical path step
- Key acceptance criteria → only those that gate basic functionality
- Ignore: edge cases, constraints, permissions, feature flags

**Grouping**: By user flow / critical path (e.g. "Create workflow", "Edit workflow", "Delete workflow")

**Item tone**: "Verify that [core action] produces [expected result]"

**Example items**:
- "Verify that creating a new board displays it in the boards list"
- "Verify that adding a card to the board saves successfully"
- "Verify that the board loads without errors after page refresh"

---

## functional

**Purpose**: Comprehensive verification of all described behaviors plus exploratory testing areas. Covers the full functional surface including edge cases, error states, and boundary conditions.

**Scope rules**:
- No hard cap on item count — thoroughness over brevity
- Cover every described behavior, not just happy paths
- Include edge cases, error handling, boundary conditions
- Include feature flag states if any are mentioned
- Add an "Exploration Areas" section at the end

**What to draw from the feature analysis**:
- All functional areas → checks per behavior
- Acceptance criteria → one check per criterion
- Edge cases & constraints → checks for boundary conditions
- Feature flags → checks for behavior with flag on and off
- Data & permissions → checks for different input types and states

**Grouping**: By functional area / component (e.g. "Board Creation", "Card Management", "Permissions", "Edge Cases")

**Item tone**: "Verify that [specific action] in [specific context] results in [expected outcome]"

**Exploration Areas section**: After all structured checks, add a section with open-ended prompts:
- These are NOT pass/fail checks — they are areas to explore freely
- Format as bullet points, not table rows
- Tone: "Explore [area] — try [suggested approach] and note any unexpected behavior"
- Target 3–5 exploration prompts

**Example exploration prompts**:
- "Explore rapid switching between board views — try toggling list/board/timeline quickly and note any rendering glitches"
- "Explore behavior with very long card titles (200+ characters) across all views"

---

## acceptance

**Purpose**: Verify that the implementation satisfies each stated requirement. Every acceptance criterion or requirement maps to exactly one checklist item.

**Scope rules**:
- One check per acceptance criterion or explicit requirement
- Item count matches the number of identified criteria — no more, no less
- Do not add edge cases or exploratory items — this is strictly spec-driven
- If the analysis found no acceptance criteria, note this and derive checks from explicit requirements instead

**What to draw from the feature analysis**:
- Acceptance criteria → one check per criterion (primary source)
- Requirements → one check per requirement (secondary, only if no criteria exist)
- Ignore: edge cases, constraints, feature flags, permissions (unless they are explicit acceptance criteria)

**Grouping**: By requirement area (e.g. "Data Management Requirements", "UI Requirements", "Integration Requirements")

**Item tone**: "Confirm that [requirement statement from spec]"

**Example items**:
- "Confirm that the board supports at least 100 cards without performance degradation"
- "Confirm that users with Reporter role cannot delete cards"

---

## security

**Purpose**: Verify authentication, authorization, input validation, and data exposure specific to the feature's attack surface.

**Scope rules**:
- Scale to the feature's security surface — a read-only view gets fewer items than an admin panel with form inputs
- Include ONLY categories relevant to the feature — skip irrelevant ones entirely
- Do not include generic security checks unrelated to the feature

**Security categories** (include only those that apply):

| Category | When to include | What to check |
|---|---|---|
| **Authentication** | Feature has login-gated content | Session handling, unauthenticated access attempts |
| **Authorization** | Feature has role-based access | Permission boundaries, role escalation, accessing other users' data |
| **Input Validation** | Feature accepts user input (forms, fields, URLs) | XSS, injection, malformed input, oversized input |
| **Data Exposure** | Feature displays or exports data | Sensitive data in responses, API leaks, excessive data in error messages |
| **CSRF / State Mutation** | Feature has forms or state-changing actions | Token validation, action replay |
| **File Handling** | Feature involves file upload/download | File type validation, path traversal, malicious content |

**What to draw from the feature analysis**:
- Data & permissions → authorization checks
- User flows → retraced with attacker mindset
- Edge cases → reframed as security boundary tests
- Functional areas → identify input points and data flows

**Grouping**: By security category (not by functional area)

**Item tone**: "Attempt to [attack vector] and verify that [security control] prevents it"

**Example items**:
- "Attempt to access another user's board via direct URL manipulation and verify a 403/404 is returned"
- "Attempt to inject HTML/JS in the card title field and verify it is sanitized on display"
- "Verify that the API does not return board data for users without view permission"
