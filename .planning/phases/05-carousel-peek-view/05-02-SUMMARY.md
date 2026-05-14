---
phase: 05-carousel-peek-view
plan: "02"
subsystem: carousel-js
tags: [carousel, javascript, animation, accessibility]
dependency_graph:
  requires: [05-01]
  provides: [carousel-js-controller]
  affects: [hero-carousel]
tech_stack:
  added: []
  patterns: [pixel-based-translateX, classList-toggle, visibility-boundary-management]
key_files:
  created: []
  modified:
    - index.html
decisions:
  - "Kept pixel offset at 240px per slide to match the 240px slide width defined in Plan 01 CSS"
  - "Used visibility:hidden/visible (not display:none) on arrows — absolute positioning means no layout shift"
  - "Active class toggle mirrors dot toggle pattern already in goTo() for consistency"
metrics:
  duration: 5m
  completed_date: "2026-05-14"
  tasks_completed: 1
  tasks_total: 1
---

# Phase 5 Plan 02: Carousel JS Controller (goTo() Update) Summary

**One-liner:** Updated carousel `goTo()` to use 240px pixel-based translateX, toggle `--active` slide class, and hide boundary arrows with `visibility: hidden`.

## What Was Built

The `goTo()` function inside the hero carousel IIFE (`index.html`) was updated with exactly three targeted changes required for the 380px peek-view layout established by Plan 01.

## Lines Modified in index.html

| Line | Before | After |
|------|--------|-------|
| 1727 | `translateX(-${current * 100}%)` | `translateX(-${current * 240}px)` |
| 1735 | _(new line inside slides.forEach)_ | `slide.classList.toggle('hero-carousel__slide--active', i === current);` |
| 1737 | _(new line after slides.forEach)_ | `prevBtn.style.visibility = current === 0 ? 'hidden' : 'visible';` |
| 1738 | _(new line after slides.forEach)_ | `nextBtn.style.visibility = current === total - 1 ? 'hidden' : 'visible';` |

Post-edit `goTo()` spans lines **1725–1739**.

## Confirmation Checks

### translateX pixel offset
- `grep -n 'current \* 240}px' index.html` returns line 1727 — confirmed present
- `grep -n 'current \* 100' index.html` returns no matches — old percentage-based line removed

### hero-carousel__slide--active toggle
- `grep -n "hero-carousel__slide--active" index.html` returns:
  - Line 367 — CSS definition (established by Plan 01)
  - Line 1735 — JS toggle inside `slides.forEach` — confirmed present

### Arrow boundary visibility
- `prevBtn.style.visibility = current === 0 ? 'hidden' : 'visible';` — line 1737 confirmed
- `nextBtn.style.visibility = current === total - 1 ? 'hidden' : 'visible';` — line 1738 confirmed
- Both set on every `goTo()` call, including the initial `goTo(0)` on page load
- Initial call hides `prevBtn` automatically because `current === 0`

## Unchanged Behaviour Preserved

- `dots.forEach` block: dot active class toggle and `aria-current` attribute — unchanged
- `slides.forEach`: `aria-hidden` attribute management — unchanged, active-class toggle added after it
- Auto-advance timer (`setInterval` calling `goTo(current + 1)`) — unchanged
- Touch swipe handlers (`touchstart`/`touchend`) — unchanged
- Mouse hover pause handlers (`mouseenter`/`mouseleave`) — unchanged

## Deviations from CONTEXT.md

None. All three changes match the plan exactly:
- D-07 (pixel translateX): implemented as `translateX(-${current * 240}px)`
- D-08 (goTo() updates): active class inside `slides.forEach`, visibility after it
- D-04/D-05 (natural edges and arrow hiding): `prevBtn` hidden at index 0, `nextBtn` hidden at `total - 1`

## Threat Surface Scan

No new trust boundaries introduced. `goTo()` consumes only integer indices from same-origin click events and the internal `setInterval` timer. The existing modulus guard `((n % total) + total) % total` bounds all inputs safely. No credentials, PII, or external data accessed.

## Self-Check

- [x] `index.html` modified (1 file changed, 4 insertions, 1 deletion)
- [x] Commit `03f2385` exists: `feat(05-02): update goTo() with pixel offset, active class, and boundary arrow visibility`
- [x] `translateX(-${current * 240}px)` present at line 1727
- [x] `hero-carousel__slide--active` toggle present at line 1735
- [x] `prevBtn.style.visibility` present at line 1737
- [x] `nextBtn.style.visibility` present at line 1738
- [x] Old `current * 100}%` pattern absent (zero matches)

## Self-Check: PASSED
