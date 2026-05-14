# WLED xLights Mapper — Landing Page

## What This Is

A single-file static landing page (GitHub Pages) marketing the WLED to xLights Mapper iOS app. It communicates the app's core value proposition — accurate LED mapping using a standard iPhone — through feature sections, phone mockups, a how-it-works flow, and a privacy assurance section.

## Core Value

Every visitor who arrives on the page should come away understanding what the app does and be able to download it — the page must convert curiosity into App Store taps.

## Requirements

### Validated

- ✓ Full landing page content rendered — hero, features, how-it-works, export, requirements, tips, privacy, footer — existing
- ✓ Scroll-reveal entrance animations (IntersectionObserver) — existing
- ✓ Apple-inspired design system (CSS tokens, Inter typeface, frosted-glass nav) — existing
- ✓ Desktop layout — two-column feature rows, alternating section backgrounds — existing
- ✓ Phone bezel mockup component — reusable CSS pattern used across multiple sections — existing
- ✓ Static hosting on GitHub Pages, no build pipeline — existing

### Active

- [x] Mobile-responsive layout — all sections usable and readable at phone width, no content clipped or hidden *(validated Phase 1)*
- [x] Hamburger navigation — nav links accessible on mobile via a toggle menu *(validated Phase 1)*
- [x] Hero carousel — replace the horizontal fan/strip of phone mockups with a conventional sliding carousel using the same assets *(validated Phase 2)*
- [x] Contact / feedback section — styled section with a mailto CTA allowing users to send app feedback *(validated Phase 3)*

### Out of Scope

- Backend server or database — architecture must remain fully static
- JavaScript frameworks (React, Vue, etc.) — keep vanilla JS only
- Formspree / EmailJS — user decided against; mailto is sufficient for v1
- App Store link wiring — no URL provided; placeholder will remain

## Context

- Single deployable artifact: `index.html` with all CSS and JS inline
- Assets in `resources/` (screenshots, icons, two autoplay MP4 videos)
- Mobile breakpoint exists (`@media max-width: 768px`) but nav links are hidden with no hamburger fallback — mobile users currently cannot navigate
- Hero "fan" strip shows 3 phone frames side-by-side; needs to become a carousel
- Several known bugs identified by codebase mapper (double-nested notch markup, broken download CTA, alt text copy-paste errors, grammar typos) — fixing these is in scope as part of the mobile pass

## Constraints

- **Architecture:** Static HTML, single file — no build step, no bundler, no external JS libraries
- **Hosting:** GitHub Pages — serve `index.html` directly, HTTPS
- **JavaScript:** Vanilla only — `IntersectionObserver`, standard DOM APIs
- **CSS:** Inline `<style>` block only — no external stylesheets

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| mailto for contact section | No backend; Formspree/EmailJS ruled out by user | ✓ Implemented — wled.2.xlights@gmail.com with pre-filled subject |
| Carousel uses existing phone mockup assets | Avoids new design work; content already approved | ✓ Implemented — 4-slide carousel with existing screenshots/video |
| Keep single-file architecture | Constraint from user; no build pipeline | ✓ Maintained throughout all 3 phases |
| Contact before Privacy in nav/page | User preference — natural flow before legal/privacy | ✓ Applied Phase 3 |

---

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-05-14 — Phase 3 complete. Milestone v1.0 done: all 3 phases complete (mobile responsive, hero carousel, contact section)*
