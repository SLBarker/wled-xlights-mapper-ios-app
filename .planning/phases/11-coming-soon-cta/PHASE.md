# Phase 11: Coming Soon CTA

**Milestone:** v4.1 Coming Soon CTA
**Status:** Not started
**Requirements:** CTA-01, CTA-02, CTA-03

## Goal

Visitors can immediately see that the app is not yet available — both the nav pill and hero button clearly communicate "Coming Soon" and cannot be activated.

## Scope

Two elements in `index.html`:

1. Nav pill — `<a href="#download" class="site-nav__cta">` (~line 1340) — text "Download"
2. Hero button — `<a href="#download" class="btn btn-primary" id="download">` (~line 1372) — text "Download on the App Store"

Changes required for both:
- Text content → "Coming Soon"
- Remove or neutralise `href="#download"` so no scroll/navigation fires on click
- CSS disabled state: reduced opacity, `cursor: not-allowed`, hover styles suppressed
- Accessibility: `aria-disabled="true"` pattern; element remains focusable (do NOT remove tabindex)

## Success Criteria

1. Nav Download pill shows "Coming Soon" text at all viewport widths
2. Hero Download button shows "Coming Soon" text
3. Clicking or tapping either button does not navigate, scroll, or produce any action
4. Both buttons appear visually distinct from active/clickable buttons (reduced opacity, cursor: not-allowed, hover animation suppressed)
5. Both buttons remain keyboard-focusable — no accessibility regression (aria-disabled pattern preferred over removing tabindex)

## Plans

- TBD (run `/gsd:plan-phase 11`)
