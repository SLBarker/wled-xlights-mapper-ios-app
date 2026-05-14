---
phase: 05-carousel-peek-view
plan: "01"
subsystem: CSS / Hero Carousel
tags: [carousel, css, peek-view, opacity, absolute-positioning, mobile]
dependency_graph:
  requires: []
  provides:
    - ".hero-carousel__track-wrap width: 380px (desktop peek-view geometry)"
    - ".hero-carousel__slide--active BEM modifier (opacity: 1, for Plan 02 JS toggle)"
    - ".hero-carousel__arrow absolute positioning inside viewport"
    - "@media (max-width: 960px) carousel overrides: 320px track-wrap, 9px arrow offsets"
  affects:
    - "index.html CSS block (lines ~340–415 desktop rules, lines ~993–997 mobile overrides)"
tech_stack:
  added: []
  patterns:
    - "BEM modifier toggle (--active suffix overrides base opacity)"
    - "CSS transform compose pattern (translateY + scale in hover rule to prevent jump)"
    - "Absolute positioning inside a position: relative containing block"
key_files:
  created: []
  modified:
    - path: "index.html"
      changes:
        - ".hero-carousel__viewport: removed gap: 1.5rem (lines ~340–344)"
        - ".hero-carousel__track-wrap: width changed 240px → 380px (line ~348)"
        - ".hero-carousel__slide: added opacity: 0.5 and transition: opacity var(--transition) (lines ~358–365)"
        - ".hero-carousel__slide--active: new BEM modifier rule inserted (lines ~367–369)"
        - ".hero-carousel__arrow: removed flex-shrink: 0; added position: absolute, top: 50%, transform: translateY(-50%), z-index: 2 (lines ~372–389)"
        - ".hero-carousel__arrow:hover: transform changed to translateY(-50%) scale(1.08) (line ~394)"
        - "#carousel-prev and #carousel-next: new desktop rules with left: 13px / right: 13px (lines ~402–406)"
        - "@media (max-width: 960px): added .hero-carousel__track-wrap { width: 320px }, #carousel-prev { left: 9px }, #carousel-next { right: 9px } (lines ~994–996)"
decisions:
  - "Slide width kept at 240px (flex: 0 0 240px / width: 240px) — Plan 02 JS uses translateX(-N * 240px) offset"
  - "overflow: hidden kept on track-wrap (not viewport) — arrows live in viewport stacking context above the clip"
  - "Arrow offsets: 13px desktop = (70px peek - 44px button) / 2; 9px mobile = visual centre of 40px peek strip with slight buffer"
  - "Hover transform composed as translateY(-50%) scale(1.08) — prevents vertical jump by re-stating the base translate"
metrics:
  duration: "~10 minutes"
  completed: "2026-05-14"
  tasks_completed: 2
  tasks_total: 2
  files_modified: 1
---

# Phase 5 Plan 01: Carousel Peek-View CSS Summary

CSS-only conversion of the hero carousel from a 240px single-slide viewport with flex-sibling arrows to a 380px peek-view viewport with absolute-positioned arrows, slide opacity dimming, and a BEM active modifier ready for Plan 02 JavaScript consumption.

## What Changed

### Task 1 — Viewport, Track-Wrap, Slide, and Active Modifier

**`.hero-carousel__viewport` (line ~340):**
- REMOVED: `gap: 1.5rem;`
- KEPT: `position: relative; display: flex; align-items: center;`
- The viewport retains `position: relative` as the containing block for absolute-positioned arrows (Task 2).

**`.hero-carousel__track-wrap` (line ~347):**
- CHANGED: `width: 240px;` → `width: 380px;`
- KEPT: `overflow: hidden;`
- Desktop peek geometry: 380px viewport, 240px slide → 70px visible on each side.

**`.hero-carousel__slide` (line ~358):**
- ADDED: `opacity: 0.5;`
- ADDED: `transition: opacity var(--transition);`
- UNCHANGED: `flex: 0 0 240px; width: 240px; display: flex; flex-direction: column; align-items: center;`

**`.hero-carousel__slide--active` (line ~367) — NEW RULE:**
```css
.hero-carousel__slide--active {
  opacity: 1;
}
```
Inserted immediately after `.hero-carousel__slide` closing brace, following the BEM modifier pattern used by `.hero-carousel__dot--active`.

### Task 2 — Arrow Absolute Positioning with Hover Compose-Fix and Mobile Overrides

