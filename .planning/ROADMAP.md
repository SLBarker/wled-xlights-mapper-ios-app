# Roadmap: WLED xLights Mapper — Landing Page

## Milestones

- ✅ **v1.0 MVP** — Phases 1–3 (shipped 2026-05-14)
- ✅ **v2.0 UI Polish** — Phases 4–5 (completed 2026-05-14)
- 🔄 **v3.0 Interactive Workflow** — Phases 6–7 (in progress)

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

### v3.0 Interactive Workflow

- [ ] **Phase 6: Stepper Foundation** - CSS component architecture + HTML restructure of both workflow sections
- [ ] **Phase 7: Stepper Interaction** - JS controller for expand/collapse, auto-expand on scroll, and keyboard accessibility

## Phase Details

### Phase 6: Stepper Foundation
**Goal**: Both workflow sections render as a stepper component with all step titles visible and layout adapts correctly across devices
**Depends on**: Nothing (first phase of v3.0 milestone)
**Requirements**: STEP-01, STEP-05, STEP-06, STEP-07, STEP-08
**Success Criteria** (what must be TRUE):
  1. All step titles and numbers are visible at a glance without any user interaction in both "In the App" and "Importing into xLights" sections
  2. Both sections use visually identical stepper markup and CSS — same component, same appearance
  3. On desktop, the stepper renders as a horizontal rail of step titles above a detail panel area
  4. On mobile, the stepper renders as a vertical list where each step header stacks above its content area
  5. An "In the App" step with a configured screenshot shows the image in a phone bezel mockup beside the text; a step without a screenshot shows text at full width
**Plans**: 2 plans
Plans:
- [x] 06-01-PLAN.md — CSS foundation: comment out old workflow CSS, add complete stepper BEM rules + 767px mobile block
- [x] 06-02-PLAN.md — HTML restructure: replace workflow block with two .stepper elements + update animatedEls JS selector
**UI hint**: yes

### Phase 7: Stepper Interaction
**Goal**: Users can click any step to read its detail, with only one step open at a time and Step 1 pre-expanded on scroll entry
**Depends on**: Phase 6
**Requirements**: STEP-02, STEP-03, STEP-04
**Success Criteria** (what must be TRUE):
  1. Clicking or tapping any step title expands that step's detail content inline below the header
  2. When a second step is opened the previously open step collapses — only one detail panel is visible at a time
  3. When the section scrolls into the viewport, Step 1's detail is already expanded without any user action
**Plans**: TBD
**UI hint**: yes

## Progress

| Phase | Milestone | Plans Complete | Status | Completed |
|-------|-----------|----------------|--------|-----------|
| 1. Mobile & Polish | v1.0 | 3/3 | Complete | 2026-05-13 |
| 2. Hero Carousel | v1.0 | 2/2 | Complete | 2026-05-14 |
| 3. Contact Section | v1.0 | 2/2 | Complete | 2026-05-14 |
| 4. Nav & Download CTA | v2.0 | 1/1 | Complete | 2026-05-14 |
| 5. Carousel Peek-View | v2.0 | 2/2 | Complete | 2026-05-14 |
| 6. Stepper Foundation | v3.0 | 2/2 | Verifying | 2026-05-15 |
| 7. Stepper Interaction | v3.0 | 0/? | Not started | - |
