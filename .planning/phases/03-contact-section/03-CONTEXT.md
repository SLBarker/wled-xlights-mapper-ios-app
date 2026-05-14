# Phase 3: Contact Section - Context

**Gathered:** 2026-05-14
**Status:** Ready for planning

<domain>
## Phase Boundary

Add a Contact/Feedback section to the page, placed between `#privacy` and `<footer>`. The section allows visitors to send feedback directly to the developer via a single `mailto:` CTA button. It must be styled consistently with the light-background sections (Features, Tips), include a nav link in both desktop nav and hamburger drawer, and require no backend or JavaScript.

</domain>

<decisions>
## Implementation Decisions

### Email Address
- **D-01:** Recipient address: `wled.2.xlights@gmail.com` (dedicated support address, not personal Gmail).
- **D-02:** `mailto:` link format: subject only — `mailto:wled.2.xlights@gmail.com?subject=Feedback%20for%20WLED%20xLights%20Mapper`. No pre-filled body.

### Section Content & Copy
- **D-03:** Section label (eyebrow): `Contact`
- **D-04:** Section heading: `Share your feedback`
- **D-05:** Section includes a short supporting paragraph before the button — something inviting (e.g. "Found a bug or have a feature request? I'd love to hear from you."). Exact copy at Claude's discretion, but it should be warm and brief (1–2 sentences).
- **D-06:** CTA button label: `Send Feedback` (matches CONT-02 exactly).
- **D-07:** Button should use the existing `.btn.btn-primary` class.

### Section Background
- **D-08:** Light background — same theme as `#features` and `#tips` sections. This creates a visual break between the dark `#privacy` section and the dark `<footer>`.

### Navigation
- **D-09:** Add a `Contact` nav link to both the desktop `.site-nav__links` list and the hamburger `.site-nav__drawer` list, alongside existing section links. The link href is `#contact`.

### Section Placement
- **D-10:** Section `id="contact"` is inserted after `</section>` of `#privacy` and before `<footer>`. It uses the standard `<section class="section" aria-labelledby="contact-title">` pattern.

### Claude's Discretion
- Paragraph copy (1–2 sentences, warm and inviting — see D-05 for guidance).
- Scroll-reveal animation for the CTA button or section body (follow the existing `opacity: 0; transform: translateY(16px)` + `.visible` IntersectionObserver pattern if appropriate).

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements
- `.planning/REQUIREMENTS.md` — CONT-01, CONT-02, CONT-03 are the requirements this phase satisfies
- `.planning/ROADMAP.md` — Phase 3 success criteria (section visible and styled, mailto CTA, pre-filled subject)

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint: all CSS stays in `<style>`, all JS stays in inline `<script>`. No external libraries.
- `.planning/codebase/CONVENTIONS.md` — BEM naming, CSS token usage, section comment format
- `.planning/codebase/STRUCTURE.md` — Where to add new CSS sections and new nav links; where to insert new `<section>` elements

### Existing Sections to Match
- `index.html` — `#features` section (light background reference) and `#tips` section (light background reference): match their background style, section padding, and `.section-label` + `.section-title` markup pattern
- `index.html` — `.site-nav__links` (lines ~988–993) and `.site-nav__drawer` (lines ~1016–1023): add `Contact` link to both

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.btn` and `.btn-primary` classes (CSS ~lines 217–244): ready-made button styling with hover transform and box-shadow. Use `<a href="mailto:..." class="btn btn-primary">Send Feedback</a>` — no new button CSS needed.
- `.section`, `.section-label`, `.section-title`, `.section-body` classes: fully defined structural classes; new Contact section uses them as-is.
- CSS tokens on `:root` (lines 14–29): `--bg`, `--surface`, `--accent`, `--shadow`, `--radius-*`, `--transition` — use for any Contact-specific styling.

### Established Patterns
- Section markup: `<section id="{id}" class="section" aria-labelledby="{id}-title"><div class="container">…</div></section>`
- Section CSS comment: `/* ── Contact ── */` banner for the new block.
- HTML section comment: `<!-- ══ Contact ══ -->` before the `<section>` tag.
- Nav links: `<li><a href="#contact">Contact</a></li>` in both `.site-nav__links` and `.site-nav__drawer .site-nav__links`.
- Light section background: no override needed — `.section` default is the light `--bg` background. Confirm by checking `#features` CSS.

### Integration Points
- Insert `<!-- ══ Contact ══ -->` + `<section id="contact" …>` block after the closing `</section>` of `#privacy` (line ~1546) and before `<!-- ══ Footer ══ -->`.
- Add Contact CSS block in `<style>` after the `/* ── Footer ── */` comment (or before it), grouped with section styles.
- Add nav links in two places: desktop `<ul class="site-nav__links">` and mobile `<div class="site-nav__drawer">` equivalent.
- If scroll-reveal is added, include the selector (e.g. `.contact-cta`) in the `document.querySelectorAll(…)` call in the existing `<script>` block.

</code_context>

<specifics>
## Specific Ideas

- The `mailto:` URL must be: `mailto:wled.2.xlights@gmail.com?subject=Feedback%20for%20WLED%20xLights%20Mapper` — both the address and subject are locked values.
- The button must be a plain `<a>` tag (not `<button>`), since it navigates to a mailto: URI.
- Light-background sections on the page use `--bg` as default (no explicit background override on `.section`). Verify this before assuming Contact needs a background override.
- Keep the Contact section centered and compact — this is a simple CTA moment, not a feature showcase. No phone mockups, no grid, no cards.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 3-Contact Section*
*Context gathered: 2026-05-14*
