# Phase 9: Crawl Infrastructure - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-18
**Phase:** 9-Crawl Infrastructure
**Areas discussed:** Sitemap scope, robots.txt format

---

## Sitemap Scope

| Option | Description | Selected |
|--------|-------------|----------|
| Landing page only (index.html) | Sitemap lists one URL. Privacy pages are utility pages — no SEO value in indexing them. Matches DISC-03's wording. | ✓ |
| Both pages (index.html + privacy.html) | Sitemap lists two URLs. Privacy page becomes discoverable via search, which can aid App Store compliance reviewers. | |

**User's choice:** Landing page only (index.html)
**Notes:** Keeps sitemap minimal and focused; privacy.html is a utility page with no search value.

---

| Option | Description | Selected |
|--------|-------------|----------|
| Just `<loc>` (minimal) | Simpler. Google largely ignores `<lastmod>` unless dynamic. | ✓ |
| Include `<lastmod>` with today's date | More complete. Signals when page was last updated. Needs manual update on each change. | |

**User's choice:** Just `<loc>` (minimal)
**Notes:** Static file — no dynamic date updating, so omitting `<lastmod>` keeps it honest.

---

## robots.txt Format

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — include Sitemap: directive | Standard practice — gives crawlers the sitemap URL directly. | ✓ |
| No — minimal robots.txt only | Just User-agent: * and Allow: /. Sitemap still discoverable via head tag. | |

**User's choice:** Yes — include Sitemap: directive
**Notes:** Belt-and-suspenders approach for crawler discovery.

---

| Option | Description | Selected |
|--------|-------------|----------|
| robots.txt only — no `<head>` change | Sitemap: directive in robots.txt is sufficient. Avoids touching index.html. | |
| Add `<link rel="sitemap">` to index.html too | Belt-and-suspenders for crawlers that check both places. | ✓ |

**User's choice:** Add `<link rel="sitemap">` to index.html too
**Notes:** Minimal head change (one line) — worth adding for completeness.

---

## Claude's Discretion

- Exact XML declaration and namespace for sitemap.xml
- Placement of `<link rel="sitemap">` within `<head>` (after canonical link)

## Deferred Ideas

None — discussion stayed within phase scope.
