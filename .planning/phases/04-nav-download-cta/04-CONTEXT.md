# Phase 4: Nav & Download CTA - Context

**Gathered:** 2026-05-14
**Status:** Ready for planning

<domain>
## Phase Boundary

Fix the desktop navigation so links never wrap before the hamburger triggers; move the Download CTA out of `.site-nav__links` so it remains visible in the nav bar at every viewport width; add the download SVG icon to the nav CTA; and update the CTA hover to match the hero button style (`translateY(-2px)` lift + blue glow).

</domain>

<decisions>
## Implementation Decisions

### Hamburger Breakpoint
- **D-01:** New breakpoint: `@media (max-width: 960px)` — replaces the current `768px`. The single `@media` block in `index.html` (lines 967–990) is updated to 960px. All existing responsive rules inside that block continue to apply at the new threshold.

### Download CTA Structure
- **D-02:** The Download CTA `<a>` element must be extracted from `<ul class="site-nav__links">` and placed as a **direct child of `.site-nav__inner`**, positioned between the links list and the hamburger toggle button. The element keeps the `.site-nav__cta` class.
- **D-03:** The CTA shows **icon + text at all viewport widths** — no mobile-only text truncation or icon-only variant. The pill with icon+label is the same at desktop and mobile widths.
- **D-04:** The CTA must be removed from `.site-nav__drawer` HTML entirely. NAV-02 says it is never in the drawer. Remove the `<li>` entry from the drawer `<ul>`.

### CTA Styling & Icon
- **D-05:** Keep `.site-nav__cta` class (do not switch to `.btn .btn-primary`). The compact pill sizing (`padding: 0.4rem 1rem`) is correct for the 56px nav bar.
- **D-06:** Add `display: inline-flex; align-items: center; gap: 0.4rem;` to `.site-nav__cta` to support the icon.
- **D-07:** Icon: reuse the same SVG path as the hero Download button — circle (`M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2z`) + down-arrow (`M8 12l4 4 4-4M12 8v8`). Size: `14×14` in the nav (hero uses `16×16`).
- **D-08:** Update `.site-nav__cta:hover` from `opacity: 0.88; transform: scale(1.03)` to `transform: translateY(-2px); box-shadow: 0 12px 40px rgba(0,113,227,0.5);` — matching the hero `.btn-primary:hover` style exactly.

### Claude's Discretion
- Minor gap/padding tuning on `.site-nav__cta` if the icon + text pill looks cramped in context.
- Whether to add `transition: transform var(--transition), box-shadow var(--transition)` to `.site-nav__cta` base rule (recommended — it currently only transitions `opacity` and `transform`).
- The `.site-nav__drawer .site-nav__cta` CSS block (lines 957–965) can be removed since no drawer CTA element will exist.

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements
- `.planning/REQUIREMENTS.md` — NAV-01, NAV-02, NAV-03, UI-01 are the four requirements this phase satisfies
- `.planning/ROADMAP.md` — Phase 4 success criteria (5 acceptance checks)

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint: all CSS in `<style>`, all JS inline. No external libraries.
- `.planning/codebase/CONVENTIONS.md` — BEM naming, CSS token usage, `--transition` token, comment formats
- `.planning/codebase/STRUCTURE.md` — Where to modify nav CSS and nav HTML

### Existing Nav Code to Modify
- `index.html` lines 58–118 — `.site-nav`, `.site-nav__inner`, `.site-nav__links`, `.site-nav__cta` CSS
- `index.html` lines 875–965 — Hamburger toggle CSS and mobile drawer CSS (including `.site-nav__drawer .site-nav__cta`)
- `index.html` lines 967–990 — `@media (max-width: 768px)` responsive block — change threshold to 960px
- `index.html` lines 999–1041 — Nav HTML: `.site-nav__inner`, `.site-nav__links`, toggle button, `.site-nav__drawer`

### Hero Reference (style to match)
- `index.html` lines 243–244 — `.btn:hover` and `.btn-primary:hover` — the exact hover style to replicate in nav CTA
- `index.html` line 1057 — Hero Download button SVG icon markup — use the same paths in the nav CTA at 14×14

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.site-nav__cta` class (lines 110–118): Existing blue pill. Keep it — just add flex + icon support and fix hover.
- Hero download SVG icon (line 1057): `stroke="currentColor" stroke-width="2.5"` — copy the paths, change size to 14×14.
- CSS tokens: `--accent`, `--transition`, `--shadow` — use for any new CSS; never hardcode colours.

### Established Patterns
- Drawer is a sibling element in DOM, not a mutation of the desktop nav (established Phase 1) — the CTA move follows the same logic.
- `@media (max-width: 768px)` is the single breakpoint block — update the threshold value in-place; do not add a second media block.
- BEM comment banners: the nav section already has `/* ── Nav ── */` and `/* ── Mobile nav drawer ── */` separators; no new banners needed for this phase.

### Integration Points
1. **HTML change (nav bar):** Extract `<li><a href="#download" class="site-nav__cta">Download</a></li>` from `<ul class="site-nav__links">` → replace with `<a href="#download" class="site-nav__cta">…</a>` as a direct flex child of `.site-nav__inner`, before the hamburger `<button>`. Add SVG icon inside the `<a>`.
2. **HTML change (drawer):** Remove the `<li><a href="#download" class="site-nav__cta">Download</a></li>` from `.site-nav__drawer ul`.
3. **CSS change (CTA base):** Add `display: inline-flex; align-items: center; gap: 0.4rem;` to `.site-nav__cta`. Extend `transition` to include `box-shadow`.
4. **CSS change (CTA hover):** Replace `opacity: 0.88; transform: scale(1.03)` with `transform: translateY(-2px); box-shadow: 0 12px 40px rgba(0,113,227,0.5);`.
5. **CSS change (breakpoint):** Change `@media (max-width: 768px)` to `@media (max-width: 960px)`.
6. **CSS cleanup:** Remove `.site-nav__drawer .site-nav__cta` block (lines 957–965) since no drawer CTA will exist.

</code_context>

<specifics>
## Specific Ideas

- The hover style must be **exactly** `transform: translateY(-2px); box-shadow: 0 12px 40px rgba(0,113,227,0.5);` — the same values as `.btn-primary:hover` — no deviation.
- SVG icon `aria-hidden="true"` (decorative, text label is present).
- The CTA `<a>` element is no longer wrapped in `<li>` when moved out of the links list — it is a bare flex child of `.site-nav__inner`.
- At mobile widths, the nav bar flex layout becomes: `[logo — flex-grow] [Download CTA] [hamburger]`. The logo can use `flex: 1` or `margin-right: auto` to push CTA and hamburger to the right.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 4-Nav & Download CTA*
*Context gathered: 2026-05-14*
