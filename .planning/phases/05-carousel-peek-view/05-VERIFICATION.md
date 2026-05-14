---
phase: 05-carousel-peek-view
verified: 2026-05-14T00:00:00Z
status: human_needed
score: 10/10 must-haves verified
overrides_applied: 0
human_verification:
  - test: "Open index.html in a browser. Load slide 1 — confirm the slide is centred with partial views of slides 2 visible on the right, prev arrow hidden, next arrow visible."
    expected: "Active slide centred at full opacity; ~70px of the adjacent slide is visible on the right; left arrow is hidden; right arrow is visible."
    why_human: "Visual layout, opacity dimming, and arrow visibility on load require a browser render — cannot be confirmed by static code grep alone."
  - test: "Click the next arrow three times. On each click, confirm the previously active slide dims to opacity 0.5 and the newly active slide is at full opacity. Confirm the final slide hides the next arrow."
    expected: "Smooth opacity transition on slide change; only the centred slide is fully opaque; next arrow hidden on slide 4."
    why_human: "Opacity transition and peek geometry rendering are visual behaviours that require browser evaluation."
  - test: "Resize the browser window to 800px wide (below 960px breakpoint). Confirm the carousel viewport narrows and arrows remain centred over the peek strips."
    expected: "Carousel viewport at 320px; 40px peek each side; arrows at 9px offset; no layout jump."
    why_human: "Responsive behaviour at the 960px breakpoint requires a live browser render."
  - test: "Swipe left on a touch device or simulate touch events in browser devtools. Confirm slides advance and boundary arrow visibility updates correctly."
    expected: "Touch swipe advances or retreats slides; prev/next arrow boundary hiding works on mobile."
    why_human: "Touch event handling requires a physical device or devtools touch simulation."
  - test: "Wait 4 seconds without interaction. Confirm auto-advance cycles to the next slide and continues cycling through all four slides."
    expected: "Each slide becomes active in turn on a 4-second interval; dot indicators update; arrow boundary hiding updates at first and last slides."
    why_human: "Timer-driven behaviour requires real-time observation in a browser."
---

# Phase 5: Carousel Peek-View Verification Report

**Phase Goal:** Rework carousel to show partial prev/next slides on both sides of the centred active slide
**Verified:** 2026-05-14
**Status:** human_needed
**Re-verification:** No — initial verification

## Goal Achievement

### Observable Truths

| # | Truth | Status | Evidence |
|---|-------|--------|----------|
| 1 | Hero carousel viewport is 380px wide on desktop, centred, with the active slide flanked by 70px of the previous slide on the left and 70px of the next slide on the right | ✓ VERIFIED | `.hero-carousel__track-wrap { overflow: hidden; width: 380px; }` at line 348; slide width `flex: 0 0 240px; width: 240px` at lines 358-359; (380-240)/2 = 70px peek per side |
| 2 | Non-active slides render at opacity 0.5; the active slide renders at opacity 1.0 with a smooth transition between states | ✓ VERIFIED | `.hero-carousel__slide { opacity: 0.5; transition: opacity var(--transition); }` at lines 363-364; `.hero-carousel__slide--active { opacity: 1; }` at lines 367-369 |
| 3 | Arrows are overlaid on top of the left and right peek strips (absolute-positioned inside the viewport) rather than sitting outside the track as flex siblings | ✓ VERIFIED | `.hero-carousel__arrow` has `position: absolute; top: 50%; transform: translateY(-50%); z-index: 2` at lines 385-388; both `#carousel-prev` and `#carousel-next` buttons are DOM children of `.hero-carousel__viewport` (HTML lines 1093, 1155) which has `position: relative` at line 341 |
| 4 | Arrow hover lift preserves the vertical centring — the button does not jump when hovered | ✓ VERIFIED | `.hero-carousel__arrow:hover { transform: translateY(-50%) scale(1.08); }` at line 394 — composed transform re-states the base translateY so vertical position is preserved |
| 5 | At viewport widths ≤ 960px the carousel viewport narrows to 320px (40px peek each side) and arrow horizontal offsets are reduced accordingly | ✓ VERIFIED | Inside the single `@media (max-width: 960px)` block: `.hero-carousel__track-wrap { width: 320px; }` at line 994; `#carousel-prev { left: 9px; }` at line 995; `#carousel-next { right: 9px; }` at line 996 |
| 6 | goTo() uses pixel-based translateX(-${current * 240}px) so the 380px-wide track-wrap centres the active 240px slide correctly | ✓ VERIFIED | `track.style.transform = \`translateX(-${current * 240}px)\`` at line 1727; zero matches for old `current * 100` pattern |
| 7 | goTo() toggles .hero-carousel__slide--active on the current slide so only the centred slide is at full opacity | ✓ VERIFIED | `slide.classList.toggle('hero-carousel__slide--active', i === current);` at line 1735, inside `slides.forEach` |
| 8 | goTo() hides the prev arrow (visibility: hidden) when current === 0 and hides the next arrow when current === total - 1 | ✓ VERIFIED | `prevBtn.style.visibility = current === 0 ? 'hidden' : 'visible';` at line 1737; `nextBtn.style.visibility = current === total - 1 ? 'hidden' : 'visible';` at line 1738 |
| 9 | Arrow visibility is set on every goTo() call — including the initial goTo(0) on page load — so boundary state is always consistent | ✓ VERIFIED | Both visibility lines are unconditional inside `goTo()` body; `goTo(0)` is called at line 1741 immediately after function definition, ensuring prev arrow is hidden on load |
| 10 | Dot indicators, aria-hidden attributes, and auto-advance timer continue to function exactly as before | ✓ VERIFIED | `dots.forEach` with `aria-current` at line 1731 unchanged; `slide.setAttribute('aria-hidden', ...)` at line 1734 preserved; `setInterval(() => goTo(current + 1), 4000)` at line 1744; `touchstart`/`touchend` handlers at lines 1766/1769; mouseenter/mouseleave pause handlers at lines 1761-1763 |

