# Phase 7: Stepper Interaction - Context

**Gathered:** 2026-05-15
**Status:** Ready for planning

<domain>
## Phase Boundary

Add a JS controller inside the existing single `<script>` block that handles three behaviors for both stepper components: (1) click any step header to expand its panel and collapse the previously open one, (2) auto-expand Step 1 when each `.stepper` element first scrolls into the viewport (fire-once), and (3) arrow key stepping between step headers. No new HTML or CSS structure is needed — Phase 6 built everything; Phase 7 layers behavior on top.

</domain>

<decisions>
## Implementation Decisions

### Scroll Auto-Expand (STEP-04)
- **D-01:** The scroll auto-expand fires **once only** — the first time each `.stepper` enters the viewport. After firing, the observer unobserves the element. If the user has navigated to Step 3 and then scrolls away and back, Step 3 remains open.
- **D-02:** Observe **each `.stepper` element independently**, not the parent `#how-it-works` section. This ensures each stepper auto-expands precisely when it becomes visible, even if the two steppers enter the viewport at different scroll positions.
- **D-03:** The scroll trigger should use a **new dedicated IntersectionObserver** (not the existing `io` animation observer). The existing `io` fires at `threshold: 0.12` and unobserves immediately after adding `.visible` — extending it would couple two unrelated concerns. The stepper auto-expand observer can use a similar low threshold (e.g., `0.15`) to fire when the stepper rail becomes visible.

### Mobile Panel Animation
- **D-04:** Animate panel height on mobile. JS opens a panel by setting `panel.style.height = panel.scrollHeight + 'px'` and closes by setting `panel.style.height = '0'`. The CSS `transition: height var(--transition)` wired in Phase 6 plays the animation. On `transitionend`, the inline style is cleared for the open panel (so it returns to CSS-managed `height: auto`), and the closed panel's inline style is also cleared (CSS default `height: 0` takes over).
- **D-05:** No scroll-into-view after expand. Panel expands in place; user scrolls manually if needed.
- **D-06:** Desktop panels continue to use `display: none / block` via the CSS sibling selector — no JS height management needed on desktop.

### Arrow Key Navigation
- **D-07:** JS adds arrow key navigation between step headers within each stepper. Direction is determined at keydown time: if `window.innerWidth > 767`, left/right arrow keys cycle through step headers; if `window.innerWidth <= 767`, up/down arrow keys cycle. No resize listener — check at keydown time.
- **D-08:** Arrow key behavior: pressing the key moves focus to the next/previous header (wrapping at boundaries). The focused header is NOT automatically activated — the user must press Enter/Space to expand it. This follows the disclosure widget pattern (focus-follows-key, activate-on-explicit-action) rather than the tablist pattern.
- **D-09:** The keydown listener is added to each `.stepper` container and uses event delegation — `e.target.closest('.stepper__header')` — so a single listener per stepper handles all headers.

### Claude's Discretion
- Exact threshold value for the auto-expand IntersectionObserver — `0.15` is reasonable (fires when 15% of the stepper is visible).
- Whether to add `tabIndex="-1"` to non-active headers to reduce tab stops on desktop — standard tablist practice but not required for disclosure widgets. Claude decides based on what feels cleanest given the `<button>` pattern in use.
- TransitionEnd cleanup approach — whether to use a one-shot `transitionend` listener per transition or always clear inline styles after a timeout.

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements & Roadmap
- `.planning/REQUIREMENTS.md` — STEP-02, STEP-03, STEP-04 are the three requirements this phase satisfies
- `.planning/ROADMAP.md` — Phase 7 success criteria (3 acceptance checks); Phase 6 dependency contract

### Phase 6 Artifacts (PRIMARY — read before touching any code)
- `.planning/phases/06-stepper-foundation/06-CONTEXT.md` — Complete Phase 6 decisions including HTML structure (D-01 through D-12), BEM class names, aria attribute setup, and the CSS/JS boundary contract
- `.planning/phases/06-stepper-foundation/06-UI-SPEC.md` — Interaction contract table (Phase 6 vs Phase 7 responsibilities), accessibility contract, height animation CSS spec

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint: all JS stays in the inline `<script>` block at end of `<body>`. No external libraries.
- `.planning/codebase/CONVENTIONS.md` — `const` throughout, camelCase variables, IntersectionObserver pattern (threshold + unobserve), template literals

### Existing Code Locations to Modify
- `index.html` inline `<script>` (~line 2029) — Add stepper controller after the existing `animatedEls` IntersectionObserver and before the lazy video observer. All new JS goes here.
- `index.html` mobile `.stepper__panel` CSS (~line 1202) — The `height: 0; overflow: hidden; transition: height var(--transition)` is already set; Phase 7 JS drives it via inline styles. No CSS changes needed unless the `:first-child` static open rule needs to be removed once JS takes over.

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- Existing `io` IntersectionObserver pattern (line ~2033): `new IntersectionObserver(callback, { threshold: 0.12 })` with `io.unobserve(entry.target)` after firing — Phase 7 replicates this fire-once pattern for the stepper auto-expand observer.
- `.stepper__header--active` CSS modifier: already defined in Phase 6; JS adds/removes it on click.
- `aria-expanded` attribute: already present on all `.stepper__header` buttons set to `"true"` (Step 1) or `"false"` (Steps 2–5); JS updates it on click.
- `aria-controls` + panel `id` attributes: already wired; JS can use `btn.getAttribute('aria-controls')` to find the corresponding panel element.

### Established Patterns
- **IntersectionObserver fire-once**: `io.unobserve(entry.target)` immediately after triggering action — matches existing scroll-reveal and lazy-video patterns.
- **Event delegation**: `container.addEventListener('click', e => { const btn = e.target.closest('.stepper__header'); if (!btn) return; ... })` — cleaner than individual button listeners.
- **const throughout**: No `var`, no `let` for references that don't change.
- **No external dependencies**: All DOM APIs — `querySelectorAll`, `closest`, `getAttribute`, `classList`, `style.height`, `scrollHeight`.

### Integration Points
1. **JS (stepper controller):** Add after `animatedEls.forEach(el => io.observe(el))` (line ~2046). Controller has three parts: click handler, auto-expand observer, keyboard handler — all scoped to `.stepper` containers.
2. **CSS (mobile first-child override):** The `:first-child` rule at line ~1216 that sets `height: auto; padding: 0 0 16px` on the first panel will conflict with JS-managed inline heights. Phase 7 should either (a) remove this CSS rule and let JS manage the initial state entirely, or (b) initialize JS state by reading the existing DOM state. Option (a) is cleaner.
3. **HTML (no changes):** The existing HTML is the contract — Phase 7 reads it, doesn't change it.

</code_context>

<specifics>
## Specific Ideas

- The scroll auto-expand behavior matches the requirement "Step 1 is already expanded without any user action" — the fire-once approach satisfies this while preserving user navigation choices after first entry.
- For the height animation, using `panel.scrollHeight` at open time ensures the correct height even if panel content changes. Clearing the inline style on `transitionend` avoids a fixed pixel height that could break if content is later modified.
- Arrow key navigation wraps at boundaries (pressing right on the last step goes to Step 1; pressing left on Step 1 goes to the last step) — standard ARIA pattern.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 7-Stepper Interaction*
*Context gathered: 2026-05-15*
