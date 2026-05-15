# Requirements: WLED xLights Mapper — Landing Page

**Defined:** 2026-05-14
**Core Value:** Every visitor should understand what the app does and be able to download it — convert curiosity into App Store taps.

## v3.0 Requirements

Requirements for the Interactive Workflow milestone. Each maps to a roadmap phase.

### Stepper Component

- [x] **STEP-01**: User sees all step titles and numbers at a glance without scrolling or interacting — the full sequence is visible as an overview — *Validated Phase 6*
- [ ] **STEP-02**: User can click or tap any step to expand its detail content inline below the step header
- [ ] **STEP-03**: Clicking a new step collapses the previously open step — only one step's detail is visible at a time
- [ ] **STEP-04**: Step 1 is expanded by default when the section scrolls into the viewport
- [x] **STEP-05**: Both "In the App" and "Importing into xLights" use the same visual component and interaction pattern — *Validated Phase 6*
- [x] **STEP-06**: Expanded "In the App" steps optionally display an app screenshot in a phone bezel mockup beside the detail text — *Validated Phase 6*
- [x] **STEP-07**: Step detail layout adapts gracefully when no screenshot is configured — text fills the full available width — *Validated Phase 6*
- [x] **STEP-08**: On desktop the stepper renders as a horizontal rail of step titles with a detail panel below; on mobile it collapses to a vertical accordion — *Validated Phase 6*

## v2.0 Requirements (completed)

All v2.0 requirements shipped 2026-05-14.

### Carousel

- [x] **CAR-01**: Carousel shows partial views of the previous and next slides simultaneously on both sides of the active (centred) slide
- [x] **CAR-02**: Carousel navigation — arrows, dot indicators, touch swipe, and auto-advance — functions correctly with the peek-view layout

### Navigation

- [x] **NAV-01**: Desktop navigation links never wrap onto a second line; the hamburger toggle appears at exactly the breakpoint where wrapping would otherwise occur
- [x] **NAV-02**: Download CTA remains visible in the nav bar at all viewport widths and is never moved into the hamburger drawer
- [x] **NAV-03**: Download CTA in the nav bar includes the download SVG icon (circle + down-arrow) matching the hero section button

### UI Polish

- [x] **UI-01**: Download CTA hover in the nav bar applies `translateY(-2px)` lift and the blue glow box-shadow, matching the hero section button hover

## Future Requirements

Deferred from v1/v2. Tracked but not in the current roadmap.

### Performance

- **PERF-02**: Self-host Inter font subset — remove Google Fonts CDN dependency
- **PERF-03**: Compress video assets — 3dpreview.mp4 is 9.2 MB, significant on mobile

### Discovery

- **DISC-01**: Open Graph / social meta tags — og:title, og:description, og:image
- **DISC-02**: Favicon and Apple touch icon
- **DISC-03**: robots.txt and XML sitemap

### Legal

- **LEGL-01**: Privacy policy link in footer — required for App Store compliance

## Out of Scope

| Feature | Reason |
|---------|--------|
| Backend / server logic | Architecture must remain fully static (GitHub Pages) |
| JavaScript frameworks | Vanilla JS only — no React, Vue, etc. |
| External stylesheets | All CSS inline in `<style>` block |
| App Store URL wiring | Blocked until URL is available |
| Formspree / EmailJS | User decided against; mailto is sufficient |
| Multiple steps open simultaneously | User chose exclusive stepper (one open at a time) |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| STEP-01 | Phase 6 | ✓ Complete |
| STEP-02 | Phase 7 | Pending |
| STEP-03 | Phase 7 | Pending |
| STEP-04 | Phase 7 | Pending |
| STEP-05 | Phase 6 | ✓ Complete |
| STEP-06 | Phase 6 | ✓ Complete |
| STEP-07 | Phase 6 | ✓ Complete |
| STEP-08 | Phase 6 | ✓ Complete |

**Coverage:**
- v3.0 requirements: 8 total
- Mapped to phases: 8 (5 → Phase 6, 3 → Phase 7)
- Unmapped: 0 ✓

---
*Requirements defined: 2026-05-14*
*Last updated: 2026-05-15 — Phase 6 complete, STEP-01/05/06/07/08 validated*
