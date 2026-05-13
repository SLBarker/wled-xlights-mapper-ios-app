# Phase 2: Hero Carousel - Context

**Gathered:** 2026-05-13
**Status:** Ready for planning

<domain>
## Phase Boundary

Replace the static `.hero__phones` fan strip (3 rotated phone bezels) in the `#hero` section with a single-slide-at-a-time carousel. Visitors see one phone mockup at a time and can advance through all 4 slides using arrow buttons, dot indicators, swipe (mobile), and auto-advance. Each slide consists of a phone bezel mockup and a caption text label that transition together.

</domain>

<decisions>
## Implementation Decisions

### Slide Composition
- **D-01:** 4 slides total — Connect (`connect.jpeg`), Scan (`ar.jpeg`), Preview & Export (`preview.jpeg`), Results (`results.jpeg`).
- **D-02:** Each slide has a caption: "Connect", "Scan", "Preview & Export", "Results".
- **D-03:** The carousel structure must be extensible — adding a new slide should require only adding a new slide element to the HTML (data-driven or markup-driven, not hardcoded JS index counts).

### Transition Style
- **D-04:** Slide left/right transition using CSS `transform: translateX` + `transition`. The phone mockup and its associated caption text transition together in the same direction.
- **D-05:** Caption text transitions with the image as a single unit — they are co-located in the slide markup and move together, not independently.

### Auto-advance
- **D-06:** Auto-advance enabled. Interval: 4 seconds per slide.
- **D-07:** Auto-advance pauses on mouse hover (mouseenter/mouseleave). Auto-advance resumes when hover ends.
- **D-08:** Auto-advance also pauses when the user manually interacts (prev/next/dot click or swipe). Resume after interaction is at Claude's discretion (reasonable: resume timer after 1 cycle, or after hover ends).
- **D-09:** Carousel loops — after the last slide, advances back to the first.

### Controls Layout
- **D-10:** Prev/next arrow buttons flank the phone mockup — one on the left, one on the right of the `.phone-bezel` container.
- **D-11:** Arrow buttons are circular icon-only buttons with SVG chevron icons, styled consistently with the existing design system (use CSS tokens: `--surface`, `--accent`, `--radius-lg`, `--shadow`).
- **D-12:** Dot indicators sit below the phone mockup (and below the caption). One dot per slide; active dot is visually distinct (filled/accent color).
- **D-13:** On mobile (≤768px), arrows are retained but may shrink — do not hide them. Swipe left/right also advances the carousel (touch events).

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements
- `.planning/REQUIREMENTS.md` — HERO-01, HERO-02, HERO-03 are the requirements this phase satisfies
- `.planning/ROADMAP.md` — Phase 2 success criteria (single phone visible, prev/next + dots, swipe on mobile)

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint: all CSS stays in `<style>`, all JS stays in inline `<script>`. No external libraries.
- `.planning/codebase/CONVENTIONS.md` — BEM naming, CSS token usage, JS patterns (const, template literals, IntersectionObserver), section comment format
- `.planning/codebase/STRUCTURE.md` — Where to add new CSS sections, JS additions go in the single `<script>` block at bottom of `<body>`

### Existing Hero Code
- `index.html` lines 246–337 — Current `.hero__phones` and `.phone-bezel` CSS to replace/extend
- `index.html` lines 955–1005 — Current hero section HTML (`.hero__phones` markup to replace with carousel)
- `index.html` lines 884–887 — Mobile responsive rules for hero phones (to replace with carousel mobile rules)

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.phone-bezel` component (CSS lines 257–337): Frame, screen, notch, caption fully defined. Carousel slides should wrap individual `.phone-bezel` elements — do not duplicate the bezel CSS.
- Design tokens on `:root` (lines 14–29): `--bg`, `--surface`, `--accent`, `--accent2`, `--shadow`, `--radius-sm/md/lg`, `--transition` — use for arrow button and dot indicator styling.
- SVG chevron pattern: the hamburger button in Phase 1 uses inline SVG — follow the same pattern for carousel arrows (width/height/viewBox, `aria-hidden="true"`, stroke-based).

### Established Patterns
- BEM naming: new carousel classes should follow `.hero-carousel`, `.hero-carousel__track`, `.hero-carousel__slide`, `.hero-carousel__arrow`, `.hero-carousel__dots`, `.hero-carousel__dot`.
- JS pattern: `const`, template literals, event listeners. The carousel JS goes in the existing `<script>` block alongside the IntersectionObserver code.
- CSS section comments: add `/* ── Hero carousel ── */` banner for the new carousel CSS block.
- HTML section comments: keep `<!-- ══ Hero ══ -->` structure, replace only the `.hero__phones` div.

### Integration Points
- Replace `.hero__phones` div (lines 973–1003) with the new carousel markup.
- Replace/remove `.hero__phones` CSS (lines 247–254) and the tilted-phone transforms (lines 335–337).
- Replace the mobile `.hero__phones` override (lines 884–887) with carousel mobile styles.
- Existing `.phone-bezel` CSS stays — it's reused inside carousel slides and elsewhere on the page.
- Touch/swipe events are new — add to the carousel JS block, isolated from the existing IntersectionObserver code.

</code_context>

<specifics>
## Specific Ideas

- The fan strip currently tilts phone 1 by -5deg, phone 3 by +5deg. These transforms are on `.hero__phones .phone-bezel:nth-child(N)` — remove them; carousel slides should be upright.
- The current mobile rule hides phones 1 and 3 (`display: none`). The carousel replaces this with a single-phone-visible layout natively — remove the hide rule.
- Caption text transitions with the slide as a unit (not a separate fade). Keep caption inside the slide element.
- "Extensible" means: to add a new slide, a developer adds one `<div class="hero-carousel__slide">` block to the HTML. JS should derive slide count from DOM, not from a hardcoded constant.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 2-Hero Carousel*
*Context gathered: 2026-05-13*
