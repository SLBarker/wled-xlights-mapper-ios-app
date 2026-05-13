<!-- refreshed: 2026-05-13 -->
# Architecture

**Analysis Date:** 2026-05-13

## System Overview

```text
┌─────────────────────────────────────────────────────────────┐
│                    Browser (User Agent)                      │
│                      `index.html`                            │
└──────────┬──────────────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────────────────────┐
│                Single-File HTML Document                     │
│  ┌──────────────┬────────────────────┬────────────────────┐ │
│  │  <head>      │     <body>         │   <script>         │ │
│  │  CSS styles  │  Semantic HTML     │  Intersection      │ │
│  │  (inline)    │  sections + media  │  Observer JS       │ │
│  └──────────────┴────────────────────┴────────────────────┘ │
└──────────┬──────────────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────────────────────┐
│                    Static Assets (Local)                     │
│   `resources/screenshot/`  `resources/video/`               │
│   `resources/icons/`                                         │
└─────────────────────────────────────────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────────────────────┐
│               External CDN (Google Fonts)                    │
│   fonts.googleapis.com — Inter typeface                      │
└─────────────────────────────────────────────────────────────┘
```

## Component Responsibilities

| Component | Responsibility | File |
|-----------|----------------|------|
| `<head>` CSS block | All layout, theming, responsive rules, animations | `index.html` lines 10–796 |
| `<nav class="site-nav">` | Sticky top navigation with anchor links and App Store CTA | `index.html` lines 803–817 |
| `<section id="hero">` | Full-screen hero with phone mockup strip and CTA buttons | `index.html` lines 821–878 |
| `<section id="pitch">` | Two-column problem/solution elevator pitch | `index.html` lines 880–900 |
| `<section id="features">` | Alternating feature rows (with phone mockups/video) + secondary feature cards | `index.html` lines 902–1081 |
| `<section id="how-it-works">` | Numbered workflow phases (in-app steps + xLights import) | `index.html` lines 1083–1160 |
| `<section id="export">` | Export description with results screenshot | `index.html` lines 1162–1196 |
| `<section id="requirements">` | Requirements card grid | `index.html` lines 1198–1235 |
| `<section id="tips">` | Two-column tips and limitations lists | `index.html` lines 1237–1270 |
| `<section id="privacy">` | Dark-background privacy assurance cards | `index.html` lines 1272–1306 |
| `<footer>` | Minimal legal and attribution text | `index.html` lines 1310–1314 |
| Inline `<script>` | IntersectionObserver scroll-reveal animation controller | `index.html` lines 1316–1336 |

## Pattern Overview

**Overall:** Self-contained single-file static landing page

**Key Characteristics:**
- All CSS is authored inline within the `<style>` block in `<head>` — no external stylesheets or CSS preprocessors
- All JavaScript is a single inline `<script>` at the end of `<body>` — no build step, no bundler, no external JS libraries
- No server-side rendering — the page is a flat HTML file served as a static asset
- External dependency is limited to Google Fonts CDN (Inter typeface) and nothing else
- Media assets are referenced with relative paths from `resources/`

## Layers

**Presentation Layer:**
- Purpose: Render the landing page and communicate app features to prospective users
- Location: `index.html`
- Contains: CSS design tokens, component styles, responsive rules, semantic HTML sections, scroll-reveal JavaScript
- Depends on: `resources/` for images and video, Google Fonts CDN for Inter
- Used by: End users via web browser; GitHub Pages or equivalent static host

**Static Assets Layer:**
- Purpose: Provide visual evidence (screenshots, video) of the iOS app's interface
- Location: `resources/screenshot/`, `resources/video/`, `resources/icons/`
- Contains: JPEG screenshots, MP4/MOV videos, PNG UI icon images
- Depends on: Nothing
- Used by: `index.html` via `<img>`, `<video>` elements

## Data Flow

### Page Load

1. Browser requests `index.html` from static host
2. HTML parsed; inline CSS applied immediately — no flash of unstyled content
3. Browser fetches Inter font from `fonts.googleapis.com` (non-blocking via `display=swap`)
4. Browser fetches local media: `resources/screenshot/*.jpeg`, `resources/video/*.mp4`, `resources/icons/*.png`
5. Inline `<script>` registers `IntersectionObserver` on animated elements; elements remain `opacity:0` until they scroll into view

### Scroll Animation Flow

1. User scrolls the page
2. `IntersectionObserver` fires when a `.feature-row`, `.feature-card`, `.step-card`, `.req-card`, `.tip-item`, or `.privacy-card` element crosses the 12% viewport threshold
3. A stagger delay is calculated from the element's index among its siblings (`idx * 60ms`)
4. The `.visible` CSS class is added, triggering the `opacity` and `transform` transition
5. The observer immediately unobserves the element (one-shot animation)

