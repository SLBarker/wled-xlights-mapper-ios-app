---
phase: 04-nav-download-cta
plan: "01"
subsystem: ui
tags: [html, css, nav, responsive, flexbox]

# Dependency graph
requires: []
provides:
  - Nav breakpoint moved from 768px to 960px — links no longer wrap at narrow desktop widths
  - Download CTA extracted from .site-nav__links to direct flex child of .site-nav__inner
  - Download CTA visible at all viewport widths including mobile (alongside hamburger)
  - Download CTA shows 14x14 circle-with-down-arrow SVG icon to the left of "Download" text
  - Download CTA hover: translateY(-2px) lift + 0 12px 40px rgba(0,113,227,0.5) glow matching hero button
  - Download CTA removed from mobile drawer (drawer has 7 items only)
affects: []

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "CTA as direct flex child of nav inner — not inside the links <ul>, always visible regardless of hamburger state"
    - "Hero-matching hover style for nav pill CTAs: translateY(-2px) + blue glow box-shadow"

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "CTA extracted from <ul> to bare <a> as flex sibling — allows hamburger to hide links without hiding CTA"
  - "Breakpoint raised to 960px to prevent nav link wrapping at narrow desktop widths"
  - "Hover style aligned with hero .btn-primary:hover (translateY + box-shadow) for visual consistency"
  - "Drawer CTA removed entirely — no duplicate Download link needed since CTA is always visible in bar"

patterns-established:
  - "Nav CTA placement: direct flex child of .site-nav__inner, ordered after </ul> and before <button class=site-nav__toggle>"

requirements-completed: [NAV-01, NAV-02, NAV-03, UI-01]

# Metrics
duration: 12min
completed: 2026-05-14
---

# Phase 4 Plan 01: Nav & Download CTA Summary

**Nav breakpoint raised to 960px, Download CTA extracted as always-visible flex child with SVG icon and hero-matching hover style**

## Performance

- **Duration:** 12 min
- **Started:** 2026-05-14T13:35:00Z
- **Completed:** 2026-05-14T13:47:00Z
- **Tasks:** 2
- **Files modified:** 1

## Accomplishments
- Nav responsive breakpoint changed from 768px to 960px — links collapse to hamburger before they can wrap
- Download CTA moved from inside `.site-nav__links <ul>` to a bare `<a>` as direct flex child of `.site-nav__inner`, making it visible at all viewport widths including mobile
- Added 14x14 SVG download icon (circle + down-arrow, matching hero button paths) inside the nav CTA
- CTA hover style updated: `translateY(-2px)` lift + `box-shadow: 0 12px 40px rgba(0,113,227,0.5)` blue glow, matching hero `.btn-primary:hover` exactly
- Download `<li>` removed from mobile drawer — drawer now has exactly 7 items, no duplicate CTA

## Task Commits

Each task was committed atomically:

1. **Task 1: Update .site-nav__cta CSS — flex layout, box-shadow transition, hover style** - `bb3cac2` (feat)
2. **Task 2: Update nav HTML — extract CTA from links list, add SVG icon, remove from drawer** - `24e05b8` (feat)

**Plan metadata:** (docs commit follows)

## Files Created/Modified
- `index.html` - Nav CSS and HTML updated: breakpoint, CTA position, CTA styles, icon, hover

## Decisions Made
- Raised breakpoint to 960px rather than an intermediate value — gives comfortable margin before 7 nav links would wrap
- Used `display: inline-flex` on `.site-nav__cta` to align icon and text horizontally with controlled gap
- Removed `opacity` from CTA transition (was `opacity var(--transition), transform var(--transition)`) — hover now uses only `transform` and `box-shadow` to match hero pattern
- Updated HTML comment "shown only at ≤768px" to "shown only at ≤960px" when updating the breakpoint (auto-fix)

## Deviations from Plan

### Auto-fixed Issues

**1. [Rule 1 - Bug] Updated stale HTML comment referencing old 768px breakpoint**
- **Found during:** Task 1 verification
- **Issue:** After changing the @media threshold to 960px, the HTML comment `<!-- Mobile nav drawer (shown only at ≤768px) -->` still referenced 768px, causing `grep -c "768px" index.html` to return 1 instead of 0 (acceptance criterion requires 0)
- **Fix:** Updated comment to `<!-- Mobile nav drawer (shown only at ≤960px) -->`
- **Files modified:** index.html
- **Verification:** `grep -c "768px" index.html` returns 0
- **Committed in:** bb3cac2 (Task 1 commit)

---

**Total deviations:** 1 auto-fixed (Rule 1 - stale comment)
**Impact on plan:** Trivial comment update required for acceptance criterion compliance. No scope creep.

## Issues Encountered
- `grep -c "inline-flex" index.html` returns 3 (not 1 as the plan's acceptance criterion states). The other 2 occurrences are pre-existing uses of `inline-flex` in unrelated CSS rules (e.g., `.btn`, `.contact__link`). The plan acceptance criterion was written expecting only the nav CTA would use `inline-flex`, but pre-existing rules already used it. The nav CTA CSS correctly uses `display: inline-flex` — the criterion was slightly overfitted. All functional criteria pass.

## Known Stubs
- `phone-bezel__placeholder` elements exist in the hero carousel (pre-existing from prior phases, not introduced by this plan). Not related to this plan's scope.

## Threat Flags
None — changes are purely CSS property updates and structural repositioning of an existing internal anchor (`href="#download"`). No new network surface, no auth paths, no external URLs introduced.

## Next Phase Readiness
- All 4 Phase 4 requirements completed: NAV-01, NAV-02, NAV-03, UI-01
- Phase 4 (Nav & Download CTA) is complete — single plan, single wave, no follow-on work
- Phase 5 (Carousel Peek-View) is the next active milestone target

---
*Phase: 04-nav-download-cta*
*Completed: 2026-05-14*