**Score:** 10/10 truths verified

### Required Artifacts

| Artifact | Expected | Status | Details |
|----------|----------|--------|---------|
| `index.html` (CSS block) | Updated .hero-carousel CSS with peek geometry, opacity rules, absolute arrows, mobile overrides | ✓ VERIFIED | `.hero-carousel__slide--active` at line 367; `width: 380px` at line 348; `position: absolute` on arrow at line 385; mobile overrides at lines 994-996 |
| `index.html` (JS goTo()) | Updated goTo() with pixel-based transform, active class toggle, arrow boundary visibility | ✓ VERIFIED | `translateX(-${current * 240}px)` at line 1727; `classList.toggle('hero-carousel__slide--active', i === current)` at line 1735; prevBtn/nextBtn visibility at lines 1737-1738 |

### Key Link Verification

| From | To | Via | Status | Details |
|------|----|-----|--------|---------|
| `.hero-carousel__viewport` | `.hero-carousel__arrow` (prev/next) | `position: relative` on viewport is containing block for absolute-positioned arrows | ✓ WIRED | Viewport has `position: relative` (line 341); arrows have `position: absolute` (line 385); both buttons are DOM children of `.hero-carousel__viewport` (HTML lines 1090-1161) |
| `.hero-carousel__slide` (base rule) | `.hero-carousel__slide--active` (modifier) | BEM modifier overrides base opacity | ✓ WIRED | Base rule sets `opacity: 0.5` (line 363); modifier sets `opacity: 1` (line 368); JS toggles the class at line 1735 |
| `@media (max-width: 960px)` | `.hero-carousel__track-wrap`, `#carousel-prev`, `#carousel-next` | Mobile-specific overrides in single existing media block | ✓ WIRED | All three overrides inside the single 960px block (lines 994-996); `grep -c` confirms exactly 1 media block |
| `goTo()` translateX | `.hero-carousel__track-wrap` (380px wide) | Pixel offset 240px × index; percentage no longer maps to one slide at 380px wrap width | ✓ WIRED | `translateX(-${current * 240}px)` (line 1727); old `100%` form absent |
| `goTo()` classList.toggle | `.hero-carousel__slide--active` (CSS class) | BEM modifier toggle pattern matching dot active pattern | ✓ WIRED | Toggle call at line 1735; CSS class defined at line 367 |
| `goTo()` visibility | `#carousel-prev` / `#carousel-next` (absolute-positioned) | `visibility: hidden/visible` — no layout shift because buttons are absolute | ✓ WIRED | Both visibility assignments at lines 1737-1738; prevBtn/nextBtn declared at lines 1713-1714 |

### Data-Flow Trace (Level 4)

Not applicable. This is a purely static landing page with no server data. The carousel renders static image assets; all state (current slide index) is managed in-memory by the IIFE. No DB queries, no API calls, no dynamic data source to trace.

### Behavioral Spot-Checks

| Behavior | Command | Result | Status |
|----------|---------|--------|--------|
| Old percentage-based translateX is gone | `grep -c 'current \* 100' index.html` | 0 matches | ✓ PASS |
| Pixel-based translateX present in goTo() | `grep -c 'current \* 240}px' index.html` | 1 match (line 1727) | ✓ PASS |
| Active slide modifier defined in CSS | `grep -c '\.hero-carousel__slide--active' index.html` | 2 matches (line 367 CSS, line 1735 JS) | ✓ PASS |
| Track-wrap 380px desktop | `grep -c 'width: 380px' index.html` | 1 match (line 348) | ✓ PASS |
| Track-wrap 320px mobile (inside 960px block) | `grep -c 'width: 320px' index.html` | 1 match (line 994) | ✓ PASS |
| Arrow absolute positioning present | `grep -c 'position: absolute' index.html` (arrow rule context) | Match at line 385 in `.hero-carousel__arrow` | ✓ PASS |
| Hover transform composed (no jump) | `grep -c 'translateY(-50%) scale(1.08)' index.html` | 1 match (line 394) | ✓ PASS |
| Exactly one 960px media block | `grep -c '@media (max-width: 960px)' index.html` | 1 | ✓ PASS |
| gap: 1.5rem absent from viewport rule | Lines 340-344 contain no gap declaration | Confirmed absent | ✓ PASS |
| flex-shrink: 0 absent from arrow rule | Lines 372-389 contain no flex-shrink declaration | Confirmed absent | ✓ PASS |
| 4s auto-advance timer present | `setInterval(...4000)` at line 1744 | Confirmed | ✓ PASS |
| Touch swipe handlers present | touchstart/touchend on track at lines 1766/1769 | Confirmed | ✓ PASS |

