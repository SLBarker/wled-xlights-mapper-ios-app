# Requirements: WLED xLights Mapper — Landing Page

**Defined:** 2026-05-18
**Core Value:** Every visitor who arrives on the page should come away understanding what the app does and be able to download it — the page must convert curiosity into App Store taps.

## v4.1 Requirements

### CTA State

- [ ] **CTA-01**: All download CTAs (nav pill and hero button) display "Coming Soon" text — visitor immediately knows the app is not yet available without needing to click anything
- [ ] **CTA-02**: Download CTAs are non-interactive — clicking or tapping does nothing (no scroll, no navigation, no broken anchor jump to `#download`)
- [ ] **CTA-03**: Download CTAs have a visually distinct disabled appearance — reduced opacity, `cursor: not-allowed`, hover animation suppressed — so the non-clickable state is communicated by appearance alone, not only by text

## Future Requirements

Deferred to a future milestone. Tracked but not in current roadmap.

### Content

- **CONT-01**: Wire the real App Store URL into both CTAs once it is available from Apple — replace the "Coming Soon" treatment with a live deep-link to the App Store listing

### Performance

- **PERF-03**: Compress video assets — `3dpreview.mp4` is 9.2 MB, significant on mobile (deferred until final video is available)

## Out of Scope

| Feature | Reason |
|---------|--------|
| Email capture / "notify me" form | No backend; mailto workaround is too manual for a proper notify flow |
| Countdown timer | No confirmed launch date to count down to |
| New "coming soon" hero section or page | CTA treatment alone is sufficient; no new content sections needed |
| Backend server or database | Architecture must remain fully static |
| JavaScript frameworks | Vanilla JS only — project constraint |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| CTA-01 | Phase 11 | Pending |
| CTA-02 | Phase 11 | Pending |
| CTA-03 | Phase 11 | Pending |

**Coverage:**
- v4.1 requirements: 3 total
- Mapped to phases: 3 (100%) ✓
- Unmapped: 0

---
*Requirements defined: 2026-05-18*
*Last updated: 2026-05-18 — v4.1 milestone started*
