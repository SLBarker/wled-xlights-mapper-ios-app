# External Integrations

**Analysis Date:** 2026-05-13

## APIs & External Services

**Font Delivery:**
- Google Fonts — serves the `Inter` typeface at page load
  - URL: `https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap`
  - Also uses `<link rel="preconnect" href="https://fonts.googleapis.com">` for performance
  - Auth: None (public CDN)
  - Impact: Page renders with system font fallback if this CDN is unavailable

**No other external APIs or services are integrated.** The page makes no fetch/XHR calls, no analytics beacon requests, no tag manager loads, and no third-party script tags.

## Data Storage

**Databases:**
- None — this is a static landing page with no server-side persistence

**File Storage:**
- Local filesystem only — all media assets are committed to the repository under `resources/`

**Caching:**
- None configured explicitly — relies on browser defaults and GitHub Pages CDN headers

## Authentication & Identity

**Auth Provider:**
- None — the page has no login, account, or gated content

## Monitoring & Observability

**Error Tracking:**
- None — no Sentry, Bugsnag, or equivalent

**Analytics:**
- None — the page explicitly contains no analytics or tracking (confirmed by page content in the Privacy section and absence of any analytics script tags)

**Logs:**
- None — static page; no server-side logging

## CI/CD & Deployment

**Hosting:**
- GitHub Pages — confirmed by git commit message "initial pages deploy" and branch `initial`

**CI Pipeline:**
- None detected — no `.github/workflows/` directory or other CI config files present
- Deployment is manual push to the GitHub Pages branch

## Webhooks & Callbacks

**Incoming:**
- None

**Outgoing:**
- None

## Environment Configuration

**Required env vars:**
- None — fully static; no runtime environment variables

**Secrets location:**
- Not applicable — no secrets, API keys, or credentials of any kind

## Third-Party Scripts

No third-party JavaScript is loaded. The only external resource is the Google Fonts stylesheet (CSS only, no JS).

## App Store Link

The page contains a "Download on the App Store" call-to-action button (`#download` anchor in `index.html`). At the time of analysis the `href` points to `#download` (self-referential placeholder), indicating the App Store URL has not yet been wired up. When the app is published, this will need to be updated to the Apple App Store product URL.

---

*Integration audit: 2026-05-13*
