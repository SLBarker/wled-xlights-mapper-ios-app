# Phase 6: Stepper Foundation - Context

**Gathered:** 2026-05-15
**Status:** Ready for planning

<domain>
## Phase Boundary

Replace both workflow sections (`how-it-works`) with a unified `.stepper` component — CSS and HTML restructure only, no JS interaction. The result: all step titles visible at a glance in a horizontal rail on desktop, a vertical accordion on mobile, phone bezel screenshots on "In the App" steps, and a static first-step-open state that Phase 7's JS controller can layer onto without structural changes.

</domain>

<decisions>
## Implementation Decisions

### HTML Structure
- **D-01:** Use an **items-only DOM** — no separate `.stepper__rail` element. HTML contains `.stepper__item × 5` for each stepper section, each item wrapping a `.stepper__header` (button) and `.stepper__panel` (detail content).
- **D-02:** On desktop (≥768px): `.stepper__item { display: contents }` — the item wrapper becomes layout-transparent, making `.stepper__header` and `.stepper__panel` direct children of the CSS Grid on `.stepper`. Headers land in grid row 1 (the visual rail); panels land in grid row 2 (spanning all columns). Only the active panel is shown.
- **D-03:** `.stepper__header` serves double duty as both the desktop tab button and the mobile accordion toggle. The spec's `.stepper__tab` / `.stepper__tab--active` classes do NOT appear — the implementation uses `.stepper__header` / `.stepper__header--active` throughout.
- **D-04:** Step 1's item is pre-marked active in HTML: `class="stepper__header stepper__header--active"`, `aria-expanded="true"` on the header. All other items: `class="stepper__header"`, `aria-expanded="false"`. Phase 7 JS moves the active class on click.
- **D-05:** `.stepper__content` inside each panel contains an `<h3>` heading (same text as `.stepper__title`) followed by description paragraphs. The title appears twice in the DOM: once in `.stepper__title` (always visible in the header/tab) and once as `<h3>` inside the panel (visible when step is expanded).
- **D-06:** Each `.stepper` block wraps one workflow phase. The section has two separate `.stepper` elements — one for "In the App" and one for "Importing into xLights" — each preceded by a `<p class="workflow__phase-title">` eyebrow label.

### Breakpoints
- **D-07:** A new `@media (max-width: 767px)` block is added for stepper-specific mobile rules. The existing `@media (max-width: 960px)` block stays unchanged for nav/carousel rules. Two breakpoints coexist — 960px for existing components, 767px for the stepper.
- **D-08:** Desktop stepper layout (≥768px) uses `display: grid; grid-template-columns: repeat(5, 1fr)` on `.stepper`. Mobile (≤767px) uses `display: block` on `.stepper` and `display: block` on `.stepper__item` (restoring normal stacking for accordion).

### Old Component Cleanup
- **D-09:** Old HTML (`<div class="workflow">`, `.workflow__phase`, `.workflow__steps`, `.step-card` blocks, `.numbered-list`) is **removed** and replaced with the new stepper markup.
- **D-10:** Old CSS rules are **commented out** (not deleted) as a safety net during transition. Rules to comment out:
  - Lines 565–568: `.workflow`
  - Lines 569–572: `.workflow__phase`
  - Lines 582–586: `.workflow__steps`
  - Lines 588–619: `.step-card`, `.step-card.visible`, `.step-card__num`, `.step-card h4`, `.step-card p`
  - Lines 621–654: `.quality-tiers`, `.tier-pill`, and tier colour variants (HTML for these is already commented out)
  - Lines 657–695: `.numbered-list` and related rules
  - Line 1006: `.workflow__steps { grid-template-columns: 1fr; }` inside the 960px media block
- **D-11:** `.workflow__phase-title` CSS (lines 573–580) is **kept** — same class and styles are reused for the eyebrow labels in the new stepper structure.

### Scroll Animation
- **D-12:** In `animatedEls` querySelectorAll (line 1615), replace `.step-card` with `.stepper__item` so the new stepper items receive scroll-reveal animation (fade-up on viewport entry).

### Claude's Discretion
- Exact CSS for hiding non-active panels on desktop: `display: none` vs `visibility: hidden` vs `opacity: 0; pointer-events: none` — use `display: none` as it removes the panel from the grid and avoids occupying row 2 invisibly.
- Whether to use `grid-row: 2` explicitly on `.stepper__panel` or rely on implicit grid placement — explicit is clearer.
- Exact IDs for `aria-controls` / panel `id` attributes — use descriptive kebab-case: `panel-app-1`, `panel-xlights-1`, etc.

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements & Roadmap
- `.planning/REQUIREMENTS.md` — STEP-01, STEP-05, STEP-06, STEP-07, STEP-08 are the five requirements this phase satisfies
- `.planning/ROADMAP.md` — Phase 6 success criteria (5 acceptance checks); Phase 7 dependency contract

