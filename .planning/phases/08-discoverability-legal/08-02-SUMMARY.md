---
phase: 08-discoverability-legal
plan: 02
subsystem: privacy.html, index.html
tags: [privacy-policy, legal, footer-link, standalone-page, App-Store-compliance]
dependency_graph:
  requires: [08-01]
  provides: [privacy-policy-page, footer-privacy-link, LEGL-01]
  affects: [privacy.html, index.html]
tech_stack:
  added: []
  patterns: [frosted-glass nav, inline CSS single-file page, mailto link]
key_files:
  created:
    - privacy.html
  modified:
    - index.html
decisions:
  - Used inline style on mailto link (color var(--accent)) rather than a CSS class — keeps the single rule co-located with the element and avoids adding a class to privacy.html's minimal stylesheet
  - Canonical URL points to full GitHub Pages path for privacy.html — matches App Store URL requirement
  - Effective date set to 18 May 2026 per plan specification
metrics:
  duration: ~8 minutes
  completed_date: "2026-05-18"
  tasks_completed: 2
  tasks_pending_human: 0
---

# Phase 8 Plan 2: Privacy Policy Page and Footer Link Summary

Standalone privacy policy page (privacy.html) created with frosted-glass nav and 6 prose sections, plus a Privacy Policy footer link wired into index.html as its last footer child — satisfying LEGL-01 (App Store compliance).

## Tasks Completed

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Create privacy.html in repo root | 8455f03 | privacy.html |
| 2 | Add Privacy Policy footer link to index.html | 9b168c4 | index.html |

## Changes Made

### privacy.html (created)

Complete standalone HTML5 page — all CSS inline in a single `<style>` block:

- `<meta charset>`, viewport, `<title>Privacy Policy — WLED to xLights Mapper</title>`
- `<link rel="canonical" href="https://slbarker.github.io/wled-xlights-mapper-ios-app/privacy.html">`
- Google Fonts preconnect + Inter link (same two tags as index.html)
- CSS: reset, `:root` tokens (copied verbatim from index.html), body rule, frosted-glass `.site-nav`, `.site-nav__inner`, `.site-nav__logo`, `.site-nav__back` with hover and focus-visible states, main layout (max-width 760px), h1 and `.effective-date`, `.privacy-section` with h2 and p rules, dark footer
- Nav: app name (left) + "← Back to landing page" link (right) with `aria-label`
- `<h1>Privacy Policy</h1>` + `<p class="effective-date">Effective: 18 May 2026</p>`
- 6 `<section class="privacy-section">` elements: No Server Communication, No Personal Data Storage, No Analytics or Tracking, No Internet Requirement, Camera — Local Processing Only, Contact
- Contact section: `<a href="mailto:wled.2.xlights@gmail.com">` styled with `color: var(--accent)`
- Minimal `<footer role="contentinfo">` with app name only

### index.html (modified)

One line added — last child of `<footer role="contentinfo">`:

```html
<p style="margin-top:0.5rem;"><a href="privacy.html" aria-label="Privacy Policy for WLED to xLights Mapper">Privacy Policy</a></p>
```

The `<a>` inherits `color: rgba(255,255,255,0.35)` from `footer p` and picks up `footer a`, `footer a:hover`, and `footer a:focus-visible` rules added in Plan 01 Task 1.

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — all 6 privacy sections are fully wired with prose content expanded from the index.html privacy cards. Contact email is live. Effective date is set. No placeholder content.

## Threat Flags

None — no new network endpoints, auth paths, or schema changes introduced. All threats reviewed in plan threat model; all dispositioned as `accept`. The mailto: link discloses a public developer contact email (T-08-05: accepted). Google Fonts CDN is same origin as index.html (T-08-06: accepted). Footer href is relative same-origin navigation only (T-08-07: accepted).

## Self-Check: PASSED

- [x] privacy.html exists at repo root — commit 8455f03
- [x] index.html modified with footer link — commit 9b168c4
- [x] `grep -c 'class="privacy-section"' privacy.html` → 6
- [x] `grep -c 'class="site-nav__back"' privacy.html` → 1 (HTML element; CSS rules also reference the class)
- [x] `grep -c 'rel="canonical"' privacy.html` → 1
- [x] `grep -c 'wled.2.xlights@gmail.com' privacy.html` → 1
- [x] `grep -c '<style>' privacy.html` → 1
- [x] `grep -c 'backdrop-filter: blur(20px)' privacy.html` → 2 (vendor prefix + standard)
- [x] `grep -c 'href="privacy.html"' index.html` → 1
- [x] `grep -c 'aria-label="Privacy Policy for WLED to xLights Mapper"' index.html` → 1
- [x] Post-commit deletion check: no tracked files deleted
