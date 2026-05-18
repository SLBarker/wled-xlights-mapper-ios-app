# Phase 10: Font Self-Hosting - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-18
**Phase:** 10-Font Self-Hosting
**Areas discussed:** Font weights, Preload strategy, font-display value

---

## Font Weights

| Option | Description | Selected |
|--------|-------------|----------|
| Drop 300 — 4 weights only | 400/500/600/700. Saves ~25KB WOFF2. Weight 300 is never set in CSS so dropping it is invisible. | ✓ |
| Keep all 5 — match current exactly | 300/400/500/600/700. File-for-file parity with current Google Fonts request. | |

**User's choice:** Drop 300 — 4 weights only (400, 500, 600, 700)
**Notes:** CSS audit confirmed weight 300 is loaded by Google Fonts but never explicitly assigned in any rule.

---

## Preload Strategy

| Option | Description | Selected |
|--------|-------------|----------|
| No preload — font-display: swap only | Simplest. Fallback shows instantly, Inter swaps in. Matches current Google Fonts experience. | ✓ |
| Preload weight 400 only | Adds one `<link rel="preload">` for Inter-Regular. Reduces FOUT on first visit. | |
| Preload all 4 weights | Four preload hints. Fastest Inter render, but adds 4 tags and may delay other resources. | |

**User's choice:** No preload — font-display: swap only
**Notes:** Simplicity preferred; swap behaviour matches what visitors already experience with Google Fonts.

---

## font-display Value

| Option | Description | Selected |
|--------|-------------|----------|
| swap | Fallback font shows immediately, Inter swaps in when loaded. Matches current Google Fonts behaviour. | ✓ |
| optional | Inter only renders if it loads within ~100ms. No FOUT, but Inter may not appear on first visit. | |

**User's choice:** swap
**Notes:** Chosen for exact parity with current Google Fonts behaviour — no visual regression risk.

---

## Claude's Discretion

- `@font-face` declaration ordering in the `<style>` block — place at top before reset/token rules
- Where to obtain WOFF2 files — Inter GitHub releases or google-webfonts-helper (Latin subset)
- File naming convention — `inter-400.woff2`, `inter-500.woff2`, etc.

## Deferred Ideas

None — discussion stayed within phase scope.
