# Roadmap: WLED xLights Mapper — Landing Page

## Milestones

- ✅ **v1.0 MVP** — Phases 1–3 (shipped 2026-05-14)
- ✅ **v2.0 UI Polish** — Phases 4–5 (shipped 2026-05-14)
- ✅ **v3.0 Interactive Workflow** — Phases 6–7 (shipped 2026-05-15)
- ✅ **v4.0 Discoverability, Compliance & Performance** — Phases 8–10 (shipped 2026-05-18)
- ✅ **v4.1 Coming Soon CTA** — Phase 11 (shipped 2026-05-18)

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

<details>
<summary>✅ v4.0 Discoverability, Compliance & Performance (Phases 8–10) — SHIPPED 2026-05-18</summary>

- [x] Phase 8: Discoverability & Legal (2/2 plans) — completed 2026-05-18
- [x] Phase 9: Crawl Infrastructure (1/1 plans) — completed 2026-05-18
- [x] Phase 10: Font Self-Hosting (2/2 plans) — completed 2026-05-18

Full details: [.planning/milestones/v4.0-ROADMAP.md](.planning/milestones/v4.0-ROADMAP.md)

</details>

### ✅ v4.1 Coming Soon CTA — SHIPPED 2026-05-18

- [x] **Phase 11: Coming Soon CTA** — Replace broken `#download` placeholder with a disabled "Coming Soon" state on all download CTAs

## Phase Details

### Phase 11: Coming Soon CTA
**Goal**: Visitors can immediately see that the app is not yet available — both the nav pill and hero button clearly communicate "Coming Soon" and cannot be activated
**Depends on**: Phase 10 (prior milestone complete)
**Requirements**: CTA-01, CTA-02, CTA-03
**Success Criteria** (what must be TRUE):
  1. Nav Download pill shows "Coming Soon" text at all viewport widths
  2. Hero Download button shows "Coming Soon" text
  3. Clicking or tapping either button does not navigate, scroll, or produce any action
  4. Both buttons appear visually distinct from active/clickable buttons (reduced opacity, cursor: not-allowed, hover animation suppressed)
  5. Both buttons remain keyboard-focusable — no accessibility regression (aria-disabled pattern preferred over removing tabindex)
**Plans**: 1 plan
Plans:
- [x] 11-01-PLAN.md — Convert nav pill + hero button to disabled "Coming Soon" CTAs with disabled-state CSS
**UI hint**: yes

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
| 9. Crawl Infrastructure | v4.0 | 1/1 | Complete | 2026-05-18 |
| 10. Font Self-Hosting | v4.0 | 2/2 | Complete | 2026-05-18 |
| 11. Coming Soon CTA | v4.1 | 1/1 | Complete | 2026-05-18 |
