# Coding Conventions

**Analysis Date:** 2026-05-13

## Project Nature

This is a single-file static HTML landing page. There is no build system, no package manager, no transpilation, and no JavaScript framework. All conventions are HTML/CSS/JS authoring conventions applied within `index.html`.

## File Structure

**Single-file architecture:**
- All HTML markup, CSS styles, and JavaScript live in `index.html`
- Static assets are organised under `resources/` by type:
  - `resources/icons/` — UI toggle icons (PNG)
  - `resources/screenshot/` — App screenshots (JPEG)
  - `resources/video/` — Demo screen recordings (MP4, MOV)

## Naming Patterns

**CSS Classes (BEM-style):**
- Block: `.site-nav`, `.phone-bezel`, `.feature-card`, `.step-card`, `.req-card`, `.privacy-card`
- Element (double underscore): `.site-nav__inner`, `.site-nav__logo`, `.site-nav__links`, `.phone-bezel__frame`, `.phone-bezel__screen`, `.phone-bezel__notch`, `.phone-bezel__caption`, `.feature-card__icon`, `.req-card__icon`
- Modifier (double hyphen, applied as additional class): `.site-nav__cta`, `.btn-primary`, `.btn-ghost`, `.tier-green`, `.tier-yellow`, `.tier-orange`, `.tier-red`, `.limitations-col`
- State class (single word): `.visible` — toggled by JS for scroll-reveal animations

**HTML IDs:**
- Section anchors use kebab-case: `#hero`, `#pitch`, `#features`, `#how-it-works`, `#export`, `#requirements`, `#tips`, `#privacy`, `#download`
- Heading IDs for `aria-labelledby` use kebab-case: `hero-title`, `pitch-title`, `features-title`, `how-title`, `export-title`, `req-title`, `tips-title`, `privacy-title`
- Feature-specific heading IDs: `feat-discovery-title`, `feat-scan-title`, `feat-quality-title`, `feat-preview-title`

**Resource files:**
- Icons: kebab-case descriptive names (`led-index-toggle.png`, `quantize-model-toggle.png`, `wire-toggle.png`)
- Screenshots: single lowercase noun (`ar.jpeg`, `connect.jpeg`, `preview.jpeg`, `results.jpeg`)
- Videos: lowercase descriptor (`3dpreview.mp4`, `discovery.mp4`, `discovery.mov`)

**JavaScript variables:**
- camelCase: `animatedEls`, `io`, `entry`, `siblings`, `idx`

## Code Style

**Formatting:**
- No automated formatter configured (no `.prettierrc`, `.editorconfig`, or equivalent)
- Indentation: 2-space soft tabs throughout HTML and CSS
- CSS rules: one declaration per line inside rule blocks
- Inline styles are used sparingly for one-off layout overrides only (e.g., `style="margin-top:1rem;"` on `<p>` elements in the export section, `style="width:240px;"` on a phone bezel)

**Linting:**
- No linting tool configured (no `.eslintrc`, `biome.json`, or equivalent)

## CSS Architecture

**CSS Custom Properties (Design Tokens):**
All design tokens are defined on `:root` in `index.html` (lines 14–29):
```css
:root {
  --bg, --surface, --text, --muted, --accent, --accent2, --warn, --danger
  --radius-sm, --radius-md, --radius-lg
  --shadow, --shadow-lg
  --transition
}
```
Always use tokens; never hardcode colour values or shadow values in component rules.

**Organisation pattern (within the single `<style>` block):**
CSS sections are delimited with inline comments using a consistent banner format:
```css
/* ── Section Name ── */
```
Sections appear in document order: Reset & tokens → Skip link → Nav → Layout → Hero → Components → Sections → Animations → Responsive.

**Responsive breakpoint:**
- Single breakpoint: `@media (max-width: 768px)` at bottom of the `<style>` block
- Mobile-first is NOT used; desktop layout is default, mobile is overridden

**Animation:**
- Scroll-reveal: elements start with `opacity: 0; transform: translateY(...)` in CSS, gain `.visible` class via JS IntersectionObserver
- Hero entrance: CSS `@keyframes fadeUp` with `animation-delay` staggered via inline `animation` shorthand on hero children
- Hover transitions: use `var(--transition)` token consistently

## HTML Patterns

**Semantic structure:**
- `<nav>` for site navigation with `aria-label="Site navigation"`
- `<main id="main-content">` wrapping all page content
- `<section>` elements with `aria-labelledby` referencing heading IDs
- `<article>` for self-contained feature rows and feature cards
- `<footer role="contentinfo">`
- Skip link (`<a href="#main-content" class="skip-link">`) as first child of `<body>`

**ARIA usage:**
- `aria-hidden="true"` on decorative elements (notch divs, SVG icons, emoji icons)
- `aria-label` on interactive/landmark elements without visible text labels
- `role="list"` / `role="listitem"` on non-`<ul>` list constructs (grid containers, card groups)
- `aria-labelledby` linking sections to their `<h2>` headings

**Comment style:**
- HTML section comments use double-box style: `<!-- ══ Section Name ══ -->`
- CSS section comments use dash-box style: `/* ── Section Name ── */`

**`<video>` pattern for autoplay demos:**
```html
<video
  src="resources/video/filename.mp4"
  autoplay
  loop
  muted
  playsinline
  style="width:100%; height:100%; object-fit:cover; display:block;"
  aria-label="Description of recording">
</video>
```
Always include `muted` and `playsinline` alongside `autoplay`. Always include `aria-label`.

**Placeholder pattern when screenshot is not yet available:**
Use `.phone-bezel__placeholder` with an inline SVG icon and uppercase label text instead of an `<img>`.

## JavaScript Patterns

**Single script block** at end of `<body>`, no external scripts:
```javascript
const animatedEls = document.querySelectorAll('...');
const io = new IntersectionObserver((entries) => {
  entries.forEach((entry, i) => {
    if (entry.isIntersecting) {
      const siblings = Array.from(entry.target.parentElement.children);
      const idx = siblings.indexOf(entry.target);
      entry.target.style.transitionDelay = `${idx * 60}ms`;
      entry.target.classList.add('visible');
      io.unobserve(entry.target);
    }
  });
}, { threshold: 0.12 });

animatedEls.forEach(el => io.observe(el));
```
- Use `const` throughout; no `var`
- Template literals for dynamic strings
- Unobserve after first intersection (fire-once)
- Stagger delay is computed from sibling index at `60ms` intervals

## Import Organization

No module imports. No external JS dependencies. Google Fonts loaded via `<link>` in `<head>`:
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

## Error Handling

Not applicable — no application logic. No network calls, no form submissions, no user input processing.

## Comments

**CSS:** Section-delimiter comments are mandatory. Inline comments used for non-obvious rules (e.g., `/* tilted hero phones */`, `/* Inline feature phone is slightly smaller */`).

**HTML:** Section-delimiter comments for each major page section. Commented-out content (disabled feature cards, disabled scan quality indicators) is preserved inside `<!-- ... -->` blocks rather than deleted.

## Typography Scale

Defined via `clamp()` for fluid responsive headings:
- Hero title: `clamp(3rem, 9vw, 6rem)`
- Section titles: `clamp(2rem, 5vw, 3rem)`
- Hero hook: `clamp(1rem, 2.5vw, 1.25rem)`
- Tagline: `clamp(0.875rem, 2vw, 1rem)`
- Body text: `1.0625rem` with `line-height: 1.75`

---

*Convention analysis: 2026-05-13*
