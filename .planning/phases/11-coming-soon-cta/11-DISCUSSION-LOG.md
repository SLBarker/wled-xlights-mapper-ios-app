# Phase 11: coming-soon-cta - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-18
**Phase:** 11-coming-soon-cta
**Areas discussed:** Icon treatment, Disabled visual weight

---

## Icon Treatment

| Option | Description | Selected |
|--------|-------------|----------|
| Remove it entirely | Text-only pill/button — no icon. Simplest, no semantic confusion. | ✓ |
| Keep the download arrow | Arrow stays alongside "Coming Soon". Familiar visual shape, but semantically off — implies action. | |
| Swap for a clock/hourglass icon | Replace with a ⏳-style icon to visually reinforce "not yet". Requires a new SVG inline. | |

**User's choice:** Remove it entirely
**Notes:** No replacement icon — "Coming Soon" text carries the message on its own.

---

## Disabled Visual Weight

### Opacity level

| Option | Description | Selected |
|--------|-------------|----------|
| 50% opacity | Clearly unavailable — half brightness makes the disabled state unmissable. | |
| 60% opacity | Balanced — visibly dimmed but still readable. Common Apple-style disabled treatment. | ✓ |
| 70% opacity | Subtle dimming — still looks close to active. May not communicate disabled state strongly enough. | |

**User's choice:** 60% opacity

### Tooltip / extra cue

| Option | Description | Selected |
|--------|-------------|----------|
| No tooltip — text speaks for itself | "Coming Soon" in the button is already the message. A tooltip would be redundant. | ✓ |
| Browser title tooltip | Add `title="Coming soon"` attribute. Native browser tooltip on hover. | |
| Change background tint | Replace blue accent with a neutral grey. Different visual signal. | |

**User's choice:** No tooltip — text speaks for itself
**Notes:** Keep existing blue background, opacity-only treatment.

---

## Claude's Discretion

- CSS selector strategy: `[aria-disabled="true"]` attribute selector vs. a `.cta--disabled` class
- Whether to combine both CTAs into one CSS rule or two separate rules

## Deferred Ideas

None — discussion stayed within phase scope.
