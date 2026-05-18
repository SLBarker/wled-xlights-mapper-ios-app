# Requirements: WLED xLights Mapper — Landing Page

**Defined:** 2026-05-18
**Core Value:** Every visitor who arrives on the page should come away understanding what the app does and be able to download it — the page must convert curiosity into App Store taps.

## v4.0 Requirements

### Discoverability

- [ ] **DISC-01**: Visitor who shares the page URL on social media or chat apps sees a rich preview card with correct title, description, and image
- [ ] **DISC-02**: Visitor sees the app favicon in the browser tab; iOS user bookmarking the page to home screen sees the Apple touch icon
- [ ] **DISC-03**: `robots.txt` allows all search engine crawling; `sitemap.xml` lists the canonical page URL

### Legal

- [ ] **LEGL-01**: Page footer contains a visible link to the privacy policy

### Performance

- [x] **PERF-02**: Inter typeface loads from GitHub Pages assets; no request is made to `fonts.googleapis.com` or `fonts.gstatic.com` on page load

## Future Requirements

Deferred to a future milestone. Tracked but not in current roadmap.

### Performance

- **PERF-03**: Compress video assets — `3dpreview.mp4` is 9.2 MB, significant on mobile (deferred until final video is available)

### Content

- **CONT-01**: Wire App Store URL into `#download` CTA (blocked — URL not yet available)

## Out of Scope

| Feature | Reason |
|---------|--------|
| Backend server or database | Architecture must remain fully static |
| JavaScript frameworks | Vanilla JS only — project constraint |
| Formspree / EmailJS | User decided against; mailto is sufficient |
| OAuth / third-party auth | Static page with no auth requirements |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| DISC-01 | Phase 8 | Pending |
| DISC-02 | Phase 8 | Pending |
| LEGL-01 | Phase 8 | Pending |
| DISC-03 | Phase 9 | Pending |
| PERF-02 | Phase 10 | Complete |

**Coverage:**
- v4.0 requirements: 5 total
- Mapped to phases: 5 (100%) ✓
- Unmapped: 0

---
*Requirements defined: 2026-05-18*
*Last updated: 2026-05-18 — v4.0 roadmap created (Phases 8–10)*
