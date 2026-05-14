# Roadmap: WLED xLights Mapper — Landing Page

## Milestones

- ✅ **v1.0 MVP** — Phases 1–3 (shipped 2026-05-14)
- 🔄 **v2.0 UI Polish** — Phases 4–5 (in progress)

## Phases

<details>
<summary>✅ v1.0 MVP (Phases 1–3) — SHIPPED 2026-05-14</summary>

- [x] Phase 1: Mobile & Polish (3/3 plans) — completed 2026-05-13
- [x] Phase 2: Hero Carousel (2/2 plans) — completed 2026-05-14
- [x] Phase 3: Contact Section (2/2 plans) — completed 2026-05-14

Full details: [.planning/milestones/v1.0-ROADMAP.md](.planning/milestones/v1.0-ROADMAP.md)

</details>

### v2.0 UI Polish

**[x] Phase 4: Nav & Download CTA** — completed 2026-05-14
- Goal: Fix nav wrapping, pin Download CTA always-visible in nav bar, add icon, match hero hover style
- Requirements: NAV-01, NAV-02, NAV-03, UI-01
- Plans: 1 plan
- Success criteria:
  1. Nav links do not wrap at any viewport width above the hamburger breakpoint
  2. Hamburger toggle appears only at the breakpoint where wrapping would occur
  3. Download CTA is visible in the nav bar at mobile widths alongside the hamburger toggle
  4. Download CTA shows the download SVG icon
  5. Download CTA hover produces `translateY(-2px)` lift and blue glow shadow

Plans:
- [x] 04-01-PLAN.md — CSS + HTML: breakpoint 960px, CTA extracted to flex child with icon, hover style updated

**Phase 5: Carousel Peek-View**
- Goal: Rework carousel to show partial prev/next slides on both sides of the centred active slide
- Requirements: CAR-01, CAR-02
- Success criteria:
  1. Active slide is centred; partial views of adjacent slides are visible on both left and right
  2. Arrows, dot indicators, touch swipe, and 4s auto-advance all work correctly with the new layout
  3. Carousel renders correctly at mobile widths (single-file, no new dependencies)

## Progress

| Phase | Milestone | Plans Complete | Status | Completed |
|-------|-----------|----------------|--------|-----------|
| 1. Mobile & Polish | v1.0 | 3/3 | Complete | 2026-05-13 |
| 2. Hero Carousel | v1.0 | 2/2 | Complete | 2026-05-14 |
| 3. Contact Section | v1.0 | 2/2 | Complete | 2026-05-14 |
| 4. Nav & Download CTA | v2.0 | 1/1 | Complete | 2026-05-14 |
| 5. Carousel Peek-View | v2.0 | 0/— | Pending | — |
