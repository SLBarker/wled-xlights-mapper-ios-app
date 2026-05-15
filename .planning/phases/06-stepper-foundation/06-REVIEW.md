---
phase: 06-stepper-foundation
reviewed: 2026-05-15T11:00:48Z
depth: standard
files_reviewed: 1
files_reviewed_list:
  - index.html
findings:
  critical: 3
  warning: 2
  info: 0
  total: 5
status: issues_found
---

# Phase 6: Code Review Report

**Reviewed:** 2026-05-15T11:00:48Z
**Depth:** standard
**Files Reviewed:** 1
**Status:** issues_found

## Summary

Reviewed the phase 6 stepper-foundation changes to `index.html`: CSS stepper section (BEM rules + mobile accordion `@media`), replacement of the `.workflow` HTML block with two `.stepper` elements (10 steps total), and the JS `animatedEls` selector update from `.step-card` to `.stepper__item`.

Three blockers were found. Two are render-breaking on desktop: `display: contents` silently kills the scroll-reveal animation system for stepper items (the IntersectionObserver cannot intersect a box-less element, and `opacity`/`transform` have no effect on `display: contents` elements), so all stepper items will be permanently invisible on desktop at page load. The third blocker is a structural HTML error where `phone-bezel__notch` is placed outside `phone-bezel__screen` in all four stepper media columns, causing the notch to render outside the phone frame rather than overlaid on the screen. Two warnings cover loss of tier-pill styling (active HTML with CSS commented out) and a missing `aria-hidden` on the misplaced notch elements.

---

## Critical Issues

### CR-01: `display: contents` on `.stepper__item` makes elements invisible and un-observable by IntersectionObserver on desktop

**File:** `index.html:701-721`

**Issue:** Two `.stepper__item` rules are declared in sequence. The first (line 701) sets `opacity: 0; transform: translateY(20px)` for the scroll-reveal starting state. The second (line 719) sets `display: contents`. In CSS, `display: contents` causes the element to produce no box of its own — its children are laid out as if they were direct children of the element's parent. A consequence of having no box is that visual properties such as `opacity` and `transform` that apply to the box have no effect. Critically, `IntersectionObserver` observes the intersection of an element's border box with the viewport; an element with `display: contents` has no border box, so the observer callback's `isIntersecting` will never fire for it.

The result on desktop (>=768px): all ten `.stepper__item` elements enter the page with `opacity: 0` (from the first rule — the browser still computes this on the element even though it has no rendering box), and since the observer never fires, `.visible` is never added, so the elements are never made visible. The stepper content panels that are hard-coded `display: block` (via `.stepper__header--active + .stepper__panel`) will render, but the items themselves remain invisible.

Note: The mobile accordion path (<=767px) restores `display: block` on `.stepper__item` (line 1172), so the accordion works correctly on mobile. The bug is desktop-only.

**Fix:** Remove the scroll-reveal starting state from `.stepper__item` on desktop. The `display: contents` pattern means the items are layout-transparent; animate the child `.stepper__header` and `.stepper__panel` elements instead, or simply remove the opacity/transform from `.stepper__item` and accept that stepper items are not scroll-animated on desktop (the panels already reveal themselves via the tab CSS selector, which is sufficient):

```css
/* Remove these lines (701-710) — they have no effect with display:contents and
   prevent IntersectionObserver from ever firing for these elements */

/* .stepper__item {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.5s ease, transform 0.5s ease;
}

.stepper__item.visible {
  opacity: 1;
  transform: translateY(0);
} */

/* Desktop layout (>=768px): CSS Grid — items become layout-transparent */
.stepper {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  margin-top: 16px;
}

.stepper__item {
  display: contents;
}
```

Also update the JS selector (line 2031) to remove `.stepper__item` since those elements cannot be observed:

```js
const animatedEls = document.querySelectorAll(
  '.feature-row, .feature-card, .req-card, .tip-item, .privacy-card, .contact-cta'
);
```

---

### CR-02: `phone-bezel__notch` placed outside `phone-bezel__screen` in all four stepper media columns — notch renders in wrong position

**File:** `index.html:1620-1627, 1652-1659, 1686-1693, 1717-1724`

**Issue:** In all four stepper steps that include a phone screenshot (Connect, Scan, Report, Preview), the `.phone-bezel__notch` div is placed as a sibling of `.phone-bezel__frame` rather than as a child of `.phone-bezel__screen`. Every other phone bezel on the page follows the correct nesting order:

```
.phone-bezel
  .phone-bezel__frame
    .phone-bezel__screen
      .phone-bezel__notch   ← correct: inside screen
      <img>
```

The four stepper bezels use:

```
.phone-bezel
  .phone-bezel__notch       ← wrong: sibling of frame, not child of screen
  .phone-bezel__frame
    .phone-bezel__screen
      <img>
```

The CSS positions the notch with `position: absolute; top: 0; left: 50%` relative to its nearest positioned ancestor. `.phone-bezel__screen` has `position: relative`, which is the intended positioning context. Placed outside the screen, the notch's containing block becomes `.phone-bezel` or whatever ancestor has `position: relative` first. Additionally, `.phone-bezel__screen` has `overflow: hidden` with `border-radius: 34px` to clip screen content; the notch placed outside the screen is not clipped by this and can visually escape the frame boundary.

