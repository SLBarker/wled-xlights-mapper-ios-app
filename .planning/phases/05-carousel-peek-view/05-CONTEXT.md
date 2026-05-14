# Phase 5: Carousel Peek-View - Context

**Gathered:** 2026-05-14
**Status:** Ready for planning

<domain>
## Phase Boundary

Rework the hero carousel so the active (centred) slide has partial views of both adjacent slides visible on either side. The active slide is centred; peeking slides are dimmed. Arrows overlay the peek strips rather than sitting outside the track. Navigation (arrows, dots, touch swipe, 4s auto-advance) continues to function. The carousel renders correctly at mobile widths with a smaller peek.

</domain>

<decisions>
## Implementation Decisions

### Peek Width
- **D-01:** Desktop peek: **70px each side**. Total visible width = 70 + 240 + 70 = **380px**. The `.hero-carousel__track-wrap` width changes from 240px to 380px. The clip boundary shifts outward accordingly.
- **D-02:** Peeking slides are dimmed to **opacity ~0.5**; the active slide remains at full opacity (1.0). Apply opacity via a CSS class toggled by `goTo()` (e.g., `.hero-carousel__slide--active` / default non-active opacity). Transition opacity with `var(--transition)`.

### Arrow Placement
- **D-03:** Arrows are **absolute-positioned inside the 380px viewport**, overlaid on the peeking strips — not flex siblings outside the track-wrap. Remove the `gap: 1.5rem` from `.hero-carousel__viewport` layout. Position arrows using `position: absolute; top: 50%; transform: translateY(-50%)` with `left` / `right` values that centre them over the peek strips (e.g., `left: 10px; right: 10px`). The `.hero-carousel__viewport` becomes `position: relative`.

### Edge Behavior
- **D-04:** **Natural edges** — no cyclic wrap in the peek layer. When slide 1 is active, the left peek area is empty (shows hero dark background). When slide 4 is active, the right peek area is empty.
- **D-05:** The **prev arrow is hidden** (`display: none` or `visibility: hidden`) when slide 1 is active. The **next arrow is hidden** when slide 4 is active. `goTo()` updates arrow visibility on every transition.

### Mobile
- **D-06:** At `@media (max-width: 960px)`, peek reduces to **40px each side**. Total visible: 40 + 240 + 40 = **320px**. Override `.hero-carousel__track-wrap` width to 320px and adjust arrow absolute positions in the existing 960px media block. Fits iPhone SE (375px viewport with 20px side padding leaves 335px usable).

### Navigation Logic
- **D-07:** JS `goTo()` must change from `translateX(-${current * 100}%)` (percentage-based, assumes track-wrap = slide width) to **pixel-based**: `translateX(-${current * 240}px)`. The track-wrap is now wider than one slide, so percentage no longer maps to one slide width.
- **D-08:** `goTo()` also updates `aria-hidden` on slides and updates dot indicators — existing behaviour preserved. Add: update arrow visibility (D-05) and update slide opacity class (D-02) on each call.

### Claude's Discretion
- Exact `left` / `right` pixel values for overlaid arrows — should visually centre over the peek strip; adjust to taste.
- Whether arrows use `visibility: hidden` (keeps layout space) or `display: none` (collapses space) at boundaries — use `visibility: hidden` to prevent layout shift if arrows affect overall width.
- Gap between track-wrap and left/right edge of viewport when arrows overlay — no extra padding needed since the arrow overlays sit inside the clip.
- `overflow: hidden` can be moved from `.hero-carousel__track-wrap` to a new containing wrapper if needed to avoid clipping overlaid arrows — planner's call.

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements
- `.planning/REQUIREMENTS.md` — CAR-01 and CAR-02 are the two requirements this phase satisfies
- `.planning/ROADMAP.md` — Phase 5 success criteria (3 acceptance checks)

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint: all CSS in `<style>`, all JS inline. No external libraries.
- `.planning/codebase/CONVENTIONS.md` — BEM naming, CSS token usage (`--transition`, `--accent`), comment banner format
- `.planning/codebase/ARCHITECTURE.md` — Single breakpoint at `@media (max-width: 960px)`; no second media block

### Existing Carousel Code to Modify
- `index.html` lines 330–418 — `.hero-carousel` CSS block (track-wrap width, viewport layout, arrow rules, slide rules, dot rules)
- `index.html` lines 970–978 — `@media (max-width: 960px)` carousel overrides — add 320px track-wrap width and arrow position overrides here
- `index.html` lines 1069–1153 — Carousel HTML: viewport, arrows, track-wrap, track, slides (4 slides), dots
- `index.html` lines 1692–1757 — Carousel JS: `goTo()`, timer, arrow/dot listeners, touch handlers

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.hero-carousel__slide` (lines 358–364): Already `flex: 0 0 240px; width: 240px` — slide width stays 240px; only the clip container changes.
- `.hero-carousel__arrow` (lines 367–392): Keep existing button styles; only position changes from flex layout to absolute. Reuse hover/focus-visible rules.
- `var(--transition)` token: Use for opacity transitions on non-active slides.
- `var(--accent)` / `var(--shadow)` tokens: Already used by arrow buttons — no hardcoded values.

### Established Patterns
- Single `@media (max-width: 960px)` block (line 970) — all mobile overrides for Phase 5 go into this block, not a new block.
- BEM modifiers: Add `.hero-carousel__slide--active` class for the active slide (full opacity). Non-active slides default to dimmed opacity via `.hero-carousel__slide` base rule.
- JS pattern: `goTo()` is the single source of truth for carousel state — all new state updates (opacity, arrow visibility) go inside `goTo()`, not in separate listeners.
- `will-change: transform` already on `.hero-carousel__track` — keep it.

### Integration Points
1. **CSS change (track-wrap):** `width: 240px` → `width: 380px`. Remove `overflow: hidden` from track-wrap if arrows need to overflow it; apply `overflow: hidden` to the viewport instead.
2. **CSS change (viewport):** Make `position: relative`. Remove `gap: 1.5rem` from flex layout. Add `overflow: hidden` if track-wrap no longer clips.
3. **CSS change (arrows):** Switch from flex-child to `position: absolute; top: 50%; transform: translateY(-50%); left: Xpx / right: Xpx`.
4. **CSS change (slide base):** Add `opacity: 0.5; transition: opacity var(--transition)` to `.hero-carousel__slide`. Add `.hero-carousel__slide--active { opacity: 1; }`.
5. **CSS change (mobile):** In `@media (max-width: 960px)`, add `width: 320px` for `.hero-carousel__track-wrap` and update arrow `left`/`right` to match 40px peek.
6. **JS change (goTo):** Replace `translateX(-${current * 100}%)` with `translateX(-${current * 240}px)`. Add: toggle `.hero-carousel__slide--active` class. Add: hide/show arrows based on `current === 0` / `current === total - 1`.

</code_context>

<specifics>
## Specific Ideas

- The dimming approach (opacity on non-active slides) communicates focus without needing scale transforms — keeps the layout stable.
- At the left boundary (slide 1 active), the empty left peek area exposes the dark hero section background — this should look intentional, not broken. The arrow hiding (D-05) ensures no orphaned button sits over empty space.
- Arrow visibility uses `visibility: hidden` rather than `display: none` to avoid layout shift if the button affects flex/absolute containment.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 5-Carousel Peek-View*
*Context gathered: 2026-05-14*
