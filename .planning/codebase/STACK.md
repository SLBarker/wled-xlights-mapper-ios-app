# Technology Stack

**Analysis Date:** 2026-05-13

## Languages

**Primary:**
- HTML5 — Single-page landing page (`index.html`)
- CSS3 — Inline within `<style>` block in `index.html` (no external stylesheet)
- JavaScript (ES2015+) — Inline `<script>` block in `index.html`

**Secondary:**
- None

## Runtime

**Environment:**
- Static file — no server-side runtime required
- Served via GitHub Pages (inferred from commit history: "initial pages deploy")

**Package Manager:**
- None — no `package.json`, `requirements.txt`, or any dependency manifest present
- Lockfile: Not applicable

## Frameworks

**Core:**
- None — pure HTML/CSS/JS, zero framework dependencies

**Testing:**
- None — no test framework present

**Build/Dev:**
- None — no build pipeline, bundler, or transpiler; the single `index.html` is the deployable artifact

## Key Dependencies

**Critical:**
- Google Fonts CDN — `Inter` typeface loaded via `<link>` from `https://fonts.googleapis.com`
  - Weights: 300, 400, 500, 600, 700
  - Fallback stack: `-apple-system, BlinkMacSystemFont, sans-serif`

**Infrastructure:**
- None — all rendering logic is self-contained in `index.html`

## Configuration

**Environment:**
- No environment variables — fully static, no server configuration
- No `.env` files present

**Build:**
- No build config files (no `webpack.config.*`, `vite.config.*`, `tsconfig.json`, etc.)

## Browser APIs Used

Vanilla JavaScript in `index.html` uses:
- `IntersectionObserver` — scroll-triggered CSS class additions for entrance animations
- `document.querySelectorAll` — DOM element selection

## CSS Architecture

All styles are written inline within a single `<style>` block in `index.html`:
- CSS Custom Properties (`--bg`, `--accent`, `--radius-md`, etc.) defined on `:root`
- `clamp()` for fluid typography
- CSS Grid and Flexbox for layout
- `@keyframes fadeUp` for hero entrance animations
- `@media (max-width: 768px)` breakpoint for mobile layout
- `backdrop-filter` for frosted-glass navigation bar

## Media Assets

All assets are local — no CDN for images or video:
- `resources/screenshot/*.jpeg` — 4 app screenshot images
- `resources/icons/*.png` — 3 UI icon images
- `resources/video/*.mp4` — 2 autoplay looping demo videos
- `resources/video/discovery.mov` — source video (unused in page, `.mp4` variant used)

## Platform Requirements

**Development:**
- Any text editor — no build step, no toolchain
- Git for version control

**Production:**
- Static file host (GitHub Pages confirmed via git log)
- No server-side language or database required
- HTTPS recommended (required for `IntersectionObserver` and `backdrop-filter` in some browsers)

---

*Stack analysis: 2026-05-13*
