---
phase: 06-stepper-foundation
plan: 02
subsystem: ui
tags: [html, stepper, bem, aria, phone-bezel, intersection-observer, workflow]

# Dependency graph
requires:
  - phase: 06-01-stepper-css-foundation
    provides: "Complete .stepper BEM CSS including desktop grid, mobile accordion, scroll-reveal base styles, and .stepper__header--active state rules"
provides:
  - Two .stepper HTML blocks in #how-it-works, each with 5 .stepper__item elements
  - Phone bezel screenshots wired to In the App steps 1-4 (connect, ar, results, preview)
  - In the App step 5 and all five Importing into xLights steps using .stepper__body--text-only
  - Updated animatedEls querySelectorAll targeting .stepper__item for scroll-reveal animation
  - Old <div class="workflow"> block removed
affects: [07-stepper-js-controller]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "Stepper HTML: items-only DOM with .stepper__item wrapping .stepper__header (button) + .stepper__panel"
    - "aria-controls/id linkage: panel-app-N and panel-xlights-N naming convention"
    - "Phone bezel reuse: .stepper__media > .phone-bezel > .phone-bezel__notch + .phone-bezel__frame > .phone-bezel__screen > img"
    - "Text-only steps: .stepper__body--text-only modifier; .stepper__content fills full width"

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "Preserved all existing step text content verbatim from the old .step-card and .numbered-list structures"
  - "Reused existing .phone-bezel CSS unchanged inside .stepper__media — no new bezel styles needed"
  - ".stepper__body--text-only applied to In the App step 5 (Export) and all 5 Importing into xLights steps"
  - "Panel IDs use descriptive kebab-case: panel-app-1 through panel-app-5, panel-xlights-1 through panel-xlights-5"
  - "Step 1 in each stepper pre-marked active with .stepper__header--active and aria-expanded=true; all others have aria-expanded=false"

patterns-established:
  - "Stepper HTML structure: .stepper > .stepper__item > button.stepper__header + div.stepper__panel > div.stepper__body > div.stepper__media? + div.stepper__content"
  - "aria-controls on button matches id on panel: aria-controls='panel-app-1' <-> id='panel-app-1'"
  - "Phone bezel inside stepper: .stepper__media > .phone-bezel > .phone-bezel__notch + .phone-bezel__frame > .phone-bezel__screen > img[loading=lazy]"

requirements-completed: [STEP-01, STEP-05, STEP-06, STEP-07, STEP-08]

# Metrics
duration: 3min
completed: 2026-05-15
---

# Phase 6 Plan 02: Stepper HTML Restructure + JS Selector Update Summary

**Two BEM-structured .stepper blocks with phone bezel screenshots replace the old .workflow div in #how-it-works, and the animatedEls scroll-reveal selector updated from .step-card to .stepper__item**

## Performance

- **Duration:** ~3 min
- **Started:** 2026-05-15T10:55:00Z
- **Completed:** 2026-05-15T10:57:23Z
- **Tasks:** 2
- **Files modified:** 1

## Accomplishments

- Removed the `<div class="workflow">` block (54 lines) and replaced it with two `.stepper` elements totalling 200+ lines of correctly structured BEM HTML
- "In the App" stepper: 5 items, steps 1-4 with `.stepper__media > .phone-bezel > img` (connect.jpeg, ar.jpeg, results.jpeg, preview.jpeg), step 5 text-only
- "Importing into xLights" stepper: 5 items, all text-only, same title/content as old numbered-list
- Updated `animatedEls` querySelectorAll to target `.stepper__item` — scroll-reveal animation now works on the new stepper items

## Task Commits

Each task was committed atomically:

1. **Task 1: Replace workflow HTML with two stepper blocks** - `5cfa1a2` (feat)
2. **Task 2: Update animatedEls JS selector** - `e963ef3` (feat)

## Files Created/Modified

- `index.html` - Old .workflow div removed; two .stepper blocks added with full BEM structure, screenshots, aria attributes; animatedEls selector updated

## Decisions Made

- Reused existing `.phone-bezel` component classes unchanged inside `.stepper__media` — no CSS modifications needed; the bezel CSS from prior phases handles rendering
- All existing step text content was preserved verbatim (trimmed trailing whitespace from original `.step-card` paragraphs which had trailing spaces)

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None. The line numbers referenced in the plan (1379-1448 for the workflow block) differed from actual line numbers (1603-1672) due to content added in prior phases, but the content was located using targeted grep and replaced correctly.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- Phase 6 is fully complete: CSS foundation (Plan 01) + HTML restructure + JS selector (Plan 02) are both committed
- Phase 7 (stepper JS controller) can add/remove `.stepper__header--active` and toggle panel `height` on mobile — no CSS or HTML structure changes needed
- All `aria-controls`/`id` linkage is in place for Phase 7 to use when driving accordion behaviour

---
*Phase: 06-stepper-foundation*
*Completed: 2026-05-15*
