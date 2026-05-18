# Roadmap: WLED xLights Mapper — Landing Page

## Milestones

- ✅ **v1.0 MVP** — Phases 1–3 (shipped 2026-05-14)
- ✅ **v2.0 UI Polish** — Phases 4–5 (shipped 2026-05-14)
- ✅ **v3.0 Interactive Workflow** — Phases 6–7 (shipped 2026-05-15)
- 📋 **v4.0 Discoverability, Compliance & Performance** — Phases 8–10 (planned)

## Phases

<details>
<summary>✅ v1.0 MVP (Phases 1–3) — SHIPPED 2026-05-14</summary>

- [x] Phase 1: Mobile & Polish (3/3 plans) — completed 2026-05-13
- [x] Phase 2: Hero Carousel (2/2 plans) — completed 2026-05-14
- [x] Phase 3: Contact Section (2/2 plans) — completed 2026-05-14

Full details: [.planning/milestones/v1.0-ROADMAP.md](.planning/milestones/v1.0-ROADMAP.md)

</details>

<details>
<summary>✅ v2.0 UI Polish (Phases 4–5) — SHIPPED 2026-05-14</summary>

- [x] Phase 4: Nav & Download CTA (1/1 plans) — completed 2026-05-14
- [x] Phase 5: Carousel Peek-View (2/2 plans) — completed 2026-05-14

</details>

<details>
<summary>✅ v3.0 Interactive Workflow (Phases 6–7) — SHIPPED 2026-05-15</summary>

- [x] Phase 6: Stepper Foundation (2/2 plans) — completed 2026-05-15
- [x] Phase 7: Stepper Interaction (2/2 plans) — completed 2026-05-15

Full details: [.planning/milestones/v3.0-ROADMAP.md](.planning/milestones/v3.0-ROADMAP.md)

</details>

### v4.0 Discoverability, Compliance & Performance

- [x] **Phase 8: Discoverability & Legal** — Open Graph tags, favicon, Apple touch icon, privacy policy footer link — completed 2026-05-18
- [ ] **Phase 9: Crawl Infrastructure** — robots.txt and XML sitemap
- [ ] **Phase 10: Font Self-Hosting** — Inter font served from GitHub Pages; Google Fonts CDN removed

## Phase Details

### Phase 8: Discoverability & Legal
**Goal**: The page presents a rich social preview when shared and satisfies App Store compliance requirements
**Depends on**: Nothing (all changes are additive to existing index.html head and footer)
**Requirements**: DISC-01, DISC-02, LEGL-01
**Success Criteria** (what must be TRUE):
  1. Sharing the page URL in iMessage, Twitter/X, Slack, or LinkedIn renders a preview card with the correct app title, description, and screenshot image
  2. Browser tab shows the WLED xLights Mapper favicon; iOS home-screen bookmark shows the Apple touch icon
  3. Page footer contains a visible, tappable privacy policy link
**Plans**: 2 plans
Plans:
**Wave 1**
- [x] 08-01-PLAN.md — Social/discovery head tags (OG, Twitter card, canonical, favicon links) + favicon.svg + PNG favicon checkpoint

**Wave 2** *(blocked on Wave 1 completion)*
- [x] 08-02-PLAN.md — privacy.html page creation + footer Privacy Policy link in index.html

**Cross-cutting constraints:**
- All CSS must remain inline in `<style>` blocks (no external stylesheets) — CLAUDE.md constraint
- Favicon accent color is `#0071e3` (the real `--accent` token), not `#2dd4bf` — UI-SPEC.md correction
- privacy.html is an acceptable companion file per CLAUDE.md
**UI hint**: yes

### Phase 9: Crawl Infrastructure
**Goal**: Search engines can discover and index the page without restriction
**Depends on**: Phase 8
**Requirements**: DISC-03
**Success Criteria** (what must be TRUE):
  1. A `robots.txt` file is served at the root URL and contains `Allow: /` for all crawlers
  2. A `sitemap.xml` file is served at the root URL and lists the canonical page URL with a `<loc>` entry
  3. Fetching `robots.txt` and `sitemap.xml` directly in a browser returns valid content (no 404)
**Plans**: 1 plan
Plans:
- [ ] 09-01-PLAN.md — Open robots.txt to all crawlers + add Sitemap directive, create sitemap.xml with canonical URL, add link rel=sitemap to index.html head

### Phase 10: Font Self-Hosting
**Goal**: Inter typeface loads from GitHub Pages assets with no outbound request to Google Fonts
**Depends on**: Phase 8
**Requirements**: PERF-02
**Success Criteria** (what must be TRUE):
  1. Network panel on a hard-reload shows zero requests to `fonts.googleapis.com` or `fonts.gstatic.com`
  2. Inter typeface renders visually identically to the previous Google Fonts version across desktop and mobile
  3. Font files are present in `resources/fonts/` and referenced via a `@font-face` declaration in the inline `<style>` block
**Plans**: TBD

## Progress

| Phase | Milestone | Plans Complete | Status | Completed |
|-------|-----------|----------------|--------|-----------|
| 1. Mobile & Polish | v1.0 | 3/3 | Complete | 2026-05-13 |
| 2. Hero Carousel | v1.0 | 2/2 | Complete | 2026-05-14 |
| 3. Contact Section | v1.0 | 2/2 | Complete | 2026-05-14 |
| 4. Nav & Download CTA | v2.0 | 1/1 | Complete | 2026-05-14 |
| 5. Carousel Peek-View | v2.0 | 2/2 | Complete | 2026-05-14 |
| 6. Stepper Foundation | v3.0 | 2/2 | Complete | 2026-05-15 |
| 7. Stepper Interaction | v3.0 | 2/2 | Complete | 2026-05-15 |
| 8. Discoverability & Legal | v4.0 | 2/2 | Complete | 2026-05-18 |
| 9. Crawl Infrastructure | v4.0 | 0/1 | Not started | — |
| 10. Font Self-Hosting | v4.0 | 0/? | Not started | — |
