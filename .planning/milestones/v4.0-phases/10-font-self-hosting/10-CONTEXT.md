# Phase 10: Font Self-Hosting - Context

**Gathered:** 2026-05-18
**Status:** Ready for planning

<domain>
## Phase Boundary

Replace the two Google Fonts `<link>` tags (`<link rel="preconnect">` to `fonts.googleapis.com` and the stylesheet `<link>` that imports Inter) with self-hosted WOFF2 files committed to `resources/fonts/` and `@font-face` declarations in the inline `<style>` block of `index.html`. No other changes to `index.html` structure or content. After this phase, zero requests are made to `fonts.googleapis.com` or `fonts.gstatic.com` on page load (satisfies PERF-02).

</domain>

<decisions>
## Implementation Decisions

### Font Weights (PERF-02)
- **D-01:** Self-host 4 weights only: **400 (Regular), 500 (Medium), 600 (SemiBold), 700 (Bold)**. Drop weight 300 (Light) — it is loaded by the current Google Fonts request but is never explicitly assigned in CSS and is therefore invisible to visitors.
- **D-02:** Latin subset only — matches what Google Fonts serves by default for English content; no full Unicode set needed.

### Font Format
- **D-03:** WOFF2 only — no WOFF fallback. WOFF2 is supported by all modern browsers including every iOS version this landing page targets. Keeps the file set small and `@font-face` declarations simple.

### Loading Strategy
- **D-04:** `font-display: swap` — fallback font (`-apple-system`, `BlinkMacSystemFont`) shows immediately; Inter swaps in when loaded. Matches current Google Fonts behavior exactly, eliminating visual regression risk.
- **D-05:** No `<link rel="preload">` hints — `font-display: swap` alone is sufficient. Avoids extra `<head>` tags and bandwidth contention.

### Claude's Discretion
- Exact `@font-face` declaration ordering in the `<style>` block — place them at the top of the inline `<style>`, before the reset/token rules.
- Where to obtain WOFF2 files — download directly from the Inter GitHub releases or via the `google-webfonts-helper` tool; either produces the same WOFF2 output for Latin subset.
- File naming convention — e.g., `inter-400.woff2`, `inter-500.woff2`, etc. (flat naming, no versioned subfolders needed for a static single-page site).

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements & Roadmap
- `.planning/REQUIREMENTS.md` — PERF-02 is the sole requirement this phase satisfies; acceptance criteria: zero requests to `fonts.googleapis.com` or `fonts.gstatic.com` on page load, Inter renders visually identically
- `.planning/ROADMAP.md` — Phase 10 goal and success criteria (3 checks)

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint: all CSS must remain inline in the `<style>` block; no external stylesheets. `@font-face` declarations go inside the inline `<style>`, not in a separate `.css` file. Font binary files in `resources/fonts/` are acceptable (same pattern as other assets in `resources/`)
- `.planning/PROJECT.md` — No build pipeline, GitHub Pages static hosting; font files must be committed directly to the repo

### Existing Code to Modify
- `index.html` lines 23–24 — Current Google Fonts tags to remove: `<link rel="preconnect" href="https://fonts.googleapis.com">` and `<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">`. The `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>` tag (if present) must also be removed.
- `index.html` `<style>` block — Add `@font-face` declarations at the top, before the reset/token rules

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `resources/` directory at repo root — already holds screenshots, icons, and video assets. `resources/fonts/` follows the same pattern; no new directory convention needed.

### Established Patterns
- CSS inline in `<style>` block (single-file constraint) — `@font-face` declarations slot into the top of this block.
- Fallback font stack already defined: `font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif` — no change needed here; fallback already correct.

### Integration Points
- `index.html` `<head>`: remove 2 (or 3) Google Fonts `<link>` tags; these are the only `<head>` changes.
- `index.html` `<style>` block top: add 4 `@font-face` rules (one per weight: 400, 500, 600, 700).
- `resources/fonts/`: new directory with 4 WOFF2 files.

</code_context>

<specifics>
## Specific Ideas

- File size expectation: ~25–30KB per WOFF2 weight (Latin subset), ~100–120KB total for 4 weights. Acceptable for a landing page.
- The Inter font can be obtained from https://github.com/rsms/inter/releases (official Inter releases) or via google-webfonts-helper for the exact Latin subset Google Fonts serves.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 10-Font Self-Hosting*
*Context gathered: 2026-05-18*
