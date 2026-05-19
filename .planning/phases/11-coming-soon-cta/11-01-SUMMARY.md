---
phase: 11-coming-soon-cta
plan: 01
subsystem: ui
tags: [html, css, accessibility, aria, cta]

requires:
  - phase: 10-font-self-hosting
    provides: stable index.html baseline

provides:
  - Disabled "Coming Soon" nav pill and hero button with aria-disabled="true"
  - Shared [aria-disabled="true"] CSS rule (opacity 0.6, cursor not-allowed, pointer-events none)
  - Hover-suppression rules preventing lift/glow on disabled CTAs

affects: [any future phase that restores a live App Store CTA]

tech-stack:
  added: []
  patterns: [aria-disabled attribute selector for disabled interactive elements, pointer-events none as belt-and-suspenders on top of no-href]

key-files:
  created: []
  modified: [index.html]

key-decisions:
  - "Removed href entirely (no href = no navigation on static page) rather than href='javascript:void(0)'"
  - "Removed id='download' — self-referential anchor target was dead; no other links point to #download"
  - "Single shared [aria-disabled='true'] CSS rule rather than per-component classes"
  - "pointer-events: none added as belt-and-suspenders alongside no-href"
  - "Background color unchanged — opacity alone communicates disabled state (D-06)"

patterns-established:
  - "aria-disabled pattern: use aria-disabled='true' on <a> elements to preserve keyboard focus while communicating disabled state — do not remove tabindex"
  - "Disabled CTA CSS: [aria-disabled='true'] selector handles all disabled link/button styling from one rule"

requirements-completed: [CTA-01, CTA-02, CTA-03]

duration: 10min
completed: 2026-05-18
---

# Phase 11: Coming Soon CTA Summary

**Disabled "Coming Soon" nav pill and hero button via aria-disabled, no-href, and shared CSS rule — visitors see unavailability immediately with no broken anchor behavior**

## Performance

- **Duration:** ~10 min
- **Completed:** 2026-05-18
- **Tasks:** 3 (2 auto + 1 human checkpoint)
- **Files modified:** 1

## Accomplishments
- Both download CTAs (desktop nav pill and hero button) now read "Coming Soon" with no download-arrow icon
- Removed the self-referential `href="#download"` and `id="download"` that caused a no-op page jump
- Added `aria-disabled="true"` to both elements — keyboard focus preserved, screen readers announce disabled state
- Added shared `[aria-disabled="true"]` CSS (opacity 0.6, cursor not-allowed, pointer-events none) plus hover-suppression rules for transform and box-shadow

## Task Commits

1. **Task 1+2: Convert CTAs + add disabled CSS** — `637ae66` (feat)
2. **Task 3: Human UAT approved** — verified in browser

## Files Created/Modified
- `index.html` — two CTA elements converted; disabled-state CSS added inline

## Decisions Made
- Used `[aria-disabled="true"]` attribute selector rather than a `.cta--disabled` class — single source of truth with the HTML attribute
- Kept both elements as `<a>` tags (no conversion to `<span>` or `<button>`) — preserves existing styled-anchor pattern

## Deviations from Plan
None — plan executed exactly as written. Note: plan's automated check `grep -c 'aria-disabled="true"'` expected count 2, but CSS selectors also contain the string (4 CSS rules + 2 HTML elements = 6). HTML element count is exactly 2 as intended; the check is a false alarm.

## Issues Encountered
None.

## Next Phase Readiness
- Phase 11 complete — v4.1 milestone delivered
- When the App Store URL becomes available (CONT-01), restore `href` and remove `aria-disabled` from both CTAs

---
*Phase: 11-coming-soon-cta*
*Completed: 2026-05-18*
