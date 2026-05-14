---
phase: 04-nav-download-cta
reviewed: 2026-05-14T12:43:48Z
depth: standard
files_reviewed: 1
files_reviewed_list:
  - index.html
findings:
  critical: 0
  warning: 2
  info: 2
  total: 4
status: issues_found
---

# Phase 4: Code Review Report

**Reviewed:** 2026-05-14T12:43:48Z
**Depth:** standard
**Files Reviewed:** 1
**Status:** issues_found

## Summary

This phase extracted the Download CTA from the `.site-nav__links` list into a direct flex child of `.site-nav__inner`, updated the hover style, added an SVG icon, and shifted the mobile breakpoint from 768px to 960px. All six plan requirements are implemented and functionally correct.

Two warnings were found: a `text-decoration` regression introduced by the structural move (the CTA anchor previously inherited `text-decoration: none` from `.site-nav__links a`; it no longer does), and unnecessary `!important` flags on properties that no longer need them after the structural change. Two pre-existing info-level defects are noted.

## Warnings

### WR-01: `text-decoration: none` lost on nav CTA after structural move

**File:** `index.html:110`
**Issue:** In the base branch, `.site-nav__cta` sat inside `.site-nav__links` and inherited `text-decoration: none` from the `.site-nav__links a` rule (line 98). The phase moved the CTA element outside that list — it is now a bare `<a>` that is a direct flex child of `.site-nav__inner`. No `text-decoration: none` declaration exists on `.site-nav__cta` itself. In most browsers, `<a>` elements render with `text-decoration: underline` by default. Because the link text colour is `#fff !important` on a blue background the underline is typically invisible at rest, but on hover (translateY lift, no explicit underline control) it may appear, and it will be present for keyboard-focused states where the browser paints a visible focus ring. This is a behaviour regression.

**Fix:** Add `text-decoration: none;` to the `.site-nav__cta` rule:

```css
.site-nav__cta {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  background: var(--accent);
  color: #fff !important;
  border-radius: 20px;
  padding: 0.4rem 1rem !important;
  text-decoration: none;
  transition: transform var(--transition), box-shadow var(--transition) !important;
}
```

---

### WR-02: `!important` on `color`, `padding`, and `transition` in `.site-nav__cta` is now unnecessary

**File:** `index.html:115-118`
**Issue:** `color: #fff !important`, `padding: 0.4rem 1rem !important`, and `transition: ... !important` were carried forward from the base branch where the CTA was an `<a>` inside a `<li>` inside `.site-nav__links`. Those `!important` flags existed to override `.site-nav__links a { color: var(--muted); }` and similar rules. After the structural move, `.site-nav__cta` is no longer a descendant of `.site-nav__links` — no conflicting rules exist. The `!important` flags now serve no purpose and will suppress any legitimate future overrides (for example, a dark-mode rule or a hover override on `color` or `transition`). The `transition: !important` is particularly harmful because it would prevent any JS-applied inline transition from taking effect.

**Fix:** Remove `!important` from all three declarations:

```css
.site-nav__cta {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  background: var(--accent);
  color: #fff;
  border-radius: 20px;
  padding: 0.4rem 1rem;
  text-decoration: none;
  transition: transform var(--transition), box-shadow var(--transition);
}
```

---

## Info

### IN-01: Missing `focus-visible` style on nav CTA

**File:** `index.html:110`
**Issue:** All other interactive nav elements have explicit `:focus-visible` rules: `.site-nav__links a:focus-visible` (line 103), `.hero-carousel__arrow:focus-visible` (line 389), `.site-nav__toggle:focus-visible` (line 896), and `.site-nav__drawer ul a:focus-visible` (line 952). The new `.site-nav__cta` element has no `:focus-visible` rule — keyboard users tabbing to the Download button will see an inconsistent or browser-default focus indicator rather than the styled `outline: 2px solid var(--accent)` used everywhere else.

**Fix:** Add a `:focus-visible` rule adjacent to the existing `:hover` rule:

```css
.site-nav__cta:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

---

### IN-02: Duplicate `.site-nav__drawer ul a:focus-visible` CSS rule (pre-existing)

**File:** `index.html:952-961`
**Issue:** There are two separate `.site-nav__drawer ul a:focus-visible` rules. The first (line 952) sets `color: var(--accent); background: rgba(0,113,227,0.07); outline: none;`. The second (line 958) sets `outline: 2px solid var(--accent); outline-offset: -2px;`. The second rule overrides `outline: none` from the first, making the first rule's `outline: none` declaration dead. Both rules were present before this phase and were not touched by it. The code is functionally correct (the browser merges them) but the duplication is misleading and the `outline: none` in the first block is never effective.

**Fix:** Merge into a single rule:

```css
.site-nav__drawer ul a:focus-visible {
  color: var(--accent);
  background: rgba(0,113,227,0.07);
  outline: 2px solid var(--accent);
  outline-offset: -2px;
}
```

---

_Reviewed: 2026-05-14T12:43:48Z_
_Reviewer: Claude (gsd-code-reviewer)_
_Depth: standard_
