# Phase 11: coming-soon-cta - Context

**Gathered:** 2026-05-18
**Status:** Ready for planning

<domain>
## Phase Boundary

Replace the two broken `#download` anchors (nav pill + hero button) with a clearly disabled "Coming Soon" state. Both CTAs must communicate unavailability through text and appearance alone — no interaction, no navigation, no hover animation. No new sections, no backend, no JS framework additions.

</domain>

<decisions>
## Implementation Decisions

### Icon Treatment
- **D-01:** Remove the download-arrow SVG icon entirely from both CTAs — nav pill and hero button become text-only. No replacement icon (clock, lock, etc.) — "Coming Soon" text is the full signal.

### Disabled Visual Style
- **D-02:** Opacity `0.6` (60%) on both disabled CTAs — readable but clearly non-interactive. Matches Apple-style disabled convention.
- **D-03:** `cursor: not-allowed` on both CTAs (per CTA-03 requirement).
- **D-04:** Hover animations fully suppressed — `transform: none`, no `box-shadow` enhancement on hover. The button must not lift or glow when moused over.
- **D-05:** No tooltip or additional visual cue — "Coming Soon" text is self-explanatory. No `title` attribute needed.
- **D-06:** Keep existing blue accent background (`var(--accent)`) — no color/tint change. Opacity alone communicates disabled state.

### Accessibility / HTML Approach
- **D-07:** Remove `href` attribute from both `<a>` elements — no href means no navigation without JS. Combined with `aria-disabled="true"`, this satisfies CTA-02 (non-interactive) and is the cleanest approach for a static page.
- **D-08:** Add `aria-disabled="true"` to both elements — keyboard focus is preserved (no tabindex removal), screen readers announce disabled state. Per roadmap: "aria-disabled pattern preferred over removing tabindex".
- **D-09:** Use `pointer-events: none` in CSS as belt-and-suspenders alongside no-href — ensures hover states don't fire even if href is accidentally restored. Pointer-events only blocks mouse/touch, not keyboard — no accessibility regression.

### Claude's Discretion
- CSS selector strategy — `[aria-disabled="true"]` attribute selector vs. a new `.cta--disabled` class is Claude's call. Attribute selector is preferred (single source of truth with the HTML attribute).
- Whether to apply disabled CSS to both elements via a shared rule or two separate rules — Claude's call.

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements
- `.planning/REQUIREMENTS.md` — CTA-01, CTA-02, CTA-03 are the three active requirements for this phase
- `.planning/ROADMAP.md` §Phase 11 — success criteria (5 items), UI hint flag, and aria-disabled guidance

### Source File
- `index.html` — single deployable artifact; all changes inline. Key elements:
  - Line ~1340: nav pill `<a href="#download" class="site-nav__cta">`
  - Line ~1372: hero button `<a href="#download" class="btn btn-primary" id="download">`
  - Lines 153–168: `.site-nav__cta` + `.site-nav__cta:hover` CSS rules
  - Lines 267–294: `.btn`, `.btn-primary`, `.btn:hover`, `.btn-primary:hover` CSS rules

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- None required — this phase modifies existing elements, adds no new components.

### Established Patterns
- `<a>` elements styled as buttons via CSS classes (`.site-nav__cta`, `.btn.btn-primary`) — the existing pattern continues; no element-type change needed.
- Inline `<style>` block for all CSS — disabled state rules go here, not in a separate file.
- `aria-hidden="true"` already used on SVG icons — same pattern, icons being removed entirely this phase.

### Integration Points
- Both CTAs currently link to `id="download"` on the hero button itself — this self-referential anchor is removed when `href` is cleared. No other links point to `#download` in the page.
- The mobile nav drawer (lines 1347–1357) does NOT have a Download link — only the desktop nav pill changes.

</code_context>

<specifics>
## Specific Ideas

- No specific references beyond requirements — implementation is straightforward text + attribute + CSS changes on two existing elements.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 11-coming-soon-cta*
*Context gathered: 2026-05-18*
