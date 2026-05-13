# Codebase Structure

**Analysis Date:** 2026-05-13

## Directory Layout

```
wled-xlights-mapper-ios-app/    # Project root / static site root
├── index.html                  # Complete landing page (HTML + CSS + JS, self-contained)
├── resources/                  # Static media assets referenced by index.html
│   ├── screenshot/             # App screenshots displayed in phone bezel mockups
│   │   ├── ar.jpeg             # AR scanning screen
│   │   ├── connect.jpeg        # mDNS controller connect screen
│   │   ├── preview.jpeg        # 3D preview screen
│   │   └── results.jpeg        # Export / results screen
│   ├── video/                  # Short screen recordings used as autoplay loops
│   │   ├── discovery.mp4       # mDNS discovery feature video (used in features section)
│   │   ├── discovery.mov       # Source MOV (not referenced in HTML, kept as source file)
│   │   └── 3dpreview.mp4       # 3D preview feature video (used in features section)
│   └── icons/                  # Small UI toggle icons used in the 3D preview feature row
│       ├── led-index-toggle.png
│       ├── quantize-model-toggle.png
│       └── wire-toggle.png
└── .planning/                  # GSD planning artefacts (not deployed)
    └── codebase/               # Codebase map documents
```

## Directory Purposes

**Project root:**
- Purpose: Static site root — the directory served by the static host
- Contains: `index.html` (the entire site), `resources/` (all media)
- Key files: `index.html`

**`resources/screenshot/`:**
- Purpose: JPEG app screenshots rendered inside CSS phone bezel components
- Contains: Four JPEG files, one per key app screen (connect, AR scan, 3D preview, results)
- Key files: `resources/screenshot/connect.jpeg`, `resources/screenshot/ar.jpeg`, `resources/screenshot/preview.jpeg`, `resources/screenshot/results.jpeg`

**`resources/video/`:**
- Purpose: Short MP4 screen recordings used as muted autoplay loops inside phone bezel mockups in the features section
- Contains: Two MP4 files referenced in `index.html`; one MOV source file not referenced in HTML
- Key files: `resources/video/discovery.mp4` (features section, mDNS discovery row), `resources/video/3dpreview.mp4` (features section, 3D preview row)
- Note: `resources/video/discovery.mov` is a source/original file — it is not referenced anywhere in `index.html`

**`resources/icons/`:**
- Purpose: Small 24×24 px PNG icons for the view-option icon list in the 3D preview feature row
- Contains: Three PNG icon images
- Key files: `resources/icons/wire-toggle.png`, `resources/icons/led-index-toggle.png`, `resources/icons/quantize-model-toggle.png`

**`.planning/codebase/`:**
- Purpose: GSD codebase map documents consumed by `/gsd-plan-phase` and `/gsd-execute-phase`
- Generated: No — written by GSD mapper agents
- Committed: Optional (project-dependent convention)

## Key File Locations

**Entry Point:**
- `index.html`: The complete website — HTML structure, all inline CSS, all inline JavaScript

**CSS (inline):**
- `index.html` lines 10–796: All styles inside `<style>` in `<head>`. Organised by component with comment separators (`/* ── Section name ── */`)

**JavaScript (inline):**
- `index.html` lines 1316–1336: Single `<script>` block at bottom of `<body>`. Registers an `IntersectionObserver` for scroll-reveal animations.

**Design Tokens:**
- `index.html` lines 14–29: `:root` CSS custom properties block defining the full colour/spacing/shadow palette

**Responsive Rules:**
- `index.html` lines 784–795: Single `@media (max-width: 768px)` block — the only breakpoint

**Media Assets:**
- Screenshots: `resources/screenshot/*.jpeg`
- Videos: `resources/video/*.mp4`
- Icons: `resources/icons/*.png`

## Naming Conventions

**Files:**
- HTML: lowercase, single word — `index.html`
- Screenshots: lowercase descriptive noun — `connect.jpeg`, `ar.jpeg`, `preview.jpeg`, `results.jpeg`
- Videos: lowercase descriptive noun — `discovery.mp4`, `3dpreview.mp4`
- Icons: lowercase kebab-case describing the UI toggle — `led-index-toggle.png`, `wire-toggle.png`, `quantize-model-toggle.png`

**Directories:**
- Lowercase single nouns — `resources`, `screenshot`, `video`, `icons`

**CSS Classes (in `index.html`):**
- Block: lowercase hyphenated noun — `.site-nav`, `.phone-bezel`, `.feature-card`, `.step-card`
- Element: BEM-style double underscore — `.site-nav__inner`, `.phone-bezel__frame`, `.feature-card__icon`
- Modifier: BEM-style double hyphen (where used) — `.btn-primary`, `.btn-ghost`, `.tier-green`, `.tier-red`
- State: single word `.visible` (applied by JavaScript)

**HTML IDs (section anchors):**
- Lowercase hyphenated — `#hero`, `#pitch`, `#features`, `#how-it-works`, `#export`, `#requirements`, `#tips`, `#privacy`
- Heading IDs follow `{section}-title` pattern — `hero-title`, `features-title`, `req-title`, etc.

## Where to Add New Code

**New page section:**
- Add a `<section id="new-section" class="section" aria-labelledby="new-section-title">` block inside `<main id="main-content">` in `index.html`
- Add corresponding CSS in the `<style>` block, following the `/* ── Section name ── */` comment separator convention
- Add a nav link inside `.site-nav__links` in the `<nav>` element

**New screenshot:**
- Place the JPEG in `resources/screenshot/`
- Reference via relative path `resources/screenshot/filename.jpeg` in an `<img>` tag inside a `.phone-bezel__screen` element

**New feature video:**
- Place the MP4 in `resources/video/`
- Use the existing `<video autoplay loop muted playsinline>` pattern with `src="resources/video/filename.mp4"`

**New scroll-animated element type:**
- Add the CSS class selector to the `document.querySelectorAll(...)` call in the inline `<script>` (line 1318)
- Add initial state CSS (`opacity: 0; transform: translateY(…)`) and `.visible` state CSS to the `<style>` block

**New icon (view option list):**
- Place PNG in `resources/icons/`
- Add an `<li>` inside `.icon-list` using `<img src="resources/icons/filename.png" alt="…">` pattern (see `index.html` lines 1015–1026)

## Special Directories

**`.planning/`:**
- Purpose: GSD agent artefacts — codebase maps, phase plans
- Generated: Partially (map docs generated by agents, plan docs generated during planning)
- Committed: No hard rule; treat as development tooling similar to `.vscode/`

---

*Structure analysis: 2026-05-13*
