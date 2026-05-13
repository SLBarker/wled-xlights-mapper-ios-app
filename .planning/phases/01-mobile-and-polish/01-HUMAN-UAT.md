---
status: partial
phase: 01-mobile-and-polish
source: [01-VERIFICATION.md]
started: 2026-05-13T00:00:00Z
updated: 2026-05-13T00:00:00Z
---

## Current Test

[awaiting human testing]

## Tests

### 1. Hamburger nav at 375px
expected: Hamburger button (3-bar icon) visible in nav bar at 375px viewport. Tapping opens a vertical drawer with nav links. Tapping again closes it. Pressing Escape closes it and returns focus to the hamburger button.
result: [pending]

### 2. No layout overflow at mobile widths
expected: All sections readable at 375px–768px with no clipped or overflowing content. Feature cards, workflow steps, requirements, and privacy cards all display as single-column.
result: [pending]

### 3. Zero .mp4 requests on initial page load
expected: DevTools Network tab (filter: media/mp4) shows no .mp4 requests on hard refresh before any scrolling.
result: [pending]

### 4. Videos lazy-load and autoplay correctly
expected: discovery.mp4 begins loading when scrolled within ~200px of the discovery feature row. 3dpreview.mp4 begins loading when scrolled within ~200px of the preview feature row. Both autoplay (looping, muted) once loaded.
result: [pending]

## Summary

total: 4
passed: 0
issues: 0
pending: 4
skipped: 0
blocked: 0

## Gaps