**`.hero-carousel__arrow` (line ~372):**
- REMOVED: `flex-shrink: 0;`
- ADDED: `position: absolute; top: 50%; transform: translateY(-50%); z-index: 2;`

**`.hero-carousel__arrow:hover` (line ~391):**
- CHANGED: `transform: scale(1.08);` → `transform: translateY(-50%) scale(1.08);`
- Critical compose-fix: restating the base `translateY(-50%)` in the hover rule prevents the button from jumping downward when hovered.

**`#carousel-prev` and `#carousel-next` (lines ~402–406) — NEW RULES:**
```css
#carousel-prev {
  left: 13px;
}
#carousel-next {
  right: 13px;
}
```
Desktop offset: `(70px peek strip - 44px button) / 2 = 13px` centres the button over the peek strip.

**`@media (max-width: 960px)` additions (lines ~994–996):**
```css
.hero-carousel__track-wrap { width: 320px; }
#carousel-prev { left: 9px; }
#carousel-next { right: 9px; }
```
Added inside the SINGLE existing 960px media block (ARCHITECTURE.md constraint: no second block).
Mobile peek geometry: 320px viewport, 240px slide → 40px each side. Arrow offset 9px centres 36px button over 40px strip with visual buffer.

## CSS Contracts for Plan 02 (JS)

| Contract | Value | Status |
|----------|-------|--------|
| Slide width | `flex: 0 0 240px; width: 240px;` | Confirmed unchanged — Plan 02 JS uses `translateX(-N * 240px)` |
| Active modifier | `.hero-carousel__slide--active { opacity: 1; }` | Defined — Plan 02 JS toggles this class on each slide |
| Track-wrap visible area | 380px desktop / 320px mobile | Defined — peek shows on both sides of active slide |
| Arrow positioning | `position: absolute` inside `position: relative` viewport | Defined — Plan 02 JS only needs to toggle `visibility` |

## Approximate Line Ranges Modified

| Rule | Pre-edit lines | Post-edit lines | Change |
|------|---------------|-----------------|--------|
| `.hero-carousel__viewport` | 340–345 | 340–344 | Removed `gap: 1.5rem` |
| `.hero-carousel__track-wrap` | 347–350 | 347–350 | Width 240px → 380px |
| `.hero-carousel__slide` | 358–364 | 358–365 | Added opacity + transition |
| `.hero-carousel__slide--active` | (new) | 367–369 | New BEM modifier |
| `.hero-carousel__arrow` | 367–381 | 372–389 | Removed flex-shrink, added absolute pos |
| `.hero-carousel__arrow:hover` | 383–387 | 391–395 | Compose-fix on transform |
| `#carousel-prev / #carousel-next` | (new) | 402–406 | New desktop ID rules |
| `@media (max-width: 960px)` additions | (new) | ~994–996 | 3 mobile overrides added |

## Deviations from Plan

None — plan executed exactly as written.

All values match CONTEXT.md decisions verbatim:
- D-01: 380px desktop / 320px mobile track-wrap width
- D-02: opacity 0.5 base / opacity 1 active
- D-03: absolute-positioned arrows inside position: relative viewport
- D-06: 13px desktop / 9px mobile arrow horizontal offsets

No new files created. No `<link>` tags added. No JavaScript modified. Exactly one `@media (max-width: 960px)` block in the file.

## Known Stubs

None — this plan is CSS geometry only. No data rendering, no stub values.

## Threat Flags

None — all changes are static CSS layout/opacity/positioning properties. No new network endpoints, auth paths, file access patterns, or schema changes.

## Self-Check: PASSED

- index.html modified: FOUND (git log confirms f7d0057 and 3e48944)
- width: 380px in track-wrap: FOUND (line 348)
- .hero-carousel__slide--active: FOUND (line 367)
- opacity: 0.5 in slide base rule: FOUND (line 363)
- gap: 1.5rem absent from viewport: CONFIRMED (not present in lines 340–344)
- flex: 0 0 240px preserved: FOUND (line 358)
- position: absolute in arrow: FOUND (line 385)
- translateY(-50%) scale(1.08) in hover: FOUND (line 394)
- #carousel-prev: 2 matches (desktop + mobile): CONFIRMED
- #carousel-next: 2 matches (desktop + mobile): CONFIRMED
- width: 320px in mobile block: FOUND (line 994)
- Exactly 1x @media (max-width: 960px): CONFIRMED (grep -c returns 1)
