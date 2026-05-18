# Milestones: WLED xLights Mapper — Landing Page

---

## v1.0 MVP — SHIPPED 2026-05-14

**Phases:** 3 (Phases 1–3) | **Plans:** 7 | **Timeline:** 2026-05-13 → 2026-05-14 (1 day)

**Delivered:** Transformed a desktop-only, bug-riddled landing page into a polished, mobile-first page with a sliding hero carousel and a direct user-feedback channel — all within a single-file static architecture.

### Accomplishments

1. Fixed 5 nested DOM bugs, corrected all alt text and aria-labels, resolved grammar typos
2. Built mobile hamburger nav with ARIA, focus management, keyboard support, and responsive single-column grids
3. Deferred 13.3 MB of autoplay video from page load via IntersectionObserver lazy loading
4. Replaced fan-strip hero with full BEM carousel CSS design system using design tokens
5. Delivered 4-slide hero carousel with arrows, dot indicators, touch swipe, and 4s auto-advance
6. Added Contact/Feedback section with pre-filled mailto CTA — all 3 CONT requirements satisfied

### Stats

- Requirements shipped: 12/12 (100%)
- Files changed: 27 | Lines added to index.html: ~590 (1,165 → 1,755)
- Git commits: ~30

### Archive

- [Roadmap archive](.planning/milestones/v1.0-ROADMAP.md)
- [Requirements archive](.planning/milestones/v1.0-REQUIREMENTS.md)

### Notes

- No milestone audit run — milestone closed with acknowledged gaps check skipped. Human UAT checkpoint passed on all 3 phases.
- App Store URL still unavailable; `#download` CTA remains placeholder.

---

## v4.0 Discoverability, Compliance & Performance — SHIPPED 2026-05-18

**Phases:** 3 (Phases 8–10) | **Plans:** 5 | **Timeline:** 2026-05-18 (1 day)

**Delivered:** Made the landing page findable, legally sound, and CDN-free — Open Graph social sharing, favicon, privacy policy, robots/sitemap crawl infrastructure, and self-hosted Inter fonts replacing the Google Fonts CDN dependency. All 5 v4.0 requirements satisfied.

### Accomplishments

1. Open Graph + Twitter Card meta tags with og-image.png — rich social preview when sharing URL in iMessage, Slack, Twitter, etc.
2. WX monogram SVG/PNG favicon + Apple touch icon — visible in browser tabs and iOS home-screen bookmarks
3. Standalone privacy.html + footer link in index.html — App Store compliance requirement satisfied (LEGL-01)
4. robots.txt opened to all crawlers, sitemap.xml created, sitemap discovery link added to index.html head
5. 4 Inter Latin WOFF2 fonts (~24KB each, ~96KB total) committed to resources/fonts/ — Google Fonts CDN removed from index.html entirely
6. @font-face declarations with font-display: swap replace Google Fonts `<link>` tags — zero outbound CDN requests on page load

### Stats

- Requirements shipped: 5/5 (100%)
- Files changed: 29 | Net lines: +1618 / -32 | index.html: 2581 lines
- Git commits: 32
- Known deferred items at close: 8 (UAT gaps + human_needed verification gaps — code correct in all cases)

### Archive

- [Roadmap archive](.planning/milestones/v4.0-ROADMAP.md)
- [Requirements archive](.planning/milestones/v4.0-REQUIREMENTS.md)

### Notes

- All 5 v4.0 requirements delivered (DISC-01, DISC-02, DISC-03, LEGL-01, PERF-02)
- CONT-01 (App Store URL) and PERF-03 (video compression) deferred to future milestone — both blocked on external dependencies
- privacy.html still loads Inter from Google Fonts CDN — only index.html was targeted by PERF-02; acceptable given negligible privacy-page traffic
- 8 open artifact items acknowledged at close: all are human browser-test/visual checks, not code gaps

---

## v3.0 Interactive Workflow — SHIPPED 2026-05-15

**Phases:** 2 (Phases 6–7) | **Plans:** 4 | **Timeline:** 2026-05-15 (1 day)

**Delivered:** Replaced static step-card layout in the "How It Works" section with a fully interactive stepper UX — horizontal tab rail on desktop, height-animated mobile accordion — giving visitors an at-a-glance overview and on-demand step detail for both "In the App" and "Importing into xLights" workflows.

### Accomplishments

1. BEM stepper CSS: 5-column CSS Grid rail on desktop (display:contents layout), height:0/overflow:hidden accordion on mobile
2. HTML restructure: two `.stepper` blocks replace the old `.workflow` div; phone bezel screenshots wired to "In the App" steps 1–4
3. JS stepper controller: click-to-expand with mobile height animation, adjacent-step collapse, and accessibility (ARIA, aria-expanded)
4. Fire-once IntersectionObserver auto-expands Step 1 on viewport entry without re-triggering on scroll-back
5. Arrow key focus cycling between step headers (left/right on desktop, up/down on mobile) without activating
6. Post-close: mobile accordion starts fully collapsed; per-step chevron icon rotates on expand; desktop↔mobile resize syncs active panel

### Stats

- Requirements shipped: 8/8 STEP requirements (100%)
- Lines added to index.html: ~700 (1,755 → ~2,450)
- Git commits: ~15
- Known deferred items at close: 7 (see STATE.md Deferred Items — all human visual-test acknowledgements)

### Archive

- [Roadmap archive](.planning/milestones/v3.0-ROADMAP.md)
- [Requirements archive](.planning/milestones/v3.0-REQUIREMENTS.md)

### Notes

- No milestone audit run — all phases human-verified via 07-02-SUMMARY.md (12/12 browser checks passed).
- 7 open artifact items (UAT gaps + VERIFICATION.md human_needed) acknowledged at close — code is correct, files not formally updated.
- App Store URL still unavailable; `#download` CTA remains placeholder.
