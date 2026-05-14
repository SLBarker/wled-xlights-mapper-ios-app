---
phase: 02-hero-carousel
plan: 02
subsystem: ui
tags: [carousel, javascript, html, accessibility, aria, touch, bem]

# Dependency graph
requires:
  - phase: 02-hero-carousel plan 01
    provides: All BEM CSS classes for carousel, phone-bezel styles intact
provides:
  - Complete working hero carousel with 4 slides, arrows, dots, and JS controller
  - Touch swipe support and 4-second auto-advance with mouseenter pause
affects: [03-contact-section — hero section layout is now finalised]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "Carousel IIFE in own <script> block — one concern per block, consistent with existing hamburger and lazy-video IIFEs"
    - "Slide count DOM-derived via slides.length — extend carousel by adding HTML only, no JS constant"
    - "Negative modulo handled via ((n % total) + total) % total for clean loop-back"
    - "passive: true on touchstart/touchend — avoids browser scroll-blocking warnings"

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "Human checkpoint passed on first attempt — no rework needed"
  - "results.jpeg slide added as fourth slide (was absent from original 3-slide fan strip)"
  - "aria-hidden toggled per slide by goTo() so screen readers only announce the active slide"

patterns-established:
  - "Carousel extend pattern: add one .hero-carousel__slide div — JS auto-discovers via querySelectorAll"

requirements-completed: [HERO-01, HERO-02, HERO-03]

# Metrics
duration: 15min
completed: 2026-05-14
---

# Phase 02 Plan 02: Hero Carousel HTML + JS Summary

**4-slide hero carousel fully functional — translateX slide animation, arrow/dot/swipe controls, 4 s auto-advance, and accessible ARIA markup**

## Performance

- **Duration:** ~15 min
- **Started:** 2026-05-14T07:00:00Z
- **Completed:** 2026-05-14T07:15:00Z
- **Tasks:** 2
- **Files modified:** 1

## Accomplishments
- Replaced `.hero__phones` fan-strip with accessible `.hero-carousel` region (role=region, aria-roledescription=carousel, per-slide role=group)
- Added fourth slide (Results screenshot) that was missing from original 3-slide fan strip
- Wired full carousel JS IIFE: translateX-driven advance, prev/next buttons, dot sync, loop, 4 s auto-advance, mouseenter pause, and left/right touch swipe (40 px threshold)
- Human verification checkpoint passed on first attempt — no issues found

## Task Commits

Each task was committed atomically:

1. **Task 1: Replace .hero__phones HTML with 4-slide carousel markup** - `bc32e13` (feat)
2. **Task 2: Add carousel JavaScript IIFE** - `96ccd42` (feat)

## Files Created/Modified
- `index.html` - Replaced hero fan-strip with carousel HTML; added carousel JS IIFE before </body>

## Decisions Made
- None beyond plan specification — all element IDs, ARIA roles, and JS patterns were specified in the plan.

## Deviations from Plan

None — plan executed exactly as written.

## Issues Encountered
- None.

## User Setup Required
None — no external service configuration required.

## Next Phase Readiness
- Hero carousel is complete and human-verified. Phase 02 is done.
- Phase 03 (Contact/Feedback section) can proceed — hero layout is finalised.
- No blockers.

---
*Phase: 02-hero-carousel*
*Completed: 2026-05-14*
