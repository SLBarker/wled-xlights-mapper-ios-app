# Phase 8: Discoverability & Legal — Pattern Map

**Mapped:** 2026-05-18
**Files analyzed:** 4 new/modified files
**Analogs found:** 4 / 4 (all patterns sourced directly from index.html — single-file project)

---

## File Classification

| New/Modified File | Role | Data Flow | Closest Analog | Match Quality |
|-------------------|------|-----------|----------------|---------------|
| `index.html` `<head>` block | config | request-response (SEO metadata) | `index.html` lines 3–9 (existing meta/link tags) | exact |
| `index.html` `<footer>` block | markup | request-response (static link) | `index.html` lines 2040–2044 (existing footer) | exact |
| `favicon.svg` | asset | static | none — new file type | n/a |
| `privacy.html` | page | request-response (static page) | `index.html` nav + body + footer patterns | role-match |

---

## Pattern Assignments

### `index.html` `<head>` — OG tags, Twitter card, canonical, favicon links

**Analog:** `index.html` lines 3–9 (existing `<head>` meta/link block)

**Existing head block** (lines 3–9) — insert new tags between line 7 and line 8:
```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>WLED to XLights — Accurate LED Mapping for xLights</title>
<meta name="description" content="Convert your WLED LEDs to a 3D xLights model. Skip the manual editing. Accurate LED mapping using a standard iPhone — LiDAR not required.">
<!-- INSERT HERE — between line 7 and line 8 -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

**HTML comment style** (from CONVENTIONS.md line 97, confirmed throughout index.html):
- HTML section delimiters: `<!-- ══ Section Name ══ -->`
- CSS section delimiters: `/* ── Section Name ── */`

**Tags to insert** — exact markup from UI-SPEC.md (lines 231–246):
```html
<!-- ══ Social & Discovery ══ -->
<meta property="og:type"        content="website">
<meta property="og:url"         content="https://slbarker.github.io/wled-xlights-mapper-ios-app/">
<meta property="og:title"       content="WLED to XLights — Accurate LED Mapping for xLights">
<meta property="og:description" content="Convert your WLED LEDs to a 3D xLights model. Skip the manual editing. Accurate LED mapping using a standard iPhone — LiDAR not required.">
<meta property="og:image"       content="https://slbarker.github.io/wled-xlights-mapper-ios-app/resources/og-image.png">
<meta name="twitter:card"        content="summary_large_image">
<meta name="twitter:title"       content="WLED to XLights — Accurate LED Mapping for xLights">
<meta name="twitter:description" content="Convert your WLED LEDs to a 3D xLights model. Skip the manual editing. Accurate LED mapping using a standard iPhone — LiDAR not required.">
<meta name="twitter:image"       content="https://slbarker.github.io/wled-xlights-mapper-ios-app/resources/og-image.png">
<link rel="canonical"  href="https://slbarker.github.io/wled-xlights-mapper-ios-app/">
<link rel="icon"       type="image/svg+xml" href="favicon.svg">
<link rel="icon"       type="image/png"     href="favicon.png">
<link rel="apple-touch-icon" href="apple-touch-icon.png">
```

---

### `index.html` `<footer>` — Privacy Policy link addition

**Analog:** `index.html` lines 2040–2044 (existing footer)

**Existing footer HTML** (lines 2040–2044) — exact current state:
```html
<!-- ══ Footer ══ -->
<footer role="contentinfo">
  <p>WLED to XLights — Convert your WLED LEDs to a 3D xLights model.</p>
  <p style="margin-top:0.5rem;">App Store and iOS are trademarks of Apple Inc. WLED and xLights are independent open-source projects.</p>
</footer>
```

**Existing footer CSS** (lines 1019–1030) — exact rules that the new `<p>` and `<a>` will inherit:
```css
/* ── Footer ── */
footer {
  background: #0a0a0f;
  border-top: 1px solid rgba(255,255,255,0.06);
  padding: 3rem 2rem;
  text-align: center;
}

footer p {
  font-size: 0.8125rem;
  color: rgba(255,255,255,0.35);
}
```

**New `<p>` to append** — add as last child of `<footer role="contentinfo">`, after the existing two `<p>` elements. Follows the `style="margin-top:..."` inline override pattern used on the second existing `<p>`:
```html
<p style="margin-top:0.5rem;">
  <a href="privacy.html"
     aria-label="Privacy Policy for WLED to xLights Mapper">Privacy Policy</a>
