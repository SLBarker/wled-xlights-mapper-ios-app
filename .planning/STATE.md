---
gsd_state_version: 1.0
milestone: v4.0
milestone_name: Discoverability Compliance and Performance
status: active
last_updated: "2026-05-18"
last_activity: 2026-05-18
progress:
  total_phases: 3
  completed_phases: 0
  total_plans: 0
  completed_plans: 0
  percent: 0
---

# Project State

## Project Reference

See: .planning/PROJECT.md (updated 2026-05-18 after v3.0 milestone)

**Core value:** Every visitor should understand what the app does and be able to download it — convert curiosity into App Store taps.
**Current focus:** v4.0 — Phase 8: Discoverability & Legal

## Current Position

```
Phase 8 [          ] 0%   Phase 9 [          ] 0%   Phase 10 [          ] 0%
```

Phase: 8 — Discoverability & Legal
Plan: —
Status: Not started
Last activity: 2026-05-18 — Roadmap created (Phases 8–10)

## Decisions Made

- Phase split: CSS/HTML foundation (Phase 6) separated from JS interaction controller (Phase 7) — static structure first, then behavior layered on top
- STEP-08 (responsive layout) assigned to Phase 6 — both desktop horizontal rail and mobile vertical accordion are pure CSS/media-query concerns; no JS needed for layout switching
- Phase 8 groups DISC-01 + DISC-02 + LEGL-01: all are additive changes to index.html (head and footer); no separate files required
- Phase 9 isolates DISC-03: robots.txt and sitemap.xml are new repo-root files with no index.html changes
- Phase 10 isolates PERF-02: font subsetting and @font-face declaration is a larger, self-contained change

## Carried Forward from v2.0

- Slide width kept at 240px — carousel JS uses `translateX(-N * 240px)` offset
- Arrow hover transform composed as `translateY(-50%) scale(1.08)` — prevents vertical jump on hover
- overflow: hidden kept on track-wrap (not viewport) — arrows live in viewport stacking context above the clip
- 13px desktop / 9px mobile arrow horizontal offsets (centres button over peek strip)

## Performance Metrics

(none yet)

## Deferred Items

Items carried forward from v3.0 close:

| Category | Item | Status |
|----------|------|--------|
| Content | App Store URL for #download CTA | Blocked — awaiting URL |
| Performance | Compress video assets (PERF-03) | Future milestone |
| uat_gap | Phase 04: 04-HUMAN-UAT.md — 5 pending browser-test scenarios | acknowledged |
| uat_gap | Phase 05: 05-HUMAN-UAT.md — 5 pending browser-test scenarios | acknowledged |
| uat_gap | Phase 06: 06-HUMAN-UAT.md — 2 pending browser-test scenarios | acknowledged |
| verification_gap | Phase 04: 04-VERIFICATION.md [human_needed visual checks] | acknowledged |
| verification_gap | Phase 05: 05-VERIFICATION.md [human_needed visual checks] | acknowledged |
| verification_gap | Phase 06: 06-VERIFICATION.md [human_needed visual checks] | acknowledged |
| verification_gap | Phase 07: 07-VERIFICATION.md [manually verified in 07-02 plan, file not updated] | acknowledged |

## Session Continuity

Last session: 2026-05-18
Stopped at: v4.0 roadmap created
Resume with: `/gsd:plan-phase 8`