**State Management:**
- No client-side state. All content is static HTML. The IntersectionObserver holds no persistent state — each element fires once.

## Key Abstractions

**Phone Bezel Component (HTML pattern):**
- Purpose: Reusable CSS-styled phone frame used to display app screenshots and inline videos
- Examples: `index.html` lines 841–876 (hero strip), lines 912–933 (feature row with video), lines 1179–1193 (export section)
- Pattern: `.phone-bezel > .phone-bezel__frame > .phone-bezel__screen > (.phone-bezel__notch + img|video|.phone-bezel__placeholder)`

**CSS Design Token System:**
- Purpose: Centralised colour palette, spacing, and transition values via CSS custom properties
- Location: `index.html` `:root` block, lines 14–29
- Tokens: `--bg`, `--surface`, `--text`, `--muted`, `--accent`, `--accent2`, `--warn`, `--danger`, `--radius-sm/md/lg`, `--shadow`, `--shadow-lg`, `--transition`

**Section Pattern:**
- Purpose: Consistent padding and max-width container for every page section
- Pattern: `<section class="section"><div class="container">…</div></section>`
- Sections alternate background between `var(--bg)` (`#f5f5f7`) and `var(--surface)` (`#ffffff`) for visual rhythm. The hero and privacy sections use dark backgrounds (`#0a0a0f`).

**Quality Tier Pill System:**
- Purpose: Communicates LED scan quality categories with colour-coded pills
- Classes: `.tier-pill.tier-green`, `.tier-pill.tier-yellow`, `.tier-pill.tier-orange`, `.tier-pill.tier-red`
- Location: `index.html` lines 527–559 (CSS), lines 977–983 (usage in features section)

## Entry Points

**Page Entry:**
- Location: `index.html`
- Triggers: HTTP GET from browser (static host, GitHub Pages, etc.)
- Responsibilities: Delivers the complete page — styles, content, images, video, and animation logic — in a single document

**Navigation Anchors:**
- Location: `index.html` `<nav>` links (lines 808–815) and hero CTA buttons (lines 832–837)
- Triggers: User click or direct URL hash
- Targets: `#hero`, `#features`, `#how-it-works`, `#export`, `#requirements`, `#tips`, `#privacy`, `#download`
- Note: `#download` resolves to the `id="download"` on the App Store CTA `<a>` element (line 832) — there is no separate download section

## Architectural Constraints

- **No build step:** The page must remain deployable by serving `index.html` directly — no compilation, bundling, or preprocessing is used or expected
- **No JavaScript frameworks:** All interactivity is native browser API (`IntersectionObserver`, CSS transitions). Do not introduce React, Vue, or similar.
- **Single file:** All styles and scripts must remain inline in `index.html`. Do not split into separate `.css` or `.js` files unless explicitly restructuring for a build pipeline.
- **Static hosting:** No server-side logic. The architecture assumes a CDN or static file host (e.g., GitHub Pages).
- **External network:** Only `fonts.googleapis.com` is contacted at runtime. All other assets are local.
- **Global state:** None. The inline `<script>` creates a single `IntersectionObserver` instance (`io`) and an `animatedEls` NodeList — both scoped to the script block with no module system.

## Anti-Patterns

### Adding `<link rel="stylesheet">` external files

**What happens:** CSS is extracted to a separate file and linked from `<head>`
**Why it's wrong:** Adds a render-blocking request and breaks the self-contained nature; deployment requires serving multiple files correctly
**Do this instead:** Keep all CSS in the inline `<style>` block in `index.html`

### Inline `style=""` for layout properties

**What happens:** One-off layout or sizing values are set via `style=""` on elements (e.g., `style="width:240px;"` in the export section, line 1180)
**Why it's wrong:** Scatters layout decisions outside the central CSS block, making responsive overrides harder
**Do this instead:** Define a utility class or extend an existing component class in the `<style>` block

## Error Handling

**Strategy:** Not applicable — no dynamic logic, no API calls, no error states

**Patterns:**
- Broken image/video: browser renders alt text or empty space; no JS fallback is implemented
- Font load failure: body falls back to `-apple-system, BlinkMacSystemFont, sans-serif` (declared in `body` font-family stack)

## Cross-Cutting Concerns

**Accessibility:** `aria-label`, `aria-labelledby`, `aria-hidden`, `role="list"/"listitem"/"contentinfo"` attributes applied throughout. Skip-to-content link present at `index.html` line 801.
**Responsive design:** Single `@media (max-width: 768px)` breakpoint in the `<style>` block (lines 784–795) collapses two-column grids to single-column and hides the nav link list.
**SEO:** `<title>`, `<meta name="description">`, and `lang="en"` set in `<head>`.

---

*Architecture analysis: 2026-05-13*
