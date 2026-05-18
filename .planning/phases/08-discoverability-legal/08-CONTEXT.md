# Phase 8: Discoverability & Legal - Context

**Gathered:** 2026-05-18
**Status:** Ready for planning

<domain>
## Phase Boundary

Add Open Graph social meta tags, a favicon + Apple touch icon, and a privacy policy footer link to the existing `index.html`. Also create a new `privacy.html` page in the repo root. All `index.html` changes are additive — no existing markup, CSS, or JS is modified. `privacy.html` is a new standalone file styled to match the site.

</domain>

<decisions>
## Implementation Decisions

### Open Graph Meta Tags (DISC-01)
- **D-01:** Add `og:title`, `og:description`, `og:image`, `og:url`, and `og:type` to `<head>` in `index.html`.
- **D-02:** `og:title` reuses the existing `<title>` value: `WLED to XLights — Accurate LED Mapping for xLights`
- **D-03:** `og:description` reuses the existing `<meta name="description">` value: `Convert your WLED LEDs to a 3D xLights model. Skip the manual editing. Accurate LED mapping using a standard iPhone — LiDAR not required.`
- **D-04:** `og:image` uses the existing `resources/og-image.png` file. Absolute URL: `https://slbarker.github.io/wled-xlights-mapper-ios-app/resources/og-image.png`
- **D-05:** `og:url` is the canonical page URL: `https://slbarker.github.io/wled-xlights-mapper-ios-app/`
- **D-06:** `og:type` is `website`.

### Favicon & Apple Touch Icon (DISC-02)
- **D-07:** Create an SVG favicon with a "WX" monogram — dark background (`#0a0a0f`, matching the site's body background) with accent-coloured text (matching `--accent` CSS token: `#2dd4bf`). File: `resources/favicon.svg` (or `favicon.svg` in repo root).
- **D-08:** Add a PNG fallback favicon (32×32px) for browsers that don't support SVG favicons. File: `favicon.png` in repo root.
- **D-09:** Apple touch icon: `apple-touch-icon.png` at 180×180px in repo root. Same "WX" monogram design, slightly larger padding.
- **D-10:** Wire in `<head>` with: `<link rel="icon" type="image/svg+xml" href="favicon.svg">`, `<link rel="icon" type="image/png" href="favicon.png">`, `<link rel="apple-touch-icon" href="apple-touch-icon.png">`.

### Privacy Policy Footer Link (LEGL-01)
- **D-11:** Create `privacy.html` in the repo root. Content is built from the existing privacy card text (5 privacy points from `#privacy` section) expanded to cover App Store compliance requirements: explicitly states no personal data collection, no tracking, no analytics, no third-party sharing, developer contact email, app name, and effective date.
- **D-12:** `privacy.html` is styled to match the main site: dark background, Inter font loaded from Google Fonts (Phase 10 will migrate this too), a simple nav header with the app name and a "← Back" link to `index.html`.
- **D-13:** Footer in `index.html` gets a visible `Privacy Policy` link: `<a href="privacy.html">Privacy Policy</a>` added to the existing `<footer>` element, on a new `<p>` below the existing content.

### Canonical URL
- **D-14:** Canonical base URL: `https://slbarker.github.io/wled-xlights-mapper-ios-app/`
- **D-15:** Add `<link rel="canonical" href="https://slbarker.github.io/wled-xlights-mapper-ios-app/">` to `<head>` alongside the OG tags.

### Claude's Discretion
- Exact SVG markup for the "WX" favicon — use a simple `viewBox="0 0 32 32"` with a rect fill and centered text element. Keep it minimal.
- Whether to include `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image` (Twitter/X summary card tags) alongside OG tags — standard practice to include both; Claude decides.
- Exact footer link styling — match the existing footer `<p>` text style; no special treatment needed.
- Whether `privacy.html` should include a `<link rel="canonical">` pointing to its own URL — standard SEO practice; Claude decides.

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements & Roadmap
- `.planning/REQUIREMENTS.md` — DISC-01, DISC-02, LEGL-01 are the three requirements this phase satisfies; acceptance criteria defined here
- `.planning/ROADMAP.md` — Phase 8 success criteria (3 checks: social preview, favicon, privacy footer link)

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint: all HTML/CSS/JS changes to the landing page stay in `index.html`; `privacy.html` is a new standalone page and is acceptable as a companion file
- `.planning/codebase/CONVENTIONS.md` — HTML comment style, BEM class names, CSS token usage, footer markup pattern
- `.planning/codebase/STRUCTURE.md` — `resources/` directory layout; repo root is the static site root (favicon files go in repo root)

### Existing Code Locations to Modify
- `index.html` `<head>` (~line 3–9) — Add OG meta tags, canonical link, and favicon `<link>` elements after existing `<meta>` tags
- `index.html` `<footer>` (~line 2041–2044) — Add Privacy Policy link `<p>` inside existing `<footer role="contentinfo">`

### Existing Assets Referenced
- `resources/og-image.png` — Confirmed OG social preview image (1.6 MB, already in repo)

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- Existing `<meta name="description">` (line 7): reused verbatim as `og:description`
- Existing `<title>` (line 6): reused verbatim as `og:title`
- Existing `<footer role="contentinfo">` (~line 2041): Privacy Policy link appended here as a new `<p>` — no structural change needed
- `--accent` CSS token (`#2dd4bf`): use for "WX" favicon text colour to match site palette
- `#0a0a0f` body background: use as favicon background colour

### Established Patterns
- `<head>` meta order: charset → viewport → title → description → (new: OG tags + canonical + favicon links) → Google Fonts
- Footer `<p>` style: `font-size` and `color` set via `footer p` CSS rule (~line 1027); new Privacy Policy `<p>` picks up same styling naturally
- HTML section comments use `<!-- ══ Section Name ══ -->` — use this for grouping OG tags in `<head>`

### Integration Points
1. **`<head>` block** — Insert OG tags + canonical + favicon links between `<meta name="description">` and `<link rel="preconnect" href="https://fonts.googleapis.com">`
2. **`<footer>`** — Append `<p><a href="privacy.html">Privacy Policy</a></p>` as last child of `<footer role="contentinfo">`
3. **Repo root** — New files: `favicon.svg`, `favicon.png`, `apple-touch-icon.png`, `privacy.html`

</code_context>

<specifics>
## Specific Ideas

- The OG image file is already present at `resources/og-image.png` — no image creation needed for DISC-01.
- The "WX" favicon monogram should feel like the site: dark, crisp, using `#2dd4bf` (the teal accent). Keep the SVG simple — just a rect + text, no gradients.
- `privacy.html` content basis: the 5 privacy cards from the `#privacy` section (no server communication, no analytics, no personal data storage, no internet requirement, camera is local-only). Expand these into prose paragraphs and add the required compliance fields (app name: "WLED to xLights Mapper", developer contact: wled.2.xlights@gmail.com, effective date).
- The privacy page nav should be minimal: app name on the left, "← Back to landing page" link on the right, matching the frosted-glass nav aesthetic of the main site.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 8-Discoverability Legal*
*Context gathered: 2026-05-18*
