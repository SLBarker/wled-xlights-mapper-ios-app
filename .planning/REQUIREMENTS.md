# Requirements: WLED xLights Mapper — Landing Page

**Defined:** 2026-05-14
**Core Value:** Every visitor should understand what the app does and be able to download it — convert curiosity into App Store taps.

## v2.0 Requirements

Requirements for the UI Polish milestone. Each maps to a roadmap phase.

### Carousel

- [ ] **CAR-01**: Carousel shows partial views of the previous and next slides simultaneously on both sides of the active (centred) slide
- [ ] **CAR-02**: Carousel navigation — arrows, dot indicators, touch swipe, and auto-advance — functions correctly with the peek-view layout

### Navigation

- [ ] **NAV-01**: Desktop navigation links never wrap onto a second line; the hamburger toggle appears at exactly the breakpoint where wrapping would otherwise occur
- [ ] **NAV-02**: Download CTA remains visible in the nav bar at all viewport widths and is never moved into the hamburger drawer
- [ ] **NAV-03**: Download CTA in the nav bar includes the download SVG icon (circle + down-arrow) matching the hero section button

### UI Polish

- [ ] **UI-01**: Download CTA hover in the nav bar applies `translateY(-2px)` lift and the blue glow box-shadow, matching the hero section button hover

## Future Requirements

Deferred from v1. Tracked but not in the current roadmap.

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

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| NAV-01 | Phase 4 | Pending |
| NAV-02 | Phase 4 | Pending |
| NAV-03 | Phase 4 | Pending |
| UI-01  | Phase 4 | Pending |
| CAR-01 | Phase 5 | Pending |
| CAR-02 | Phase 5 | Pending |

**Coverage:**
- v2.0 requirements: 6 total
- Mapped to phases: 6
- Unmapped: 0 ✓

---
*Requirements defined: 2026-05-14*
*Last updated: 2026-05-14 after v2.0 milestone definition*
