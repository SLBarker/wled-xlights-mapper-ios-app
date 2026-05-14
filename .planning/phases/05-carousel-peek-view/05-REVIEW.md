---
phase: 05-carousel-peek-view
reviewed: 2026-05-14T18:01:50Z
depth: standard
files_reviewed: 1
files_reviewed_list:
  - index.html
findings:
  critical: 0
  warning: 3
  info: 2
  total: 5
status: issues_found
---

# Phase 5: Code Review Report

**Reviewed:** 2026-05-14T18:01:50Z
**Depth:** standard
**Files Reviewed:** 1
**Status:** issues_found

## Summary

Reviewed all phase 5 changes: the carousel peek-view CSS (track-wrap widened to 380px / 320px mobile, slide opacity/active modifier, arrow conversion to `position:absolute` with composed hover transform) and the JS controller (`goTo()` pixel offset, active class toggle, boundary arrow visibility).

The CSS changes are structurally sound. The absolute-arrow pattern is correct — arrows are positioned within `.hero-carousel__viewport` which carries `position: relative`, and they are siblings of the track-wrap so `overflow:hidden` on the track-wrap does not clip them. The hover transform compose (`translateY(-50%) scale(1.08)`) is correct.

Three issues require attention: autoplay silently wraps past a boundary that the UI signals as a dead-end (BLOCKER-level UX contract break), the initial HTML slide state is missing `aria-hidden` attributes that `goTo(0)` sets only after JavaScript runs (leaves a bad pre-JS state), and on mobile the absolute arrows overlap meaningfully into the active slide area.

---

## Warnings

### WR-01: Autoplay timer silently wraps at last slide, contradicting hidden-next-button contract

**File:** `index.html:1737-1744`

**Issue:** `nextBtn.style.visibility = 'hidden'` is set when `current === total - 1`, signalling to the user there is no next slide. However, the autoplay timer calls `goTo(current + 1)` unconditionally every 4 seconds. When on the last slide, the timer fires `goTo(4)`, which wraps to slide 0 via the modulo guard. The user sees the carousel abruptly jump back to slide 1 even though the Next button is invisible. The hidden button communicates "this is the end" but the timer contradicts it.

If the design intent is a linear (non-looping) carousel, the timer should stop at the boundary. If the intent is a looping carousel, the boundary buttons should not be hidden — they should loop too.

**Fix (linear carousel — stop timer at boundary):**
```js
function startTimer() {
  if (current < total - 1) {
    timer = setInterval(() => {
      if (current < total - 1) {
        goTo(current + 1);
      } else {
        stopTimer();
      }
    }, 4000);
  }
}
```

**Fix (looping carousel — remove boundary visibility hide):**
```js
// In goTo(), replace the boundary visibility lines with:
prevBtn.style.visibility = 'visible';
nextBtn.style.visibility = 'visible';
// (remove the conditional entirely)
```

---

### WR-02: Slides have no `aria-hidden` in static HTML; pre-JS state exposes all slides to screen readers

**File:** `index.html:1103-1149`

**Issue:** The four `.hero-carousel__slide` elements in the HTML have no `aria-hidden` attribute. `goTo(0)` sets `aria-hidden="true"` on slides 1–3 and `aria-hidden="false"` on slide 0, but this only runs after the JavaScript IIFE executes. In the instant between DOM-ready and script execution (and in any no-JS environment), all four slides are equally exposed to assistive technology, and a screen reader will read out all four "1 of 4", "2 of 4", "3 of 4", "4 of 4" labels in sequence with no indication that this is a navigable carousel.

**Fix:** Add `aria-hidden="true"` to slides 1–3 in the static HTML so the initial state matches the post-JS state:
```html
<div class="hero-carousel__slide" role="group" aria-roledescription="slide"
     aria-label="1 of 4: Connect">
  ...
</div>
<div class="hero-carousel__slide" role="group" aria-roledescription="slide"
     aria-label="2 of 4: Scan" aria-hidden="true">
  ...
</div>
<div class="hero-carousel__slide" role="group" aria-roledescription="slide"
     aria-label="3 of 4: Preview &amp; Export" aria-hidden="true">
  ...
</div>
<div class="hero-carousel__slide" role="group" aria-roledescription="slide"
     aria-label="4 of 4: Results" aria-hidden="true">
  ...
</div>
```

---

### WR-03: Mobile arrow positions overlap the active slide's phone bezel

**File:** `index.html:993-996`

**Issue:** On mobile, the track-wrap is 320px wide and slides are 240px wide, leaving 80px of peek space split across both sides (40px each if centered, but the track is left-aligned, so the 80px peek shows entirely on the right). The arrows are positioned at `left: 9px` and `right: 9px` relative to `.hero-carousel__viewport`. The viewport's width is auto-sized to its content (the 320px track-wrap), so the arrow centers sit at roughly 9px + 18px = 27px from each edge of the track-wrap — directly overlapping the first and last ~27px of the visible 240px slide. This places the Prev arrow on top of the left edge of the phone bezel for slide 1 (and similarly for the Next arrow on slide 4).

Desktop avoids this because 380 − 240 = 140px of peek space provides 70px per side for the arrows to occupy naturally, but 44px-wide arrows at `left: 13px` (center at 35px) still overlap the active slide by 35 − 0 = 35px. The overlap is intentional by design, but at mobile sizes the arrows at 36px width with `left: 9px` (center at 27px) cut significantly into the 240px slide window.

**Fix:** On mobile, increase the left/right offsets to push arrow centers outside the track content, or reduce the arrow size further and adjust offsets:
```css
@media (max-width: 960px) {
  #carousel-prev { left: -10px; }  /* push partially outside viewport */
  #carousel-next { right: -10px; }
}
```
Alternatively, accept the overlap as a design choice and verify it visually — but document it explicitly if intentional.

---

## Info

### IN-01: `aria-current="false"` set on inactive dots — remove attribute instead

**File:** `index.html:1731`

**Issue:** `dot.setAttribute('aria-current', 'false')` is called for non-active dots. The WAI-ARIA specification notes that `aria-current="false"` is valid but semantically equivalent to the attribute being absent; some assistive technology implementations announce "false" explicitly. The recommended pattern for non-current items is to remove the attribute entirely rather than setting it to `false`.

**Fix:**
```js
dots.forEach((dot, i) => {
  const active = i === current;
  dot.classList.toggle('hero-carousel__dot--active', active);
  if (active) {
    dot.setAttribute('aria-current', 'true');
  } else {
    dot.removeAttribute('aria-current');
  }
});
```

---

### IN-02: `transition: var(--transition)` on arrow applies to all properties

**File:** `index.html:384`

**Issue:** `.hero-carousel__arrow { transition: var(--transition); }` — because no CSS property name is specified before `var(--transition)`, this is equivalent to `transition: all 0.35s cubic-bezier(...)`. This means any property change on the arrow (including `visibility`, `outline`, `box-shadow`, etc.) will be animated. This is unlikely to cause a visible bug here since only `background`, `color`, and `transform` are changed on interaction, but it is imprecise and matches a broader pattern than intended.

**Fix:** Specify the animated properties explicitly:
```css
.hero-carousel__arrow {
  transition: background var(--transition), color var(--transition), transform var(--transition);
}
```

---

_Reviewed: 2026-05-14T18:01:50Z_
_Reviewer: Claude (gsd-code-reviewer)_
_Depth: standard_