### UI Design Contract (PRIMARY — read before touching any CSS or HTML)
- `.planning/phases/06-stepper-foundation/06-UI-SPEC.md` — Complete BEM contract, spacing tokens, typography, colors, phone bezel usage, screenshot assignments, accessibility contract, and interaction contract (CSS vs JS boundary). This is the authoritative visual spec.

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint: all CSS in `<style>`, all JS inline. No external libraries.
- `.planning/codebase/CONVENTIONS.md` — BEM naming, CSS token usage (`--transition`, `--accent`, `--surface`, `--shadow`), `/* ── Section ── */` banner format, `<!-- ══ Section ══ -->` HTML comment style
- `.planning/codebase/STRUCTURE.md` — Single `<style>` block structure; where to add new CSS sections

### Existing Code Locations to Modify
- `index.html` lines 565–695 — Old workflow CSS to comment out (`.workflow`, `.step-card`, `.numbered-list` and related)
- `index.html` line 1006 — `.workflow__steps` mobile override to comment out (inside the 960px media block)
- `index.html` lines 1374–1450 — `#how-it-works` section HTML to restructure (keep the `<section>`, `<div class="container">`, heading — replace inner `<div class="workflow">` block)
- `index.html` line 1615 — `animatedEls` querySelectorAll: replace `.step-card` with `.stepper__item`

### Prior Phase Decisions (carried forward)
- `.planning/phases/04-nav-download-cta/04-CONTEXT.md` — Phase 4 established 960px as the breakpoint for nav/carousel; Phase 6 adds 767px as a second breakpoint for the stepper only
- `.planning/phases/05-carousel-peek-view/05-CONTEXT.md` — Phone bezel width 240px must not change (carousel JS depends on it); stepper desktop media column reuses `.phone-bezel` at 240px unchanged

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.phone-bezel`, `.phone-bezel__frame`, `.phone-bezel__screen`, `.phone-bezel__notch` (used throughout): Reused unchanged inside `.stepper__media`. No new bezel CSS needed. Width stays 240px on desktop, 180px centered on mobile (per UI-SPEC).
- `.workflow__phase-title` CSS (lines 573–580): Kept as-is. The eyebrow label `<p class="workflow__phase-title">` is reused verbatim before each `.stepper` block.
- Screenshot files already present: `resources/screenshot/connect.jpeg`, `resources/screenshot/ar.jpeg`, `resources/screenshot/results.jpeg`, `resources/screenshot/preview.jpeg` — all four used in "In the App" steps 1–4.
- CSS tokens: `var(--accent)`, `var(--surface)`, `var(--bg)`, `var(--muted)`, `var(--text)`, `var(--shadow)`, `var(--radius-sm)`, `var(--transition)` — all used in new stepper CSS.

### Established Patterns
- `display: contents` removes the layout box but not the DOM node — `.stepper__item` remains in the accessibility tree. Safe to use.
- Single `@media (max-width: 960px)` block at bottom of `<style>` (line 967) — stepper's new 767px block goes after it (or before, keeping stepper rules together).
- Scroll-reveal pattern: `opacity: 0; transform: translateY(20px)` on base element, `opacity: 1; transform: translateY(0)` on `.visible` class added by IntersectionObserver. Apply same pattern to `.stepper__item`.
- BEM state class: `.stepper__header--active` follows the established modifier pattern (double hyphen).

### Integration Points
1. **CSS (new section):** Add `/* ── Stepper ── */` section in `<style>` after the commented-out workflow CSS. Include all stepper rules + the new 767px media block.
2. **HTML (how-it-works section):** Lines 1379–1448 — replace `<div class="workflow" aria-label="In-app workflow steps">…</div>` with two `.stepper` blocks. Keep `<section id="how-it-works">`, `.container`, heading, and `<span class="section-label">` unchanged.
3. **JS (animatedEls):** Line 1615 — change `.step-card` to `.stepper__item` in the querySelectorAll selector string.
4. **CSS (responsive):** Line 1006 — comment out `.workflow__steps { grid-template-columns: 1fr; }` since `.workflow__steps` no longer exists.

</code_context>

<specifics>
## Specific Ideas

- The items-only approach with `display: contents` means the spec's separate `.stepper__tab` BEM element is not used. `.stepper__header` is the single interactive element for both desktop and mobile. The CSS for desktop should style `.stepper__header` to look like a tab (matching the UI-SPEC's tab button spec).
- On desktop, `adjacent sibling selector` works through `display:contents`: `.stepper__header--active + .stepper__panel { display: block; }` correctly shows the active panel because header and panel are DOM siblings inside `.stepper__item` even though the item has no layout box.
- "In the App" step 5 (Export) has no screenshot — use `.stepper__body--text-only` modifier, `<div class="stepper__content">` fills full width via `flex: 1`.
- All 5 "Importing into xLights" steps have no screenshots — all use `.stepper__body--text-only`.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 6-Stepper Foundation*
*Context gathered: 2026-05-15*
