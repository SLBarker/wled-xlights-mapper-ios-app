---
phase: 03-contact-section
reviewed: 2026-05-14T00:00:00Z
depth: standard
files_reviewed: 1
files_reviewed_list:
  - index.html
findings:
  critical: 0
  warning: 3
  info: 1
  total: 4
status: issues_found
---

# Phase 3: Code Review Report

**Reviewed:** 2026-05-14
**Depth:** standard
**Files Reviewed:** 1
**Status:** issues_found

## Summary

Phase 3 adds a Contact section (CSS, HTML, mailto CTA) and wires it into both nav lists and the IntersectionObserver. The core deliverables are structurally sound: the mailto URI is correctly encoded, the section uses proper landmark semantics, and the scroll-reveal JS selector is valid. Three warnings were found — one layout defect that will visually mis-center the paragraph on wide screens, one missing background declaration that breaks the established section alternation pattern, and one aria-controls mismatch on the hamburger toggle that is exposed by the new mobile nav entry (pre-existing bug, but the phase 3 mobile nav addition makes it more consequential). No security vulnerabilities or data-loss risks were identified.

## Warnings

### WR-01: `section-body` paragraph is not horizontally centred inside the centred contact section

**File:** `index.html:1570` (CSS rule at line 148-153)

**Issue:** `#contact { text-align: center; }` centres text content but the `<p class="section-body">` block element has `max-width: 640px` with no `margin: 0 auto`. On viewports wider than 640px the paragraph box sits flush-left within the container while its text is centred inside those 640px. The net result is that the text block appears left-of-centre relative to the section: the left edge of the text is at `x=0` of the container but the right edge ends at `x=640px`, making centred text float visually to the left on desktop.

**Fix:** Add `margin: 0 auto` to `.section-body` in the `<style>` block, or add a one-off inline style on the element:

```html
<!-- Option A — preferred: fix the shared rule (line 151) -->
.section-body {
  font-size: 1.0625rem;
  color: var(--muted);
  max-width: 640px;
  line-height: 1.75;
  margin: 0 auto;   /* add this */
}

<!-- Option B — scoped to contact only, no side-effects -->
<p class="section-body" style="margin: 0 auto;">Found a bug or have a feature request? I'd love to hear from you.</p>
```

Note: Option A affects `section-body` usage in other sections (`#pitch`, `#export`) — verify those sections are not adversely affected (those sections use `section-body` inside a two-column grid where auto-margin would have no effect, so Option A is safe).

---

### WR-02: `#contact` has no explicit background colour — breaks the section alternation pattern

**File:** `index.html:839`

**Issue:** Every other section in the page declares an explicit background:

| Section | Background |
|---------|-----------|
| `#pitch` | `var(--surface)` (#fff) |
| `#features` | `var(--bg)` (#f5f5f7) |
| `#how-it-works` | `var(--surface)` |
| `#export` | `var(--bg)` |
| `#requirements` | `var(--surface)` |
| `#tips` | `var(--bg)` |
| `#privacy` | `#0a0a0f` (dark) |
| `#contact` | **none — inherits `var(--bg)` from `body`** |
| `footer` | `#0a0a0f` (dark) |

`#contact` implicitly gets `var(--bg)` = `#f5f5f7` (light grey). The sequence before and after is dark(`#privacy`) → grey(`#contact`) → dark(`footer`). While this may be the intended design, the omission is out of pattern with every other section and is likely an oversight rather than a deliberate choice. If the section is meant to be `var(--surface)` (white) to break from the privacy dark section, that needs to be made explicit.

**Fix:**
```css
/* line 839 — add background to the existing rule */
#contact {
  text-align: center;
  background: var(--surface);   /* or var(--bg) if grey is intended */
}
```

---

### WR-03: `aria-controls` on the hamburger toggle points to the desktop nav, not the mobile drawer it actually controls

**File:** `index.html:1014`

**Issue:** The hamburger toggle button has `aria-controls="site-nav-menu"` (line 1014), which references the desktop `<ul id="site-nav-menu">` (the list that is hidden via `display:none` at mobile breakpoints). The element the toggle actually shows and hides at runtime is the drawer `<div id="site-nav-drawer">`. This means screen readers are told the button controls a hidden-and-irrelevant element rather than the drawer that contains the Phase 3 "Contact" entry and all other mobile nav links. This was a pre-existing bug; it is included here because the Phase 3 mobile nav addition (`<li><a href="#contact">Contact</a></li>` at line 1036) adds a user-visible entry to the drawer, making the mismatch more consequential for assistive technology users trying to navigate to the Contact section on mobile.

**Fix:**
```html
<!-- line 1014: change aria-controls to point to the drawer -->
<button
  class="site-nav__toggle"
  aria-label="Open navigation menu"
  aria-expanded="false"
  aria-controls="site-nav-drawer"   <!-- was "site-nav-menu" -->
  type="button">
```

---

## Info

### IN-01: Unused `i` parameter in IntersectionObserver `forEach` callback

**File:** `index.html:1591`

**Issue:** `entries.forEach((entry, i) => {` — the `i` index parameter is declared but never referenced in the callback body. This is a pre-existing artifact (the original implementation may have used `i` for stagger before the sibling-index approach was adopted). Not introduced by Phase 3, but the Phase 3 `.contact-cta` addition makes this the right time to clean it up.

**Fix:**
```js
entries.forEach((entry) => {   // remove unused `i`
```

---

_Reviewed: 2026-05-14_
_Reviewer: Claude (gsd-code-reviewer)_
_Depth: standard_
