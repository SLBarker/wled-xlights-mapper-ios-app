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

---

## Milestone: v3.0 — Interactive Workflow

**Shipped:** 2026-05-15
**Phases:** 2 | **Plans:** 4 | **Timeline:** 1 day (2026-05-15)

### What Was Built

- BEM stepper CSS: 5-column CSS Grid rail on desktop (display:contents on items), height:0 accordion on mobile
- HTML restructure: two .stepper blocks with 10 .stepper__item elements replace the old .workflow div; phone bezel screenshots wired to In the App steps 1–4
- JS stepper controller: click-to-expand with mobile height animation, adjacent-step collapse, ARIA state management
- Fire-once IntersectionObserver auto-expands Step 1 on viewport entry (threshold 0.15) without re-triggering
- Arrow key focus cycling within each stepper container (direction-aware desktop/mobile)
- Post-close: mobile chevron toggle, fully-collapsed start state, desktop↔mobile resize boundary sync

### What Worked

- Phase split (CSS/HTML in Phase 6, JS in Phase 7) was clean — Phase 7 could treat the DOM as stable; no markup changes were needed when wiring JS
- Two-IIFE structure (click handler separate from auto-expand/keyboard) made each script independently committable and verifiable
- Human verification plan (07-02) caught two real bugs that grep verification could not: height:'' → height:auto regression, and padding-before-measure requirement for jump-free animation
- The BEM naming convention established in Phase 6 made Phase 7 zero-friction — all selector targets were pre-defined

### What Was Inefficient

- STEP-02/03/04 requirements in REQUIREMENTS.md were not ticked off after Phase 7 shipped — milestone close required manual reconciliation
- VERIFICATION.md files not updated to "passed" after human sign-off in 07-02-SUMMARY.md — creates audit debt that gets flagged at close
- Post-close enhancements (chevron toggle, collapsed start, resize sync) were unplanned but necessary — could have been included in the Phase 7 plan with a more thorough mobile UX review upfront

### Patterns Established

- `display:contents` stepper pattern: items are layout-transparent; headers land in grid-row:1, panels span grid-column:1/-1 in grid-row:2 via explicit declarations
- Adjacent-sibling CSS for tab visibility: `.stepper__header--active + .stepper__panel { display: block }` works through display:contents
- openPanel/closePanel height animation: snapshot scrollHeight AFTER setting padding, animate to px, then set to 'auto' on transitionend (NOT '' — that falls back to CSS height:0)
- Fire-once IntersectionObserver: `unobserve(entry.target)` immediately after firing, before any state changes
- IIFE-per-concern: each independent behavior (click, auto-expand, keyboard) gets its own IIFE — scoped, committable, testable independently

### Key Lessons

1. Require VERIFICATION.md and REQUIREMENTS.md updates as part of the final plan's acceptance criteria — not as separate follow-up work
2. Mobile UX review should happen during discuss-phase, not post-execution — the chevron toggle and collapsed-start were obvious mobile UX improvements that weren't in scope until they were needed
3. When a human verification plan finds bugs, the VERIFICATION.md should be updated to reflect passed status within that same plan

### Cost Observations

- Very fast milestone — 4 plans, 1 day, all within sonnet context
- JS bugs found in human verification (not grep) confirm value of the human checkpoint plan for animation-dependent features

---

## Cross-Milestone Trends

| Metric | v1.0 | v3.0 |
|--------|------|------|
| Phases | 3 | 2 |
| Plans | 7 | 4 |
| Timeline | 1 day | 1 day |
| Requirements shipped | 12/12 (100%) | 8/8 (100%) |
| Plan rework needed | 0 | 0 |
| Human checkpoints passed first attempt | 3/3 | 1/1 |
| Bugs found in human verification | 0 | 2 |