### Requirements Coverage

| Requirement | Source Plan | Description | Status | Evidence |
|-------------|------------|-------------|--------|----------|
| CAR-01 | Plan 01 | Carousel shows partial views of the previous and next slides simultaneously on both sides of the active (centred) slide | ✓ SATISFIED | 380px track-wrap (line 348), 240px slides (line 358), 70px peek geometry; opacity dimming (lines 363-369); absolute arrows over peek strips (lines 385-407) |
| CAR-02 | Plan 02 | Carousel navigation — arrows, dot indicators, touch swipe, and auto-advance — functions correctly with the peek-view layout | ✓ SATISFIED | `goTo()` with pixel offset (line 1727), active class toggle (line 1735), boundary arrow visibility (lines 1737-1738); dot/aria-hidden/timer/touch all preserved |

No orphaned requirements — both phase 5 requirements (CAR-01, CAR-02) are claimed and verified.

### Anti-Patterns Found

| File | Line | Pattern | Severity | Impact |
|------|------|---------|----------|--------|
| index.html | 511, 729, 828 | `gap: 1.5rem` | Info | These are in other unrelated CSS rules (not `.hero-carousel__viewport`). The gap was correctly removed from the viewport rule at lines 340-344. Not a blocker. |
| index.html | 256, 675, 713, 749, 797, 913 | `flex-shrink: 0` | Info | These are in other unrelated CSS rules (not `.hero-carousel__arrow`). The flex-shrink was correctly removed from the arrow rule at lines 372-389. Not a blocker. |

No blocker anti-patterns found. The `return null` / empty array patterns were not found in any carousel-related code.

### Human Verification Required

All automated checks pass. The following items require browser verification to confirm visual and interactive behaviour:

#### 1. Peek geometry on load

**Test:** Open `index.html` in a browser. Before any interaction, inspect the carousel.
**Expected:** Slide 1 is centred at full opacity; approximately 70px of slide 2 is visible to the right; the left (prev) arrow is hidden (`visibility: hidden`); the right (next) arrow is visible.
**Why human:** CSS geometry and `visibility` state on load require a browser render. Static grep cannot verify that `translateX(0px)` on load positions slide 1 centred correctly within the 380px viewport when the track starts at the left edge.

#### 2. Opacity dimming and transition on slide change

**Test:** Click the next arrow. Observe the transition.
**Expected:** Slide 1 dims to opacity 0.5 smoothly while slide 2 rises to opacity 1.0. Both prev and next arrows are visible. Approximately 70px of slide 1 remains visible to the left; approximately 70px of slide 3 is visible to the right.
**Why human:** CSS transition behaviour and peek visibility on a non-boundary slide require visual confirmation.

#### 3. Boundary arrow hiding on last slide

**Test:** Click the next arrow three times to reach slide 4.
**Expected:** Slide 4 is at full opacity; the right (next) arrow is hidden; the left (prev) arrow is visible; approximately 70px of slide 3 is visible to the left.
**Why human:** Boundary arrow hiding at `total - 1` requires interactive testing.

#### 4. Mobile responsive layout at ≤ 960px

**Test:** Resize the browser to 800px wide (or use devtools device emulation).
**Expected:** Carousel viewport collapses to 320px; 40px peek visible on each side; arrow buttons (36px at mobile size) are centred over the 40px peek strips at 9px offset; no layout jump.
**Why human:** CSS media query breakpoint rendering requires a live browser.

#### 5. Auto-advance and touch swipe

**Test:** Wait 4 seconds without interaction, then simulate touch swipe (devtools or real device).
**Expected:** Auto-advance cycles slides with correct opacity and dot indicator updates; touch swipe advances/retreats slides; boundary arrow hiding applies correctly on mobile.
**Why human:** Timer-driven behaviour and touch event handling require real-time observation.

### Gaps Summary

No gaps found. All 10 must-have truths are verified in the codebase. Both requirements (CAR-01, CAR-02) are satisfied. The only pending items are human browser verification checks for visual layout, opacity transition quality, and interactive behaviour — these are inherent to a front-end landing page and cannot be confirmed by static analysis.

---

_Verified: 2026-05-14_
_Verifier: Claude (gsd-verifier)_