</p>
```

**New CSS rule to add for the footer link** — add after `footer p` rule (after line 1030), before `/* ── Animations ── */`:
```css
footer a {
  color: inherit;
  text-decoration: underline;
  transition: color var(--transition);
}

footer a:hover {
  color: rgba(255,255,255,0.7);
}

footer a:focus-visible {
  outline: 2px solid #0071e3;
  outline-offset: 2px;
}
```

---

### `favicon.svg` — SVG monogram favicon

**Analog:** none — first SVG file in project. Spec from UI-SPEC.md lines 151–158.

**Complete SVG markup:**
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="6" fill="#0a0a0f"/>
  <text
    x="16" y="16"
    font-family="Inter,system-ui,sans-serif"
    font-weight="700"
    font-size="14"
    fill="#0071e3"
    text-anchor="middle"
    dominant-baseline="central">WX</text>
</svg>
```

Key values:
- Background fill: `#0a0a0f` — matches `footer` background (index.html line 1021) and site dark surface
- Text fill: `#0071e3` — matches `--accent` token (index.html line 19)
- `rx="6"` — matches `--radius-sm: 16px` proportionally scaled; keeps corners consistent with card aesthetic
- No gradients, no strokes, no shadows — minimal per D-07 and UI-SPEC

---

### `privacy.html` — Standalone privacy policy page

**Analog:** `index.html` — full page structure, nav CSS, body CSS, footer CSS

**Head pattern** — copy from index.html lines 3–9, adapt for privacy page:
```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Privacy Policy — WLED to xLights Mapper</title>
<link rel="canonical" href="https://slbarker.github.io/wled-xlights-mapper-ios-app/privacy.html">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

**CSS token block** — copy verbatim from index.html lines 14–29:
```css
:root {
  --bg:        #f5f5f7;
  --surface:   #ffffff;
  --text:      #1d1d1f;
  --muted:     #6e6e73;
  --accent:    #0071e3;
  --accent2:   #34c759;
  --warn:      #ff9f0a;
  --danger:    #ff3b30;
  --radius-sm: 16px;
  --radius-md: 28px;
  --radius-lg: 40px;
  --shadow:    0 4px 32px rgba(0,0,0,0.08);
  --shadow-lg: 0 16px 64px rgba(0,0,0,0.14);
  --transition: 0.35s cubic-bezier(0.25,0.46,0.45,0.94);
}
```

**Body CSS** — copy from index.html lines 33–39:
```css
body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.7;
  overflow-x: hidden;
}
```

**Nav CSS pattern** — copy frosted-glass nav from index.html lines 58–125. For privacy.html the nav is simplified (no hamburger, no links list, no CTA — just logo left + back link right). Reuse these specific rules:
```css
/* index.html lines 58–76 */
.site-nav {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(245,245,247,0.72);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border-bottom: 1px solid rgba(0,0,0,0.06);
  padding: 0 2rem;
}

.site-nav__inner {
  max-width: 1100px;
  margin: auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
}

/* index.html lines 78–84 */
.site-nav__logo {
  font-weight: 700;
  font-size: 1rem;
  color: var(--text);
  text-decoration: none;
  letter-spacing: -0.02em;
}
```

**Nav HTML pattern** — simplified from index.html lines 1255–1301 (no hamburger button, no links list, no drawer — just the inner flex container):
```html
<nav class="site-nav" aria-label="Site navigation">
  <div class="site-nav__inner">
    <span class="site-nav__logo">WLED to xLights Mapper</span>
    <a href="index.html"
       class="site-nav__back"
       aria-label="Back to WLED xLights Mapper landing page">← Back to landing page</a>
  </div>
</nav>
```

**Back link CSS** (new rule, no analog — derived from UI-SPEC interaction states):
```css
.site-nav__back {
  font-size: 0.8125rem;
  color: var(--accent);
  text-decoration: none;
  min-height: 44px;
  display: inline-flex;
  align-items: center;
  transition: opacity var(--transition);
}

