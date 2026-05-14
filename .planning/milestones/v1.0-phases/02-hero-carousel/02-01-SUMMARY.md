---
phase: 02-hero-carousel
plan: 01
subsystem: ui
tags: [css, carousel, hero, bem, design-tokens, responsive]

# Dependency graph
requires:
  - phase: 01-mobile-polish
    provides: Responsive CSS breakpoints and mobile nav already in place
provides:
  - Complete hero carousel CSS section with all BEM classes
  - Mobile responsive overrides for carousel
affects: [02-hero-carousel plan 02 — markup and JS depend on these class names]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "BEM naming for carousel: .hero-carousel, .hero-carousel__viewport, .hero-carousel__track, .hero-carousel__slide, .hero-carousel__arrow, .hero-carousel__dots, .hero-carousel__dot"
    - "CSS custom properties used exclusively — no hardcoded hex/rgb in carousel styles"
    - "Single-file constraint maintained — all CSS inline in <style> block"

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "Carousel track uses overflow:hidden + flex layout — no scrollbar, JS-driven translate"
  - "Arrow buttons sized 44px desktop / 36px mobile for touch target compliance"
  - "Active dot uses scale(1.25) + --accent fill for clear current-slide indication"

patterns-established:
  - "/* ── Section Name ── */ comment banners delimit CSS sections"
  - "Carousel mobile overrides go inside @media (max-width: 768px) replacing removed hero__phones rules"

requirements-completed: [HERO-01, HERO-02, HERO-03]

# Metrics
duration: 6min
completed: 2026-05-14
---

# Phase 2 Plan 01: Hero Carousel CSS Summary

**Deleted fan-strip `.hero__phones` CSS and tilted-phone transforms; inserted complete `/* ── Hero carousel ── */` section with all BEM classes styled via design tokens**

## Performance

- **Duration:** 6 min
- **Started:** 2026-05-14T06:36:00Z
- **Completed:** 2026-05-14T06:42:29Z
- **Tasks:** 1
- **Files modified:** 1

## Accomplishments
- Removed `.hero__phones` fan-strip block (8 lines) and the three tilted `nth-child` transform rules
- Replaced mobile `hero__phones` override with carousel-specific responsive rules (`margin-top: 3rem`, arrow `36px`)
- Added full `/* ── Hero carousel ── */` CSS section with 9 BEM classes: `.hero-carousel`, `.hero-carousel__viewport`, `.hero-carousel__track-wrap`, `.hero-carousel__track`, `.hero-carousel__slide`, `.hero-carousel__arrow` (+ `:hover`, `:focus-visible`), `.hero-carousel__dots`, `.hero-carousel__dot`, `.hero-carousel__dot--active`
- All carousel token usage verified — `var(--surface)`, `var(--accent)`, `var(--shadow)`, `var(--transition)` — zero hardcoded colours

## Task Commits

Each task was committed atomically:

1. **Task 1: Remove old hero__phones CSS and add carousel CSS section** - `35dfd36` (feat)

## Files Created/Modified
- `index.html` - Removed fan-strip CSS; added hero carousel CSS section (net +90 / -18 lines in style block)

## Decisions Made
- None beyond plan specification — all class names, token choices, and layout approach (overflow:hidden + flex track) were specified in the plan.

## Deviations from Plan

None — plan executed exactly as written.

## Issues Encountered
- The automated verify check `grep 'hero-carousel__arrow' index.html | grep -c 'var(--'` returned 0 because CSS properties with `var(--` are on indented child lines, not the selector line. Manual verification confirmed `var(--surface)`, `var(--shadow)`, `var(--accent)`, `var(--transition)` all appear inside the `.hero-carousel__arrow {}` block — no hardcoded values. Check was a false negative; implementation is correct.

## User Setup Required

None — no external service configuration required.

## Next Phase Readiness
- All carousel CSS BEM classes are defined and ready; Plan 02 can now safely add the carousel HTML markup and JavaScript without any style-less flash.
- The `.phone-bezel` block remains intact — Plan 02 will reuse these phone mockup styles inside carousel slides.
- No blockers.

---
*Phase: 02-hero-carousel*
*Completed: 2026-05-14*
