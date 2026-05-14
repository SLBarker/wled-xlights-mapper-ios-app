# Phase 2: Hero Carousel - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-13
**Phase:** 2-hero-carousel
**Areas discussed:** Slide composition, Transition style, Auto-advance, Controls layout

---

## Slide Composition

| Option | Description | Selected |
|--------|-------------|----------|
| 3 slides (existing fan strip) | Connect, Scan, Preview — matches what the fan strip currently shows | |
| 4 slides (add results.jpeg) | Connect, Scan, Preview, Results — shows full workflow end-to-end | ✓ |

**User's choice:** 4 slides — add results.jpeg as 4th slide.
**Captions:** Connect / Scan / Preview & Export / Results.
**Notes:** User also noted the carousel must be extensible — adding a slide should require only a new HTML element, not JS changes. Each slide's caption text must transition with the phone mockup as a single unit.

---

## Transition Style

| Option | Description | Selected |
|--------|-------------|----------|
| Slide left/right | CSS transform: translateX + transition. Classic carousel feel. | ✓ |
| Crossfade | CSS opacity + transition. Softer, more minimal. | |

**User's choice:** Slide left/right.
**Notes:** Caption text transitions with the image as a co-located unit, not independently.

---

## Auto-advance

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — auto-advance | Slides advance automatically on a timer | ✓ |
| Manual only | User must use arrows or swipe to advance | |

**Interval:** 4 seconds per slide.
**Pause on hover:** Yes — auto-advance pauses on mouseenter, resumes on mouseleave.
**Notes:** User confirmed pause-on-hover. Carousel loops back to first slide after last.

---

## Controls Layout

| Option | Description | Selected |
|--------|-------------|----------|
| Flanking the phone | Arrows appear to the left and right of the phone frame — always visible | ✓ |
| Overlaid on the phone | Arrows float over left/right edges of the phone frame | |

**Arrow style:** Icon-only circular buttons with SVG chevron icons.
**Dot indicators:** Below the phone mockup and caption.
**Mobile:** Arrows retained (not hidden) on mobile; touch/swipe also advances.

---

## Claude's Discretion

- Auto-advance resume behavior after manual interaction: at Claude's discretion (reasonable default: reset the 4s timer on each manual interaction, so it auto-advances 4s after the last user action).
- Arrow button hover/focus visual state: consistent with design system tokens.

## Deferred Ideas

None — discussion stayed within phase scope.