.site-nav__back:hover { opacity: 0.75; }

.site-nav__back:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

**Content section heading CSS** (new rules for privacy.html `<h2>` section headings):
```css
.privacy-section h2 {
  font-size: 1.25rem;
  font-weight: 700;
  line-height: 1.3;
  color: var(--text);
  margin-bottom: 0.5rem;
}

.privacy-section p {
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.7;
  color: var(--text);
}
```

**Privacy card content** — source text from index.html lines 2008–2034 (5 cards). Expand each `<h4>` + `<p>` pair into a `<section class="privacy-section">` with an `<h2>` heading:

| Card (index.html) | Heading | Body text source |
|---|---|---|
| Line 2009–2013 | No Personal Data Captured | "WLED to XLights captures no personal data whatsoever." |
| Line 2014–2018 | On-Device Storage Only | "The only information stored is WLED controller connection details..." |
| Line 2019–2023 | No Analytics or Tracking | "No analytics. No tracking. No data leaves your device." |
| Line 2024–2028 | Fully Offline | "The app works entirely on local Wi-Fi. No account, no cloud sync..." |
| Line 2029–2033 | Camera Processed On-Device | "Camera and ARKit data is processed entirely on-device..." |

**Contact section** — uses `wled.2.xlights@gmail.com` (from CONTEXT.md specifics). Render email as `<a href="mailto:wled.2.xlights@gmail.com">` styled with `color: var(--accent)`.

**Footer pattern** — copy existing footer from index.html lines 2040–2044, simplified for privacy page (no trademark disclaimer needed; just copyright line):
```html
<footer role="contentinfo">
  <p>WLED to xLights Mapper</p>
</footer>
```

---

## Shared Patterns

### CSS Token Usage
**Source:** `index.html` lines 14–29 (`:root` block)
**Apply to:** All new CSS in index.html and all CSS in privacy.html

Never hardcode colour or shadow values. Always reference tokens:
- `var(--bg)` for page background
- `var(--surface)` for card/nav backgrounds
- `var(--text)` for body text
- `var(--accent)` for interactive links and CTA
- `var(--muted)` for secondary/hint text
- `var(--transition)` for all `transition:` values

Exception: footer uses literal `#0a0a0f` and `rgba(255,255,255,0.35)` — there is no token for the dark-surface background; copy this literal pattern from lines 1021 and 1029.

### HTML Comment Style
**Source:** `index.html` throughout (e.g., line 1255, 2040)
**Apply to:** All new HTML blocks in index.html and privacy.html

```html
<!-- ══ Section Name ══ -->
```

### Focus-Visible Outline
**Source:** `index.html` `.skip-link:focus` (line 55), `.site-nav__links a:focus-visible` (line 103)
**Apply to:** All interactive elements in both files

```css
element:focus-visible {
  outline: 2px solid #0071e3;
  outline-offset: 2px;
}
```

Use `#0071e3` literal (matches `--accent` value) to avoid cascade issues on the dark footer background.

### Inline `margin-top` Override Pattern
**Source:** `index.html` line 2043 — `style="margin-top:0.5rem;"` on footer `<p>`
**Apply to:** New Privacy Policy `<p>` in footer

Spacing between footer `<p>` elements uses inline `style="margin-top:0.5rem;"` — not a CSS class rule. Match this pattern exactly.

### Google Fonts Link Pattern
**Source:** `index.html` lines 8–9
**Apply to:** `privacy.html` `<head>`

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

---

## No Analog Found

| File | Role | Data Flow | Reason |
|------|------|-----------|--------|
| `favicon.svg` | asset | static | No SVG files exist in the project; spec fully defined in UI-SPEC.md lines 151–158 |
| `favicon.png` | asset | static | Binary rasterized asset; generated from favicon.svg at 32×32px |
| `apple-touch-icon.png` | asset | static | Binary rasterized asset; generated from favicon.svg at 180×180px with proportionally larger padding |

---

## Metadata

**Analog search scope:** `index.html` (entire single-file project), `.planning/codebase/CONVENTIONS.md`
**Files scanned:** 3 (`index.html`, `08-CONTEXT.md`, `08-UI-SPEC.md`, `CONVENTIONS.md`)
**Pattern extraction date:** 2026-05-18
