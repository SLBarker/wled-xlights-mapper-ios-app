# Codebase Concerns

**Analysis Date:** 2026-05-13

## Tech Debt

**No App Store link wired up:**
- Issue: The primary CTA "Download on the App Store" (`<a href="#download" class="btn btn-primary" id="download">`) links to itself (`#download`) — the `id="download"` is on the `<a>` tag, not a separate download section. There is no `apps.apple.com` URL anywhere in the file. The nav "Download" item also links to `#download`, which scrolls nowhere meaningful.
- Files: `index.html` lines 814, 832
- Impact: Every visitor who clicks the primary CTA goes nowhere. This is the most critical missing piece before the page can serve its purpose.
- Fix approach: Replace `href="#download"` with the actual App Store URL (e.g. `https://apps.apple.com/app/id...`). Remove `id="download"` from the anchor or move it to a dedicated section if a download landing block is planned.

**Commented-out features left in source:**
- Issue: Two blocks of HTML are commented out — a UDP DNRGB feature card (lines 1072–1078) and quality-tier pills inside the Scan step card (lines 1112–1119). These are dead weight: they cannot be rendered, tested, or reviewed without uncommenting.
- Files: `index.html` lines 1072–1078, 1112–1119
- Impact: Source noise that can confuse future editors about intended content. If the feature is planned, it should be tracked elsewhere (e.g. a planning doc); if it is dropped, the markup should be deleted.
- Fix approach: Delete both commented blocks or extract the content intent to a planning note.

**Redundant video asset:**
- Issue: Both `resources/video/discovery.mov` (4.1 MB) and `resources/video/discovery.mp4` (1.9 MB) exist, but only the `.mp4` is referenced in `index.html`. The `.mov` is an unused duplicate.
- Files: `resources/video/discovery.mov`
- Impact: 4.1 MB of dead weight committed to the repository and served by the hosting provider unnecessarily.
- Fix approach: Delete `discovery.mov` from the repository.

**`.DS_Store` files committed:**
- Issue: `.DS_Store` files are present at the repo root, `resources/`, `resources/screenshot/`, and `resources/video/`. These macOS metadata files are committed to git.
- Files: `.DS_Store`, `resources/.DS_Store`, `resources/screenshot/.DS_Store`, `resources/video/.DS_Store`
- Impact: Repository pollution; can leak filesystem path metadata.
- Fix approach: Delete all `.DS_Store` files, add `**/.DS_Store` to `.gitignore`, and re-commit.

**No `.gitignore` file:**
- Issue: No `.gitignore` is present in the repository, which allowed `.DS_Store` files to be committed and leaves the repo open to future accidental commits of editor files, OS artefacts, or build output.
- Files: (none — file is absent)
- Impact: Ongoing risk of committing unwanted files.
- Fix approach: Add a `.gitignore` containing at minimum `**/.DS_Store`, `.planning/`, and any editor-specific patterns.

## Known Bugs

**Nested `phone-bezel__screen` / duplicate notch divs:**
- Symptoms: Several phone bezel mockups contain a double-nested structure — an outer `phone-bezel__screen` wrapping an inner `phone-bezel__screen`, each with its own `phone-bezel__notch`. This produces two notch bars rendered on top of each other.
- Files: `index.html` lines 843–849 (hero phone 1), 866–871 (hero phone 3), 915–920 (discovery feature), 990–994 (3D preview feature), 1182–1187 (export section)
- Trigger: Always visible — these are static markup errors.
- Workaround: None; the double notch is hidden only because the outer screen clips it, but the extra DOM nodes are always present.

**"Download" CTA is a self-referencing anchor:**
- Symptoms: Clicking "Download on the App Store" scrolls the page minimally (the button is in the hero) and does not open the App Store.
- Files: `index.html` lines 832–836
- Trigger: Any click on the primary CTA or nav "Download" link.
- Workaround: None for visitors.

**Typos in copy — possessive "its" written as "it's":**
- Symptoms: Two sentences in Step 04 use the contraction "it's" (it is) where the possessive "its" is grammatically correct.
- Files: `index.html` lines 1133–1134 ("adjust it's position", "based on it's neighbours")
- Trigger: Always visible.
- Workaround: None — visible copy error.

