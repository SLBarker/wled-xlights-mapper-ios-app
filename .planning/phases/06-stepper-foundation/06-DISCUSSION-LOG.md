# Phase 6: Stepper Foundation - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-15
**Phase:** 6-stepper-foundation
**Areas discussed:** HTML structure strategy, Breakpoint approach, Old component cleanup, Scroll animation scope

---

## HTML Structure Strategy

| Option | Description | Selected |
|--------|-------------|----------|
| Dual navigation — both rail and items in DOM | HTML contains .stepper__rail (tab buttons, desktop-only) AND .stepper__item × 5 (each with header + panel). Rail hidden on mobile; headers hidden on desktop. | |
| Items-only, CSS reshapes into a rail | HTML contains only .stepper__item × 5. On desktop, CSS Grid + display:contents repositions all headers into a row (forming the rail). No content duplication. | ✓ |
| You decide | Leave DOM structure choice to the planner/executor. | |

**User's choice:** Items-only, CSS reshapes into a rail

**Follow-up Q — display:contents mechanism:**

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — display:contents is fine | Supported in all modern browsers. Removes item wrapper from layout but keeps it in accessibility tree. | ✓ |
| No — avoid display:contents | Use absolute positioning or a JS-built rail instead. | |

**User's choice:** Yes — display:contents is fine

**Follow-up Q — heading inside panel:**

User asked to see a concrete example of how current step-card HTML maps to the new stepper markup (specifically how the step title/heading and content paragraphs relate in the DOM). Confirmed after seeing example:

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — that's correct | Title in .stepper__title (header, always visible) + h3 repeated inside .stepper__content (panel, visible when expanded). | ✓ |
| No heading inside panel | Panel shows only description paragraphs, no repeated heading. | |

**User's choice:** Yes — that's correct

**Notes:** `.stepper__header` serves as both desktop tab and mobile accordion toggle; spec's `.stepper__tab` class is not used. `adjacent sibling selector` works through `display:contents` since header and panel remain DOM siblings inside `.stepper__item`.

---

## Breakpoint Approach

| Option | Description | Selected |
|--------|-------------|----------|
| Add a second @media block at 768px (stepper-only) | Existing 960px block stays for nav/carousel. New 767px block added for stepper only. Follows UI-SPEC exactly. | ✓ |
| Use 960px for the stepper too | Single breakpoint but stepper collapses to accordion earlier than spec intends. | |
| You decide | Leave breakpoint choice to the planner. | |

**User's choice:** Add a second @media block at 768px (stepper-only)

**Notes:** Two coexisting breakpoints — 960px for existing components (nav, carousel), 767px for the new stepper component only.

---

## Old Component Cleanup

| Option | Description | Selected |
|--------|-------------|----------|
| Remove completely in Phase 6 | Delete .step-card, .numbered-list, .workflow__steps CSS + HTML entirely. | |
| Keep old CSS as comments | Replace HTML; comment out old CSS rules rather than deleting. Safety net while component is unproven. | ✓ |
| Leave old code until Phase 7 verifies | Keep both old and new code in Phase 6. Phase 7 removes old code. | |

**User's choice:** Keep old CSS as comments

**Notes:** Old HTML is replaced. Old CSS (lines 565–695, line 1006) is commented out, not deleted. `.workflow__phase-title` CSS (lines 573–580) is kept as-is — it's reused by the new stepper structure.

---

## Scroll Animation Scope

| Option | Description | Selected |
|--------|-------------|----------|
| Update selector to .stepper__item in Phase 6 | Replace .step-card with .stepper__item in animatedEls querySelectorAll (line 1615). New items get scroll-reveal immediately. | ✓ |
| Defer to Phase 7 | Leave .step-card selector (selects nothing — no error, no effect). Phase 7 updates it. | |
| Remove scroll animation from stepper entirely | Don't animate stepper items on scroll-in. | |

**User's choice:** Update selector to .stepper__item in Phase 6

---

## Claude's Discretion

- Exact CSS mechanism for hiding non-active panels on desktop (`display:none` vs others)
- Whether to use explicit `grid-row: 2` on panels or rely on implicit grid placement
- Exact ID naming convention for `aria-controls` / panel `id` attributes

## Deferred Ideas

None — discussion stayed within phase scope.
