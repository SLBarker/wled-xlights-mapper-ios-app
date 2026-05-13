# Requirements: WLED xLights Mapper — Landing Page

**Defined:** 2026-05-13
**Core Value:** Every visitor should understand what the app does and be able to download it — convert curiosity into App Store taps.

## v1 Requirements

### Mobile & Responsive

- [ ] **MOBL-01**: All page sections are readable and usable at 375px–768px viewport widths with no clipped or overflowing content
- [ ] **MOBL-02**: Hamburger navigation toggle is visible and functional on mobile — tap opens/closes nav links with animation
- [ ] **MOBL-03**: Nested `phone-bezel__screen` / duplicate notch DOM bug is fixed across all five affected instances
- [ ] **MOBL-04**: All `<img>` alt text is accurate and unique (no copy-pasted "controller connect screen" description on unrelated screenshots)
- [ ] **MOBL-05**: Grammar typos corrected — "it's" → "its" in Step 04 copy

### Performance

- [ ] **PERF-01**: Both autoplay videos (`3dpreview.mp4`, `discovery.mp4`) load lazily — not fetched until they scroll near the viewport

### Hero Carousel

- [ ] **HERO-01**: Hero section displays a conventional sliding carousel in place of the horizontal fan/strip of phone mockups
- [ ] **HERO-02**: Carousel uses the existing phone mockup assets (same screenshots/videos) as carousel slides
- [ ] **HERO-03**: Carousel includes prev/next controls and dot indicators; supports touch/swipe on mobile

### Contact

- [ ] **CONT-01**: A Contact / Feedback section is added to the page, styled to match the existing design language
- [ ] **CONT-02**: Section contains a "Send Feedback" CTA button that opens the user's email client via `mailto:`
- [ ] **CONT-03**: `mailto:` link includes a pre-filled subject line: `Feedback for WLED xLights Mapper`

## v2 Requirements

### Performance

- **PERF-02**: Self-host Inter font subset to remove Google Fonts CDN dependency and improve privacy
- **PERF-03**: Compress and resize video assets (3dpreview.mp4 is 9.2 MB — significant for mobile)

### Discovery

- **DISC-01**: Add Open Graph / social meta tags (`og:title`, `og:description`, `og:image`) for link previews
- **DISC-02**: Add favicon and Apple touch icon
- **DISC-03**: Add `robots.txt` and XML sitemap

### Legal

- **LEGL-01**: Add privacy policy link to footer (required for App Store compliance)

## Out of Scope

| Feature | Reason |
|---------|--------|
| Formspree / EmailJS | User decided against; mailto sufficient for v1 |
| Backend / server logic | Architecture must remain fully static |
| JavaScript frameworks | Vanilla JS only per architectural constraint |
| App Store link | No URL provided; placeholder `#download` remains until URL available |
| `.DS_Store` cleanup | Repository hygiene — tracked separately from page features |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| MOBL-01 | Phase 1 | Pending |
| MOBL-02 | Phase 1 | Pending |
| MOBL-03 | Phase 1 | Pending |
| MOBL-04 | Phase 1 | Pending |
| MOBL-05 | Phase 1 | Pending |
| PERF-01 | Phase 1 | Pending |
| HERO-01 | Phase 2 | Pending |
| HERO-02 | Phase 2 | Pending |
| HERO-03 | Phase 2 | Pending |
| CONT-01 | Phase 3 | Pending |
| CONT-02 | Phase 3 | Pending |
| CONT-03 | Phase 3 | Pending |

**Coverage:**
- v1 requirements: 12 total
- Mapped to phases: 12
- Unmapped: 0 ✓

---
*Requirements defined: 2026-05-13*
*Last updated: 2026-05-13 after initial definition*