**Incorrect and copy-pasted alt text:**
- Symptoms: Four `<img>` elements (three hero phone screenshots and the export section screenshot) all share the identical alt text: `"App screenshot showing the controller connect screen with mDNS discovery list"`. The `ar.jpeg`, `preview.jpeg`, and `results.jpeg` images show different screens and are described inaccurately.
- Files: `index.html` lines 847, 858, 870, 1187
- Trigger: Always present; impacts screen reader users and SEO.
- Workaround: None.

## Security Considerations

**Google Fonts loaded over external CDN:**
- Risk: The Inter font is fetched from `fonts.googleapis.com` and `fonts.gstatic.com` at page load. This creates a third-party network dependency and a minor privacy consideration (Google logs the request IP).
- Files: `index.html` lines 8–9
- Current mitigation: `rel="preconnect"` reduces latency; connection is HTTPS.
- Recommendations: Self-host the Inter font subset to remove the external dependency and eliminate the CDN privacy exposure. Use `font-display: swap` (already the default with `&display=swap` in the Google URL).

**No Content-Security-Policy header:**
- Risk: No CSP is declared in a `<meta http-equiv="Content-Security-Policy">` tag. For a GitHub Pages static site this is low severity, but any future inline script additions or third-party embeds would not be governed by a policy.
- Files: `index.html` (header section, absent)
- Current mitigation: There is only one small inline `<script>` block currently; no external scripts beyond Google Fonts CSS.
- Recommendations: Add a `<meta>` CSP at minimum, or configure CSP headers at the hosting layer when a custom domain is set up.

## Performance Bottlenecks

**Large unoptimised video assets:**
- Problem: `resources/video/3dpreview.mp4` is 9.2 MB and `resources/video/discovery.mp4` is 1.9 MB. Both autoplay inline in phone mockups and are not lazy-loaded.
- Files: `index.html` lines 920–929, 995–1003; `resources/video/3dpreview.mp4`, `resources/video/discovery.mp4`
- Cause: Videos are embedded with `autoplay loop muted playsinline` with no `preload` attribute set to `none` or `metadata`, no poster image, and no responsive source sets.
- Improvement path: Add `preload="none"` and a `poster` image to defer video load until visible. Use an `IntersectionObserver` to set `src` lazily, or use `<source>` elements with a WebM encode alongside MP4 for smaller file sizes. Consider compressing `3dpreview.mp4` further (current 9.2 MB is large for a landing page embedded asset).

**Screenshot images not using modern formats:**
- Problem: All screenshots are `.jpeg` with sizes up to 880 KB (`ar.jpeg`). No `<picture>` element or WebP/AVIF alternatives are provided.
- Files: `resources/screenshot/ar.jpeg` (880 KB), `resources/screenshot/connect.jpeg` (252 KB), `resources/screenshot/preview.jpeg` (212 KB), `resources/screenshot/results.jpeg` (264 KB)
- Cause: Images are served as-is with no responsive sizing, no `srcset`, and no modern format negotiation.
- Improvement path: Convert to WebP (or AVIF) and wrap in `<picture>` with JPEG fallback. Add `loading="lazy"` to all below-the-fold images.

**No `preload` resource hint for hero images:**
- Problem: The hero phone mockup images (`connect.jpeg`, `ar.jpeg`, `preview.jpeg`) are not preloaded, so they are discovered late in the render pipeline causing layout shifts or delayed LCP.
- Files: `index.html` head section (absent)
- Cause: No `<link rel="preload" as="image">` tags.
- Improvement path: Add `<link rel="preload" as="image" href="resources/screenshot/connect.jpeg">` (and the other two visible-on-load images) to the `<head>`.

**Google Fonts render-blocking:**
- Problem: The Google Fonts `<link>` is a standard stylesheet load (not using `font-display` swap at the link level), which can block rendering on slow connections.
- Files: `index.html` line 9
- Cause: `<link href="https://fonts.googleapis.com/css2?..." rel="stylesheet">` is a synchronous stylesheet.
- Improvement path: Use the `font-display=swap` parameter (already present in the URL) or self-host to eliminate the round-trip entirely.

## Fragile Areas

