---
phase: 01-mobile-and-polish
plan: "01"
subsystem: markup
tags: [bugfix, accessibility, html, alt-text, dom-structure, grammar]
dependency_graph:
  requires: []
  provides: [clean-phone-bezel-dom, accurate-alt-text, correct-step04-copy]
  affects: [index.html]
tech_stack:
  added: []
  patterns: [phone-bezel-component, aria-labels]
key_files:
  created: []
  modified:
    - index.html
decisions:
  - "ARKit's possessive proper noun (line 1191) was not changed — the acceptance criterion 'grep -c \"it\\'s\" returns 0' cannot be satisfied because it matches substrings; the grammatical intent (Step 04 it's → its) was fully addressed"
metrics:
  duration: "142s"
  completed: "2026-05-13"
  tasks_completed: 2
  tasks_total: 2
---

# Phase 1 Plan 01: Markup Bug Fixes Summary

Repaired five nested phone-bezel__screen DOM instances, corrected four copy-pasted alt/aria-label texts, and fixed two possessive grammar errors in Step 04 copy.

## Tasks Completed

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Fix nested phone-bezel__screen DOM — five instances | fd579d5 | index.html |
| 2 | Fix alt text and aria-labels, correct Step 04 grammar | 8e62f79 | index.html |

## What Was Built

**Task 1 — DOM structure repair:**
Fixed the nested `.phone-bezel__screen` double-border artefact at five locations:
1. Hero phone 1 (`connect.jpeg`) — removed inner screen wrapper and duplicate notch
2. Hero phone 2 (`ar.jpeg`) — added missing `aria-hidden="true"` to notch (no nesting but notch lacked aria attribute)
3. Hero phone 3 (`preview.jpeg`) — removed inner screen wrapper and duplicate notch
4. Features discovery row (`discovery.mp4`) — removed inner screen wrapper and duplicate notch
5. Features 3D preview row (`3dpreview.mp4`) — removed inner screen wrapper and duplicate notch
6. Export section (`results.jpeg`) — removed inner screen wrapper and duplicate notch

All `<video>` elements retained their `autoplay loop muted playsinline` attributes and inline `style` attribute. All `.phone-bezel__notch` elements now carry `aria-hidden="true"`.

**Task 2 — Text corrections:**
- `connect.jpeg` alt: added "WLED" before "controller" (was missing brand qualifier)
- `ar.jpeg` alt: replaced copy-pasted connect-screen text with accurate AR scanning description
- `preview.jpeg` alt: replaced copy-pasted connect-screen text with accurate 3D preview description
- `results.jpeg` alt: replaced copy-pasted connect-screen text with accurate export results description
- `discovery.mp4` aria-label: replaced AR-scan description with mDNS discovery description
- `3dpreview.mp4` aria-label: replaced AR-scan description with 3D preview description
- Step 04 copy: fixed two instances of "it's" (contraction) → "its" (possessive)

## Deviations from Plan

### Note — Acceptance Criterion vs. Actual State

The acceptance criterion `grep -c "it's" index.html` returns 0 cannot be fully satisfied. Line 1191 contains "ARKit's" — the possessive form of the proper noun "ARKit" — which is grammatically correct and unrelated to the Step 04 grammar fix. This is a grep false positive (substring match).

The plan action explicitly states: "Only fix 'it's' → 'its' inside the Step 04 step-card. Do not alter any other text on the page." Both Step 04 occurrences were fixed. "ARKit's" was correctly left unchanged.

**Classification:** Pre-existing accepted false positive in the test criterion, not a deviation in implementation.

## Known Stubs

None. All four images and both videos now have unique, accurate descriptions.

## Threat Flags

None. This plan modified only text content (alt attributes, aria-labels, copy) within a static HTML file. No new network endpoints, auth paths, or schema changes introduced.

## Self-Check

- [x] `index.html` exists and is modified
- [x] Commits fd579d5 and 8e62f79 exist
- [x] No nested phone-bezel__screen elements remain (verified by Python parse)
- [x] connect.jpeg: "WLED controller connect screen" present
- [x] ar.jpeg: "AR scanning view with gray-code pattern detection overlay" present
- [x] preview.jpeg: "3D LED preview with quality tier colour overlay" present
- [x] results.jpeg: "xmodel export results screen with quantisation metrics" present
- [x] discovery.mp4: "automatic mDNS controller discovery and segment selection" present
- [x] 3dpreview.mp4: "interactive 3D LED preview with pan, zoom, and rotate controls" present
- [x] Step 04: both "it's" → "its" corrected
- [x] "controller connect screen" appears exactly 1 time (connect.jpeg only)
