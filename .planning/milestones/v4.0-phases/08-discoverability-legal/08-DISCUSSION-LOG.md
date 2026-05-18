# Phase 8: Discoverability & Legal - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-18
**Phase:** 8-Discoverability & Legal
**Areas discussed:** OG image, Favicon & touch icon, Privacy policy link, Canonical URL

---

## OG image

| Option | Description | Selected |
|--------|-------------|----------|
| preview.jpeg (3D preview) | Shows the 3D model visualization — the most visually distinctive output | |
| ar.jpeg (AR scanning) | Shows the AR scanning step — the most unique feature | |
| results.jpeg (export results) | Shows the export/results screen — the end payoff | |
| A dedicated OG image | Create a separate 1200×630px social card image with app branding | ✓ |

**User's choice:** Dedicated OG image — specifically `resources/og-image.png` which already exists in the repo.

**Follow-up — og:title and og:description:**

| Option | Description | Selected |
|--------|-------------|----------|
| Reuse existing | og:title = current `<title>`; og:description = current `<meta name=description>` | ✓ |
| Customise both | Write tighter social-specific copy | |

**Notes:** `resources/og-image.png` confirmed present (1.6 MB). User directed Claude to use this file directly — no new image creation required.

---

## Favicon & touch icon

**Source:**

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — existing app icon | Use existing app icon asset files | |
| Use og-image.png as source | Derive favicon from og-image.png by cropping/resizing | |
| Create a simple text-based favicon | Generate a simple monogram or icon as SVG or PNG | ✓ |

**Design:**

| Option | Description | Selected |
|--------|-------------|----------|
| "WX" monogram | Two-letter monogram, dark bg with accent colour text | ✓ |
| LED dot grid | Small grid of coloured dots | |
| You decide | Claude picks the design | |

**Format:**

| Option | Description | Selected |
|--------|-------------|----------|
| SVG favicon | Modern SVG + PNG fallback | ✓ |
| PNG only | favicon.png at multiple sizes | |
| ICO + PNG | Legacy .ico + PNG | |

**Notes:** WX monogram on dark background (#0a0a0f) using the site's --accent colour (#2dd4bf). SVG primary, PNG fallback, apple-touch-icon.png at 180×180.

---

## Privacy policy link

**Destination:**

| Option | Description | Selected |
|--------|-------------|----------|
| A new privacy.html page | Standalone page in repo root, Claude writes content | ✓ |
| An external URL | Pre-existing hosted privacy policy URL | |
| The existing #privacy section | Link to anchor on same page | |

**Content:**

| Option | Description | Selected |
|--------|-------------|----------|
| Minimal App Store compliance | States no data collection, no tracking, contact email, effective date | |
| Full legal privacy policy | Comprehensive formal policy | |
| Reuse existing privacy card content | Expand the 5 existing privacy bullet points | |

**User's choice (free text):** "please use the existing privacy content, but ensure all App Store compliance requirements are also covered."

**Styling:**

| Option | Description | Selected |
|--------|-------------|----------|
| Styled to match the site | Dark bg, Inter font, nav header with back link | ✓ |
| Minimal / plain | Simple white-background legal document | |

**Notes:** privacy.html should feel cohesive with the main landing page. Content expands the 5 existing privacy card points into prose + adds compliance fields (app name, developer email, effective date).

---

## Canonical URL

| Option | Description | Selected |
|--------|-------------|----------|
| GitHub Pages default URL | https://slbarker.github.io/wled-xlights-mapper-ios-app/ | ✓ |
| Custom domain | Custom domain configured for GitHub Pages | |
| Not sure yet | Use placeholder, finalise before planning | |

**User's choice (free text):** "the url is https://slbarker.github.io/wled-xlights-mapper-ios-app/"

**Notes:** Repo confirmed as `SLBarker/wled-xlights-mapper-ios-app`. Canonical URL locked. Used for og:url, og:image absolute path, and will feed into Phase 9 sitemap.xml.

---

## Claude's Discretion

- Exact SVG markup for the "WX" favicon (viewBox dimensions, font-size, text centering)
- Whether to include Twitter/X card meta tags (`twitter:card`, `twitter:image`, etc.) alongside OG tags
- Exact footer link markup and placement within `<footer>`
- Whether `privacy.html` includes its own `<link rel="canonical">` tag

## Deferred Ideas

None — discussion stayed within phase scope.
