# Phase 9: Crawl Infrastructure - Context

**Gathered:** 2026-05-18
**Status:** Ready for planning

<domain>
## Phase Boundary

Update `robots.txt` (currently `Disallow: /`) to allow all crawlers, add a `Sitemap:` directive pointing to the sitemap, and create `sitemap.xml` listing the landing page's canonical URL. Also add a `<link rel="sitemap">` tag to `index.html` `<head>`. No other changes to `index.html` beyond that one `<head>` addition.

</domain>

<decisions>
## Implementation Decisions

### Sitemap Scope (DISC-03)
- **D-01:** `sitemap.xml` lists only the landing page — one `<loc>` entry: `https://slbarker.github.io/wled-xlights-mapper-ios-app/`. `privacy.html` is intentionally excluded (utility page, no SEO value).
- **D-02:** Minimal sitemap format — `<loc>` only. No `<lastmod>`, `<changefreq>`, or `<priority>` elements. Keeps the file honest and avoids stale metadata on a static file.

### robots.txt Format (DISC-03)
- **D-03:** Replace the existing `Disallow: /` with `Allow: /`. Add a `Sitemap:` directive: `Sitemap: https://slbarker.github.io/wled-xlights-mapper-ios-app/sitemap.xml`.
- **D-04:** Final `robots.txt` content:
  ```
  User-agent: *
  Allow: /

  Sitemap: https://slbarker.github.io/wled-xlights-mapper-ios-app/sitemap.xml
  ```

### index.html Head Tag (belt-and-suspenders)
- **D-05:** Add `<link rel="sitemap" type="application/xml" href="/sitemap.xml">` to `index.html` `<head>`, alongside the existing OG/canonical tags from Phase 8. This is the only `index.html` change in this phase.

### Claude's Discretion
- Exact XML declaration and namespace for `sitemap.xml` — use the standard `<?xml version="1.0" encoding="UTF-8"?>` declaration with `xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"`.
- Placement of the `<link rel="sitemap">` in `<head>` — insert after the Phase 8 canonical link tag.

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Requirements & Roadmap
- `.planning/REQUIREMENTS.md` — DISC-03 is the sole requirement this phase satisfies; acceptance criteria: robots.txt allows all crawlers, sitemap.xml lists canonical page URL, both files return valid content (no 404)
- `.planning/ROADMAP.md` — Phase 9 success criteria (3 checks)

### Architecture Constraints
- `CLAUDE.md` — Single-file constraint applies to landing page CSS/JS; `robots.txt` and `sitemap.xml` are new standalone files at the repo root (acceptable, same pattern as `privacy.html` from Phase 8)
- `.planning/codebase/STRUCTURE.md` — Repo root is the static site root; `robots.txt` and `sitemap.xml` must live at the repo root to be served at `/robots.txt` and `/sitemap.xml` by GitHub Pages

### Existing File to Modify
- `index.html` `<head>` — Add `<link rel="sitemap">` tag after the existing canonical link (Phase 8 added canonical at ~line 9–10)

### Existing File to Update
- `robots.txt` — File already exists with `Disallow: /`; must be replaced, not created fresh

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- Canonical URL `https://slbarker.github.io/wled-xlights-mapper-ios-app/` — already established in Phase 8 `<link rel="canonical">` and OG tags; reuse verbatim in `sitemap.xml` and `robots.txt` Sitemap directive

### Established Patterns
- Phase 8 `<head>` ordering: charset → viewport → title → description → OG tags → canonical → favicon links → Google Fonts. Insert `<link rel="sitemap">` after the canonical link.
- HTML comment grouping: `<!-- ══ Section Name ══ -->` style — use this if grouping the sitemap link with other discovery tags

### Integration Points
- `robots.txt` at repo root — already tracked in git; update in place
- `index.html` `<head>` — single additive line insertion after canonical tag

</code_context>

<specifics>
## Specific Ideas

- The `robots.txt` file already exists at repo root with `Disallow: /` — this phase flips it to open. Planner/executor must update the file, not create a new one.
- Sitemap XML structure: standard `<urlset>` with a single `<url>/<loc>` entry. No optional elements.

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 9-Crawl Infrastructure*
*Context gathered: 2026-05-18*
