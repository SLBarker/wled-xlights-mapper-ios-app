---
phase: 01-mobile-and-polish
plan: "03"
subsystem: ui
tags: [performance, lazy-loading, intersection-observer, video, vanilla-js]

dependency_graph:
  requires:
    - phase: 01-mobile-and-polish
      provides: hamburger-nav-toggle, mobile-drawer-nav, single-column-grid-overrides
  provides:
    - lazy-video-loading
    - data-src-video-elements
    - lazy-video-observer
  affects: [index.html]

tech-stack:
  added: []
  patterns:
    - "Lazy video loading via IntersectionObserver with rootMargin 200px — defers src write until video is 200px from viewport"
    - "data-src pattern: video elements carry data-src instead of src on page load; observer promotes to src when triggered"
    - "IIFE-scoped vanilla JS for lazy video observer — no global namespace pollution"

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "data-src pattern chosen over poster+src swap — simpler, browser-native, no placeholder image needed"
  - "rootMargin 200px gives 200px early-load headroom so video is ready before it enters the viewport on most connections"
  - "video.load() called explicitly after src assignment to ensure browser starts fetching immediately"

patterns-established:
  - "Lazy video: data-src on HTML element, IntersectionObserver writes src and calls load(), then unobserves"
  - "Three IntersectionObserver instances co-exist: animation observer (io), hamburger (none), lazy video (lazyVideoObserver)"

requirements-completed: [PERF-01]

duration: "3min"
completed: "2026-05-13"
---

# Phase 1 Plan 03: Lazy Video Loading Summary

**IntersectionObserver-based lazy loading defers 13+ MB of MP4 video downloads until user scrolls within 200px of each video element**

## Performance

- **Duration:** ~3 min
- **Started:** 2026-05-13T00:00:00Z
- **Completed:** 2026-05-13T00:00:00Z
- **Tasks:** 1
- **Files modified:** 1

## Accomplishments

- Replaced `src` with `data-src` on both video elements (discovery.mp4 and 3dpreview.mp4)
- Added lazy video IntersectionObserver with `rootMargin: '200px'` that writes `src` and calls `video.load()` when a video scrolls within range
- Deferred download of discovery.mp4 (4.1 MB) and 3dpreview.mp4 (9.2 MB) — total 13.3 MB — from initial page load
- All other video attributes (autoplay, loop, muted, playsinline, style, aria-label) preserved unchanged
- Existing animation observer and hamburger nav script left untouched

## Task Commits

Each task was committed atomically:

1. **Task 1: Convert video src to data-src and add lazy-load observer** - `3999918` (feat)

## Files Created/Modified

- `index.html` - data-src on both video elements, new lazy video IntersectionObserver script block

## Decisions Made

- `data-src` pattern chosen (plan specification) — browser will not request the video until `src` is set, giving zero-cost deferral with no placeholder image needed
- `rootMargin: '200px'` provides a pre-load buffer so video starts fetching before it enters the viewport on most connections
- `video.load()` called explicitly after `src` assignment to ensure the browser initiates the network request immediately

## Deviations from Plan

None — plan executed exactly as written.

Note: The acceptance criteria stated `grep -c "data-src" index.html` should return exactly 4, counting "2 in HTML video elements, 2 in the JS — `video[data-src]` selector and `video.dataset.src`". In practice the count is 3: `video.dataset.src` uses the JS property accessor syntax and does not contain the literal string `data-src`. The implementation matches the plan's code specification exactly — this is a documentation error in the acceptance criteria, not an implementation shortfall.

## Issues Encountered

None.

## Known Stubs

None. Both video elements have their data-src pointing to real MP4 files that exist in `resources/video/`. The lazy observer will load them correctly when triggered.

## Threat Flags

No new security-relevant surface. The `data-src` attribute values are static strings embedded in HTML — no user input involved. The threat model disposition T-03-01 (accept) applies: values are not an XSS vector.

## Self-Check

- [x] `index.html` modified and exists
- [x] Commit 3999918 exists (Task 1)
- [x] `grep -n "data-src" index.html` returns 3 lines (2 HTML video elements + 1 JS selector)
- [x] `grep ' src="resources/video' index.html` returns 0 lines (no bare src on video elements)
- [x] `grep -c "lazyVideoObserver" index.html` returns 3 (declaration, observe call, unobserve call)
- [x] `grep -c "rootMargin.*200px" index.html` returns 1
- [x] `grep -c "video.load()" index.html` returns 1
- [x] `grep -c "IntersectionObserver" index.html` returns 2 (animation + video observers)
- [x] `grep -c "autoplay" index.html` returns 2
- [x] `grep -c "io.observe" index.html` returns 1 (original animation observer unchanged)
- [x] `grep -c "site-nav--open" index.html` returns 7 (hamburger script unchanged)
- [x] Exactly one `</body>` tag
- [x] Script order: animation observer → hamburger nav → lazy video (correct)

## Self-Check: PASSED

---
*Phase: 01-mobile-and-polish*
*Completed: 2026-05-13*
