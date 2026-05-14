# Project Retrospective

*A living document updated after each milestone. Lessons feed forward into future planning.*

---

## Milestone: v1.0 — MVP

**Shipped:** 2026-05-14
**Phases:** 3 | **Plans:** 7 | **Timeline:** 1 day (2026-05-13 → 2026-05-14)

### What Was Built

- Mobile hamburger nav with ARIA, focus management, and keyboard support
- Full responsive layout — single-column grids, reduced padding at ≤768px
- 13.3 MB of autoplay video deferred from page load via IntersectionObserver `data-src` pattern
- Replaced fan-strip hero with 4-slide carousel (arrows, dots, swipe, 4s auto-advance)
- Contact/Feedback section with pre-filled mailto CTA
- 5 DOM/copy bugs from initial codebase audit resolved

### What Worked

- Wave-based plan execution kept individual plan scope tight — each plan was completable in under 15 min
- Splitting CSS and HTML/JS into separate plans (02-01 CSS / 02-02 HTML+JS; 03-01 CSS / 03-02 HTML) reduced risk: markup work had stable class names before it ran
- SUMMARY.md files captured decisions and deviations immediately, so context was always fresh
- Human verification checkpoint on Phase 2 carousel passed on first attempt — no rework
- Single-file architecture constraint simplified the execution model: one file, one deploy artifact, no dependency graph

### What Was Inefficient

- Phase 1 acceptance criteria for `grep -c "it's"` could not be fully satisfied due to substring match against "ARKit's" — a documentation error in the criterion, not the implementation. Verification criteria should use more targeted grep patterns
- Phase 3 plan's `grep -c "href=\"#contact\""` count annotation was wrong (said 3, should be 2) — another criterion documentation error
- `discovery.mov` (4.1 MB unused asset) was identified early but never cleaned up — should have been in scope for Phase 1 housekeeping

### Patterns Established

- `data-src` lazy video pattern: replace `src` with `data-src` on video elements; IntersectionObserver writes `src` and calls `video.load()` with `rootMargin: '200px'`
- Hamburger nav pattern: `.site-nav__toggle` button + `.site-nav__drawer` sibling div (not a mutation of the desktop `.site-nav__links`) — keeps desktop nav independent
- Carousel extend pattern: add `.hero-carousel__slide` div to HTML only; JS auto-discovers via `querySelectorAll`
- CSS/HTML plan split: define all class names in a CSS-only plan first, then reference them confidently in the markup/JS plan
- Three co-existing IntersectionObserver IIFEs: animation observer (`io`), hamburger nav (none), lazy video (`lazyVideoObserver`) — IIFE scoping prevents naming conflicts

### Key Lessons

1. Acceptance criteria grep commands need to account for substring matches — test the grep pattern against the file before writing it as a criterion
2. Unused assets should be cleaned up in the same phase where they're identified, not deferred
3. The CSS-first / HTML-JS-second plan split for UI features is worth the extra plan — it eliminates style-flash risk and keeps each plan focused on one concern

### Cost Observations

- Very low cost milestone — single HTML file, no dependencies, no build pipeline
- Phases were short (1–15 min/plan) due to constrained scope
- No model escalation needed — standard sonnet throughout

---

## Cross-Milestone Trends

| Metric | v1.0 |
|--------|------|
| Phases | 3 |
| Plans | 7 |
| Timeline | 1 day |
| Requirements shipped | 12/12 (100%) |
| Plan rework needed | 0 |
| Human checkpoints passed first attempt | 3/3 |
