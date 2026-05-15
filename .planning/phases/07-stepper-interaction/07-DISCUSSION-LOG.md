# Phase 7: Stepper Interaction - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-15
**Phase:** 7-Stepper Interaction
**Areas discussed:** Scroll re-entry behavior, Mobile panel animation, Arrow key navigation

---

## Scroll Re-entry Behavior

### Q1: When the section re-enters the viewport after the user has navigated to a different step, what should happen?

| Option | Description | Selected |
|--------|-------------|----------|
| Reset to Step 1 every time | Each time the section scrolls into view, Step 1 is always re-expanded. Predictable but may feel disruptive. | |
| Fire once only | Step 1 auto-expands only the first time the section enters the viewport. After that, the user's choice is preserved. Matches the lazy-video observer pattern. | ✓ |
| You decide | Claude picks the approach that best fits the existing code patterns. | |

**User's choice:** Fire once only

---

### Q2: The fire-once trigger needs to observe the section entering the viewport. Which element should it watch?

| Option | Description | Selected |
|--------|-------------|----------|
| Each .stepper element | One observer per stepper block. Each fires independently — correct for two separate steppers that may scroll into view at different times. | ✓ |
| The #how-it-works section | One observer on the parent section. Both steppers auto-expand simultaneously. Simpler but less precise. | |

**User's choice:** Each .stepper element

---

## Mobile Panel Animation

### Q1: Should panels animate open/close on mobile?

| Option | Description | Selected |
|--------|-------------|----------|
| Animate height on mobile | JS sets panel.style.height = panel.scrollHeight + 'px' to open, '0' to close. CSS transition plays. | ✓ |
| Instant toggle everywhere | Use display:none/block on mobile too, matching desktop. Simpler JS, no inline heights. | |

**User's choice:** Animate height on mobile

---

### Q2: When a panel opens on mobile, should the page scroll to bring the expanded content into view?

| Option | Description | Selected |
|--------|-------------|----------|
| No scroll | Panel expands in place — user can scroll manually. Simpler and less disorienting. | ✓ |
| Scroll into view | After expanding, JS calls panel.scrollIntoView(). Helpful for tall panels. | |

**User's choice:** No scroll

---

## Arrow Key Navigation

### Q1: Should JS add arrow key stepping between steps?

| Option | Description | Selected |
|--------|-------------|----------|
| Tab + Enter/Space only | <button> elements already provide full keyboard accessibility. Correct for disclosure widget pattern. | |
| Arrow keys too | Add left/right (desktop) and up/down (mobile) arrow key navigation. Matches ARIA tablist pattern. | ✓ |

**User's choice:** Arrow keys too

---

### Q2: For arrow key navigation, how should the viewport detection work?

| Option | Description | Selected |
|--------|-------------|----------|
| Check window.innerWidth at keydown time | Left/right always fire — check width at keydown. Simple, no resize listener, matches 767px breakpoint. | ✓ |
| Pre-compute on load + resize | Compute isMobile on load and update via ResizeObserver or window resize listener. | |

**User's choice:** Check window.innerWidth at keydown time

---

## Claude's Discretion

- Exact threshold for the auto-expand IntersectionObserver (0.15 suggested)
- Whether to add `tabIndex="-1"` to non-active headers to reduce tab stops on desktop
- TransitionEnd cleanup approach (one-shot listener vs. timeout)

## Deferred Ideas

None — discussion stayed within phase scope.
