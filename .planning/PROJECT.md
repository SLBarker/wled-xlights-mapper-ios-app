# WLED xLights Mapper — Landing Page

## What This Is

A single-file static landing page (GitHub Pages) marketing the WLED to xLights Mapper iOS app. It communicates the app's core value proposition — accurate LED mapping using a standard iPhone — through a hero carousel, interactive step-by-step workflow sections, feature rows, privacy assurance, and a direct user-feedback channel.

## Core Value

Every visitor who arrives on the page should come away understanding what the app does and be able to download it — the page must convert curiosity into App Store taps.

## Current State

**Shipped:** v1.0 MVP (2026-05-14), v2.0 UI Polish (2026-05-14), v3.0 Interactive Workflow (2026-05-15)

- Mobile-responsive with hamburger nav, single-column grids, and accessible keyboard navigation
- 4-slide hero carousel with 3D depth peek-view (adjacent slides partially visible)
- Nav links never wrap; Download CTA pinned in nav bar at all widths with icon and hover style
- Interactive stepper UX in "How It Works" — horizontal tab rail on desktop, height-animated accordion with chevron toggles on mobile
- Both "In the App" and "Importing into xLights" steppers use identical BEM component; "In the App" steps show phone bezel screenshots
- Contact/Feedback section with pre-filled mailto CTA
- 13.3 MB of autoplay video deferred from page load via IntersectionObserver
- ~2,450 lines, single `index.html`, no build pipeline, GitHub Pages hosted

**Open:** App Store URL not yet available — `#download` CTA remains a placeholder link.

## Requirements

### Validated

- ✓ Full landing page content rendered — hero, features, how-it-works, export, requirements, tips, privacy, footer — existing
- ✓ Scroll-reveal entrance animations (IntersectionObserver) — existing
- ✓ Apple-inspired design system (CSS tokens, Inter typeface, frosted-glass nav) — existing
- ✓ Desktop layout — two-column feature rows, alternating section backgrounds — existing
- ✓ Phone bezel mockup component — reusable CSS pattern used across multiple sections — existing
- ✓ Static hosting on GitHub Pages, no build pipeline — existing
- ✓ Mobile-responsive layout — all sections usable and readable at phone width — v1.0
- ✓ Hamburger navigation — nav links accessible on mobile via a toggle menu — v1.0
- ✓ Lazy video loading — 13.3 MB of MP4 video deferred until scroll proximity — v1.0
- ✓ Hero carousel — 4-slide sliding carousel replaces horizontal fan/strip — v1.0
- ✓ Contact / feedback section — styled section with mailto CTA for user feedback — v1.0
- ✓ Carousel peek-view — both adjacent slides partially visible — v2.0
- ✓ Carousel navigation works with peek layout — v2.0
- ✓ Desktop nav links never wrap before hamburger triggers — v2.0
- ✓ Download CTA always visible in nav bar at all widths — v2.0
- ✓ Download CTA includes download icon in nav bar — v2.0
- ✓ Download CTA hover matches hero section style — v2.0
- ✓ All step titles and numbers visible at a glance without interaction (STEP-01) — v3.0
- ✓ Click/tap any step to expand its detail content inline (STEP-02) — v3.0
- ✓ Only one step open at a time — clicking a new step collapses the previous (STEP-03) — v3.0
- ✓ Step 1 pre-expanded when the section scrolls into the viewport (STEP-04) — v3.0
- ✓ Both workflow sections use identical stepper component and interaction (STEP-05) — v3.0
- ✓ Expanded "In the App" steps optionally show screenshot in phone bezel (STEP-06) — v3.0
- ✓ Steps without a screenshot show text at full width (STEP-07) — v3.0
- ✓ Horizontal rail on desktop, vertical accordion on mobile (STEP-08) — v3.0

### Active (v4.0 targets)

- [ ] Wire App Store URL into `#download` CTA — blocked until URL is available
- [ ] Open Graph / social meta tags — og:title, og:description, og:image (DISC-01)
- [ ] Favicon and Apple touch icon (DISC-02)
- [ ] robots.txt and XML sitemap (DISC-03)
- [ ] Privacy policy link in footer — required for App Store compliance (LEGL-01)
- [ ] Self-host Inter font subset — remove Google Fonts CDN dependency (PERF-02)
- [ ] Compress video assets — 3dpreview.mp4 is 9.2 MB, significant on mobile (PERF-03)

### Out of Scope

- Backend server or database — architecture must remain fully static
- JavaScript frameworks (React, Vue, etc.) — keep vanilla JS only
- Formspree / EmailJS — user decided against; mailto is sufficient
- `.DS_Store` cleanup — repository hygiene, tracked separately

## Context

- Single deployable artifact: `index.html` (~2,450 lines) with all CSS and JS inline
- Assets in `resources/` (screenshots, icons, two autoplay MP4 videos)
- Available screenshots: `connect.jpeg`, `ar.jpeg`, `preview.jpeg`, `results.jpeg` — used in "In the App" stepper steps 1–4
- App Store link placeholder `#download` remains until URL provided

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
| Keep single-file architecture | Constraint from user; no build pipeline | ✓ Maintained throughout all phases |
| Contact before Privacy in nav/page | User preference — natural flow before legal/privacy | ✓ Applied Phase 3 |
| Drawer as sibling element, not mutation of desktop nav | Keeps desktop nav clean and mobile drawer independent | ✓ Established pattern |
| data-src pattern for lazy video | Browser-native, no placeholder image needed | ✓ Clean deferral with 200px rootMargin pre-load buffer |
| Stepper rail for workflow steps | User wants overview-first UX; horizontal rail gives sequence at a glance | ✓ Shipped v3.0 |
| Optional screenshot per step | Not every step has a relevant asset; layout adapts when absent | ✓ Shipped v3.0 |
| CSS Grid + display:contents for stepper layout | Headers and panels at different DOM nesting need to share grid rows | ✓ Shipped v3.0 |
| Phase split: CSS/HTML foundation then JS interaction | Static structure first, behavior layered on — cleanly separable concerns | ✓ Clean execution in v3.0 |
| Two separate IIFEs for click vs auto-expand/keyboard | Independently committable, testable, revertable | ✓ Worked well in v3.0 |
| Mobile accordion starts fully collapsed with chevron toggle | Better mobile UX — user controls what they read; saves vertical space | ✓ Shipped post-v3.0 close |

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
*Last updated: 2026-05-18 after v3.0 milestone*