**Fix:** Move `.phone-bezel__notch` inside `.phone-bezel__screen` and add the missing `aria-hidden="true"` (see WR-01). Apply this fix to all four stepper media slots. Example for Step 1 (Connect), lines 1620-1627:

```html
<div class="stepper__media">
  <div class="phone-bezel">
    <div class="phone-bezel__frame">
      <div class="phone-bezel__screen">
        <div class="phone-bezel__notch" aria-hidden="true"></div>
        <img src="resources/screenshot/connect.jpeg" alt="Connect screen showing WLED controller connection UI" loading="lazy">
      </div>
    </div>
  </div>
</div>
```

Apply the same correction to the Scan (line ~1654), Report (line ~1688), and Preview (line ~1719) stepper items.

---

### CR-03: `.quality-tiers` and `.tier-pill` CSS commented out but classes still actively used in HTML

**File:** `index.html:622-654` (CSS), `index.html:1494-1499` (HTML)

**Issue:** Phase 6 commented out the entire `.quality-tiers`, `.tier-pill`, and all `.tier-*` colour modifier rules (lines 622-654). However, these classes are still referenced in the features section HTML at lines 1494-1499 — the RANSAC quality overlay sub-section of the "Features" panel, which is a visible, above-the-fold section of the page. The four tier-pill spans will lose all visual styling: no pill shape (`border-radius: 100px`), no coloured background, no colour-coded dot pseudo-element, and no gap between pills. They will render as unstyled inline text.

This HTML was not touched in phase 6 and was previously styled correctly. The CSS was commented out as part of the old `.step-card` cleanup, but `.tier-pill` was not a `.step-card` rule — it was a standalone presentational component shared between the features section and the (now-replaced) how-it-works section.

**Fix:** Restore the `.quality-tiers` and `.tier-pill` CSS rules (they are not specific to the old `.step-card` workflow and are still needed). Move them out of the commented block and keep them active, ideally near the Features section rules:

```css
/* Quality tier pills — used in Features section */
.quality-tiers {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  margin-top: 1rem;
}

.tier-pill {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.25rem 0.75rem;
  border-radius: 100px;
  font-size: 0.75rem;
  font-weight: 600;
}

.tier-pill::before {
  content: '';
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
}

.tier-green  { background: rgba(52,199,89,0.12);  color: #1a7a35; }
.tier-green::before  { background: #34c759; }
.tier-yellow { background: rgba(255,214,10,0.15); color: #7a6000; }
.tier-yellow::before { background: #ffd60a; }
.tier-orange { background: rgba(255,159,10,0.15); color: #7a4a00; }
.tier-orange::before { background: var(--warn); }
.tier-red    { background: rgba(255,59,48,0.12);  color: #7a1010; }
.tier-red::before    { background: var(--danger); }
```

---

## Warnings

### WR-01: `aria-hidden="true"` missing from `phone-bezel__notch` in all four stepper phone bezels

**File:** `index.html:1621, 1654, 1688, 1719`

**Issue:** Every other `.phone-bezel__notch` element on the page carries `aria-hidden="true"` (lines 1333, 1345, 1357, 1369, 1437, 1462, 1482, 1508, 1889 — 9 occurrences). The four stepper media bezels omit this attribute, making the decorative notch element visible to screen readers where it has no semantic meaning or label. This is a regression from the pattern established throughout the rest of the page.

**Fix:** Add `aria-hidden="true"` to each of the four stepper notch elements (fix is combined with CR-02 above):

```html
<div class="phone-bezel__notch" aria-hidden="true"></div>
```

---

### WR-02: Mobile accordion `height: auto` for first-child open state is not animatable — transition silently broken before Phase 7 JS

**File:** `index.html:1217-1219`

**Issue:** The mobile accordion CSS (line 1203-1214) sets panels to `height: 0; overflow: hidden; transition: height var(--transition)`. The comment acknowledges Phase 7 JS will manage open/close transitions. However, the "first item open by default" rule at line 1217 sets `height: auto`, which cannot be transitioned — `height: auto` is not a transitionable value in CSS. When Phase 7 JS toggles between `height: 0` and `height: auto`, the transition will silently not animate (it will snap open/closed). This is not a rendering defect today (it still opens), but the `transition: height` property declaration is effectively dead on this property pair, and it is a known trap that will cause a Phase 7 bug if the JS simply toggles a class that sets `height: auto`.

**Fix:** Plan Phase 7 JS to use `element.scrollHeight` to animate to a concrete pixel value rather than `height: auto`, or use the CSS `grid-template-rows: 0fr / 1fr` animation pattern. Document this constraint explicitly as a Phase 7 implementation requirement before it causes a bug:

```js
// Phase 7: animate open
panel.style.height = panel.scrollHeight + 'px';

// Phase 7: animate close
panel.style.height = '0';
```

Also remove the `transition: height var(--transition)` from the CSS until Phase 7 implements the animatable height pattern — leaving it in place implies it works, which it does not.

---

_Reviewed: 2026-05-15T11:00:48Z_
_Reviewer: Claude (gsd-code-reviewer)_
_Depth: standard_
