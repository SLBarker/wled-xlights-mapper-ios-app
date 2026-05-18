---
phase: 10-font-self-hosting
plan: 01
subsystem: ui
tags: [inter, woff2, fonts, performance, self-hosting]

# Dependency graph
requires: []
provides:
  - "Inter Latin-subset WOFF2 files (weights 400, 500, 600, 700) in resources/fonts/"
  - "Font binary assets committed to repo, ready for @font-face referencing"
affects:
  - 10-font-self-hosting (Plan 10-02 — @font-face declarations in index.html)

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "Font assets stored in resources/fonts/ following existing resources/ asset convention"

key-files:
  created:
    - resources/fonts/inter-400.woff2
    - resources/fonts/inter-500.woff2
    - resources/fonts/inter-600.woff2
    - resources/fonts/inter-700.woff2
  modified: []

key-decisions:
  - "Used gwfh.mranftl.com API to discover current WOFF2 URLs (v13 per-file paths were stale; actual version is v20)"
  - "Downloaded Latin-subset WOFF2 files directly from fonts.gstatic.com via gwfh API variant URLs"
  - "Flat file naming: inter-400.woff2 through inter-700.woff2 — no version suffix, no subfolders"

patterns-established:
  - "Font assets in resources/fonts/ — consistent with resources/screenshot/, resources/video/, resources/icons/ sibling directories"

requirements-completed: [PERF-02]

# Metrics
duration: 1min
completed: 2026-05-18
---

# Phase 10 Plan 01: Font Self-Hosting — Download WOFF2 Files Summary

**Inter v20 Latin-subset WOFF2 files (400/500/600/700) fetched via gwfh API and committed to resources/fonts/ (~24KB each, 96KB total)**

## Performance

- **Duration:** 1 min
- **Started:** 2026-05-18T20:52:38Z
- **Completed:** 2026-05-18T20:53:32Z
- **Tasks:** 1
- **Files modified:** 4 (new binary assets)

## Accomplishments

- Created `resources/fonts/` directory following existing asset directory pattern
- Downloaded Inter v20 Latin-subset WOFF2 files for weights 400 (Regular), 500 (Medium), 600 (SemiBold), 700 (Bold)
- All 4 files verified as valid WOFF2 binaries (~23-24KB each, within 15-60KB acceptance range)
- No weight-300 file downloaded (correctly dropped per D-01 decision)

## Task Commits

Each task was committed atomically:

1. **Task 1: Create resources/fonts/ and download Inter Latin WOFF2 files** - `71ad204` (feat)

**Plan metadata:** (pending — docs commit below)

## Files Created/Modified

- `resources/fonts/inter-400.woff2` - Inter Regular weight, Latin subset, WOFF2 (23664 bytes)
- `resources/fonts/inter-500.woff2` - Inter Medium weight, Latin subset, WOFF2 (24272 bytes)
- `resources/fonts/inter-600.woff2` - Inter SemiBold weight, Latin subset, WOFF2 (24452 bytes)
- `resources/fonts/inter-700.woff2` - Inter Bold weight, Latin subset, WOFF2 (24356 bytes)

## Decisions Made

- The plan's per-file URL pattern (`gwfh.mranftl.com/fonts/inter/v13/...`) returned 2035-byte HTML error pages — the font version has advanced to v20. Used the API fallback immediately: `curl -s "https://gwfh.mranftl.com/api/fonts/inter?subsets=latin"` extracted current WOFF2 URLs from the variants JSON array, then downloaded from `fonts.gstatic.com` directly.

## Deviations from Plan

None — plan executed exactly as written (fallback path used as documented, not a deviation).

The v13 URL failure was anticipated in the plan: "If the v13 URLs fail, query the API... and extract WOFF2 URLs from the variants JSON". The API fallback worked correctly and produced identical file quality.

## Issues Encountered

- Per-file v13 URLs at gwfh.mranftl.com returned HTML error responses (2035 bytes). API query revealed current version is v20. Resolved immediately using the plan's documented fallback path.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- `resources/fonts/` contains exactly 4 valid WOFF2 files: inter-400.woff2, inter-500.woff2, inter-600.woff2, inter-700.woff2
- Plan 10-02 can reference these files via `src: url('resources/fonts/inter-NNN.woff2')` in `@font-face` declarations
- Plan 10-02 should also remove the 2 Google Fonts `<link>` tags from `index.html` `<head>` (lines 23-24)

---
*Phase: 10-font-self-hosting*
*Completed: 2026-05-18*