**All CSS and HTML in a single 1,340-line file:**
- Files: `index.html`
- Why fragile: The entire landing page — reset, design tokens, layout, section styles, animations, responsive breakpoints, and JavaScript — lives in one file. Any edit to CSS risks unintended side-effects on other sections. There is no build step, linting, or component boundary.
- Safe modification: Make changes to one CSS rule at a time and verify visually across breakpoints. Use browser devtools to confirm specificity before editing.
- Test coverage: None — no automated tests exist for the page.

**Phone bezel component is copy-pasted HTML, not a reusable pattern:**
- Files: `index.html` lines 841–876 (hero), 911–932, 940–958, 960–984, 986–1029 (features), 1179–1193 (export)
- Why fragile: The phone bezel markup is duplicated verbatim (with variations) six times. Several instances have the structural bug of nested `phone-bezel__screen` divs (see Known Bugs). Fixing the bezel design or bug requires updating every instance individually with no guarantee of consistency.
- Safe modification: Locate all instances via `grep -n "phone-bezel__frame"` before editing; update each occurrence and verify the notch nesting is correct.
- Test coverage: None.

**Scroll animations depend on `IntersectionObserver` with no fallback:**
- Files: `index.html` lines 1317–1336
- Why fragile: Animated elements start with `opacity: 0` and only become visible when the `IntersectionObserver` fires `classList.add('visible')`. If JavaScript is disabled or fails, all animated sections (`.feature-row`, `.feature-card`, `.step-card`, `.req-card`, `.tip-item`, `.privacy-card`) remain invisible.
- Safe modification: Add a CSS `@supports` or `<noscript>` rule setting these elements to `opacity: 1` as a fallback.
- Test coverage: None.

**Mobile navigation has no fallback:**
- Files: `index.html` lines 784–786 (CSS), 807–816 (HTML)
- Why fragile: On viewports ≤768 px the entire `site-nav__links` is hidden with `display: none` and no hamburger menu or alternative navigation is provided. Mobile visitors have no way to jump to page sections via the nav.
- Safe modification: Add a mobile hamburger toggle or replace the hidden nav with a persistent Download button visible at all breakpoints.
- Test coverage: None.

## Missing Critical Features

**No favicon:**
- Problem: No `favicon.ico`, `favicon.png`, or `apple-touch-icon` is present. The `<head>` contains no `<link rel="icon">` tag.
- Blocks: Browser tab shows a blank/default icon; iOS "Add to Home Screen" shows a generic icon; poor first impression.

**No Open Graph or Twitter Card meta tags:**
- Problem: No `og:title`, `og:description`, `og:image`, `twitter:card`, or similar social sharing meta tags are present in the `<head>`.
- Blocks: Sharing the URL on social media, messaging apps, or Slack produces an unstyled link with no preview image or description.

**No sitemap or robots.txt:**
- Problem: Neither `sitemap.xml` nor `robots.txt` exists at the repo root.
- Blocks: Search engine crawlers lack guidance; the page may be indexed sub-optimally. A sitemap would also surface the canonical URL.

**No structured data (Schema.org):**
- Problem: No `application/ld+json` block exists. For an app landing page, `SoftwareApplication` structured data would improve App Store search integration and rich snippets.
- Blocks: Rich result eligibility in Google Search.

**No copyright year or contact/support link in footer:**
- Problem: The footer contains only a tagline and trademark disclaimer. There is no copyright year, no contact email, no support link, and no privacy policy page link.
- Blocks: Legal completeness for an App Store-distributed product (Apple requires a privacy policy URL on App Store listing pages).

**No `<link rel="canonical">` tag:**
- Problem: No canonical URL is declared. If the page is served under multiple URLs (e.g. with and without `www`, or via GitHub Pages default domain alongside a custom domain), search engines may treat them as duplicates.
- Files: `index.html` head section (absent)

## Test Coverage Gaps

**No tests of any kind:**
- What's not tested: HTML validity, link integrity (especially the broken `#download` CTA), accessibility, visual regression, and performance budgets.
- Files: `index.html` (entire page)
- Risk: Regressions in layout, broken links, and accessibility issues can be introduced without detection.
- Priority: Medium — recommended additions: an HTML validator CI step, a link-checker action (to catch the dead `#download` href), and a Lighthouse CI check for performance and accessibility scores.

---

*Concerns audit: 2026-05-13*
