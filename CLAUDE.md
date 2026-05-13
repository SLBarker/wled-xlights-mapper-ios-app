# WLED xLights Mapper — Landing Page

## Project

Single-file static landing page for the WLED to xLights Mapper iOS app. Deployed on GitHub Pages.

**Stack:** HTML5 / CSS3 (inline) / Vanilla JS (inline) — no build pipeline, no frameworks.
**Deployable artifact:** `index.html` (all styles and scripts must remain inline).

## Architecture Constraints

- **No external stylesheets** — all CSS stays in the `<style>` block in `<head>`
- **No JavaScript frameworks** — vanilla JS only (`IntersectionObserver`, standard DOM APIs)
- **No build step** — `index.html` must be directly servable; no compilation or bundling
- **Static hosting** — GitHub Pages; no server-side logic
- **Single file** — do not split into separate `.css` or `.js` files

## Current State

Three improvement phases planned:

| Phase | Goal | Status |
|-------|------|--------|
| 1 | Mobile responsive + hamburger nav + bug fixes + lazy video | Not started |
| 2 | Hero carousel (replace fan/strip) | Not started |
| 3 | Contact/Feedback section with mailto CTA | Not started |

## GSD Workflow

This project uses GSD (Get Shit Done) for structured execution.

- Planning artifacts: `.planning/`
- Requirements: `.planning/REQUIREMENTS.md`
- Roadmap: `.planning/ROADMAP.md`
- State: `.planning/STATE.md`

**Next step:** `/gsd-discuss-phase 1` or `/gsd-plan-phase 1`

## Key Files

- `index.html` — entire landing page (styles, markup, scripts)
- `resources/screenshot/` — app screenshots (JPEG)
- `resources/video/` — autoplay demo videos (MP4)
- `resources/icons/` — UI icon images (PNG)
- `.planning/codebase/` — codebase analysis documents
