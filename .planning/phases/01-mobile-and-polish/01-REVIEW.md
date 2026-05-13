---
phase: 01-mobile-and-polish
reviewed: 2026-05-13T00:00:00Z
depth: standard
files_reviewed: 1
files_reviewed_list:
  - index.html
findings:
  critical: 0
  warning: 6
  info: 7
  total: 13
status: issues_found
---

# Phase 1: Code Review Report

**Reviewed:** 2026-05-13
**Depth:** standard
**Files Reviewed:** 1
**Status:** issues_found

## Summary

`index.html` is a well-structured single-file landing page. Accessibility scaffolding is solid (skip link, ARIA labels, landmark roles) and the new hamburger-nav and lazy-video implementations are functionally correct. No security vulnerabilities were found — the page has no server interaction, no user input, and no dynamic content injection.

Six warnings were found — mostly correctness and robustness issues that would produce broken behaviour in specific browsers or use-cases. Seven info items cover quality, consistency, and minor omissions.

---

## Warnings

### WR-01: Lazy-loaded videos have `autoplay` attribute but never call `.play()` after `src` is set

**File:** `index.html:1044-1052`, `index.html:1115-1123`, `index.html:1517-1518`

**Issue:** Both `<video>` elements carry the `autoplay` attribute, but `src` is intentionally withheld at parse time (only `data-src` is set). The browser honours `autoplay` only at parse time or when the element is inserted into the DOM with `src` already present. Once the IntersectionObserver fires and sets `video.src` + calls `video.load()`, the browser does **not** automatically begin playback — the autoplay policy has already been evaluated and the video remains paused. On Chrome/Safari the video will sit as a black frame after it loads.

**Fix:** After `video.load()`, call `video.play().catch(function(){})` to trigger playback. The `.catch` silences the `NotAllowedError` that some browsers throw when autoplay is blocked for non-muted video (these videos are muted, so it should succeed, but the catch guard is defensive best practice).

```javascript
video.src = video.dataset.src;
video.load();
video.play().catch(function () {});
lazyVideoObserver.unobserve(video);
```

---

### WR-02: `aria-controls` points to the wrong element ID

**File:** `index.html:926`

**Issue:** The hamburger `<button>` declares `aria-controls="site-nav-menu"` (line 926), which resolves to the desktop `<ul id="site-nav-menu">` (line 913). The button actually controls the mobile drawer `<div id="site-nav-drawer">` (line 940). Screen readers following `aria-controls` will announce the wrong element — or announce a visually-hidden list — rather than the drawer that the button opens and closes.

**Fix:** Change the button's `aria-controls` to match the drawer ID:

```html
aria-controls="site-nav-drawer"
```

---

### WR-03: `preconnect` for Google Fonts is missing the `crossorigin` attribute and the `fonts.gstatic.com` origin

**File:** `index.html:8-9`

**Issue:** Google Fonts uses two origins: `fonts.googleapis.com` for the CSS and `fonts.gstatic.com` for the actual font files. Only the first origin is preconnected, and it is missing `crossorigin`. The browser therefore opens an anonymous connection for the stylesheet and a separate credentialed connection for each font file. The `fonts.gstatic.com` origin — where the font binary is fetched — is never preconnected, negating most of the preconnect benefit and delaying the first contentful paint with the custom typeface.

**Fix:**

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

---

### WR-04: `closeMenu(false)` on drawer link click does not return focus — keyboard users become stranded

**File:** `index.html:1491-1494`

**Issue:** When a keyboard user opens the drawer, tabs to a link, and activates it, `closeMenu(false)` is called. Focus is not returned to the hamburger button. The drawer collapses (visually and via `aria-hidden="true"`) while focus remains on the now-hidden link, which means the next Tab keypress may land on an invisible or unreachable element. This is a keyboard-navigation correctness issue, not merely a style preference.

**Fix:** Pass `true` when closing via a link click so focus returns to the toggle:

```javascript
link.addEventListener('click', function () {
  closeMenu(true);
});
```

---

### WR-05: `.feature-card:hover` transform conflicts with the pre-visible initial state

**File:** `index.html:420-438`

**Issue:** `.feature-card` starts with `opacity: 0; transform: translateY(24px)`. `.feature-card.visible` sets `transform: translateY(0)`. `.feature-card:hover` sets `transform: translateY(-4px)`. Because specificity of `.feature-card:hover` and `.feature-card.visible` is equal, the hover rule wins only when hovered — but if a user hovers over a card before it is marked `.visible` (e.g., the card is in the viewport but IO callback has not yet fired), the card will snap to `translateY(-4px)` rather than staying at `translateY(24px)` and then animating in. More importantly, once `.visible` is added and the card is at rest, hovering correctly applies `translateY(-4px)`, but if the IO `transitionDelay` is still active, lifting the mouse mid-transition can cause a visual jump because both `opacity` and `transform` are listed in the `transition` property but the hover state overrides `transform` without resetting `opacity`. A secondary concern: the commented-out `feature-card` article at line 1192-1198 is still observed by the IO (the selector `.feature-card` matches it), which means a hidden card silently consumes a slot in the siblings index used for stagger timing — the stagger delay of the card following it will be one step higher than intended.

**Fix:** Scope the hover rule to visible cards only:

```css
.feature-card.visible:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}
```

And remove the commented-out article element from the DOM rather than leaving it as an HTML comment inside a `role="list"` region (see also IN-01).

---

### WR-06: `<section id="pitch">` has `aria-labelledby="pitch-title"` but only the first `<h2>` carries that ID

**File:** `index.html:1008-1027`

**Issue:** The pitch section contains two side-by-side `<h2>` elements: "Manual LED mapping is painful" (id="pitch-title") and "Walk. Scan. Import." (no id). The section's accessible name therefore only exposes the first heading to assistive technology and document outline tools. The second heading — which introduces the solution — appears as an unlabelled heading in the section tree, breaking the implied document outline structure. This is a correctness issue for users relying on heading navigation.

**Fix:** Either wrap each side in its own `<section>` or `<div>` with appropriate landmarks, or assign the second heading an id and include it in the `aria-labelledby` list:

```html
<section id="pitch" class="section" aria-labelledby="pitch-title pitch-title-2">
  ...
  <h2 class="section-title" id="pitch-title">Manual LED mapping is painful</h2>
  ...
  <h2 class="section-title" id="pitch-title-2">Walk. Scan. Import.</h2>
```

---

## Info

### IN-01: Commented-out `<article>` block sits inside a `role="list"` landmark

**File:** `index.html:1192-1198`

**Issue:** A full `<article class="feature-card" role="listitem">` element is commented out inside `<div class="feature-cards" role="list">`. While HTML comments don't affect the rendered DOM, they inflate file size and — more importantly — the IntersectionObserver selector `.feature-card` will not observe it (it's a comment). However, the intent is clearly to hold dead code in place. Leaving dead markup inside an accessible list container is a code-quality smell.

**Fix:** Delete the comment block entirely. If the UDP DNRGB feature is planned for reinstatement, track it in a planning artifact rather than in-file dead code.

---

### IN-02: Missing `prefers-reduced-motion` media query

**File:** `index.html:368-374`, `index.html:778-781`

**Issue:** The page uses CSS entry animations (`fadeUp`, `translateY` transitions on `.feature-row`, `.feature-card`, etc.) and JavaScript-driven IntersectionObserver scroll animations. None of these are gated on `prefers-reduced-motion: reduce`. Users who have requested reduced motion in their OS accessibility settings will receive full animations. While not a functional bug, it is a common accessibility quality bar and is relevant to the Phase 1 polish goals.

**Fix:** Add a media query that disables transitions and keyframe animations for users who prefer reduced motion:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
  .feature-row, .feature-card, .step-card,
  .req-card, .tip-item, .privacy-card {
    opacity: 1;
    transform: none;
  }
}
```

---

### IN-03: Inline `style` attributes used in the export section instead of CSS classes

**File:** `index.html:1292`, `1295`, `1299-1300`, `1428`

**Issue:** Several elements use inline styles (`style="margin-top:1rem;"`, `style="display:flex; justify-content:center;"`, `style="width:240px;"`) contrary to the project's architecture of keeping all styles in the `<style>` block. While not a functional bug, it creates an inconsistency that will become harder to maintain and override responsively. The `.feature-row__phone` div at line 1299 also carries a `style` override that fights with `.pitch__inner`'s responsive collapse.

**Fix:** Extract the repeated inline styles to named CSS classes:

```css
.section-body + .section-body { margin-top: 1rem; }
.export-phone { display: flex; justify-content: center; }
.export-phone .phone-bezel { width: 240px; }
```

---

### IN-04: Google Fonts is an external dependency that breaks on network failure / CSP

**File:** `index.html:8-9`

**Issue:** The page loads Inter from `fonts.googleapis.com` at runtime. On a strict CSP, in an offline environment, or if the CDN is unavailable, the font request fails and the page falls back to system fonts. The fallback stack (`-apple-system, BlinkMacSystemFont, sans-serif`) is reasonable, but there is no declared `font-display` swap behaviour in the local CSS to prevent layout shift. This is a robustness observation for a static page on GitHub Pages.

**Fix:** Add `&display=swap` to the Google Fonts URL (already present, so swap is already on). The remaining exposure is the hard external dependency itself — for a production landing page, self-hosting Inter via `@font-face` would eliminate it entirely.

---

### IN-05: `<video>` elements lack `poster` attributes

**File:** `index.html:1044`, `index.html:1115`

**Issue:** Both lazy-loaded videos have no `poster` image. Before the IntersectionObserver fires (and even briefly after `video.load()` begins), the video element displays as a black rectangle. On slow connections, the black frame persists for a noticeable duration. A poster frame matching the first frame of each video would improve perceived quality.

**Fix:** Add a `poster` attribute referencing a representative JPEG frame:

```html
<video data-src="resources/video/discovery.mp4"
       poster="resources/screenshot/connect.jpeg"
       autoplay loop muted playsinline ...>
```

---

### IN-06: `<a href="#download">` is also the `id="download"` anchor — the CTA links to itself

**File:** `index.html:965`

**Issue:** The primary call-to-action button has `id="download"` and `href="#download"`. The nav "Download" links (`href="#download"`) resolve to this same button, so activating a nav CTA link just scrolls to and re-focuses the hero CTA button. This is intentional as a temporary placeholder (no App Store URL exists yet), but it means the nav CTA and the hero CTA are functionally identical in a non-obvious way. If the App Store link is ever added, only the hero anchor's `href` will be updated; the nav links still point to `#download` (the element ID) rather than to an external URL, which could silently break.

**Fix:** If the App Store URL is not yet available, use `href="#"` or `href="#hero"` as explicit stubs. When the URL is known, replace all three `href="#download"` occurrences with the real URL and remove `id="download"` from the anchor element.

---

### IN-07: Step cards in the "How It Works" section jump from `<h4>` headings without an intermediate `<h3>` in the workflow phase block

**File:** `index.html:1213-1265`

**Issue:** The heading hierarchy inside `<section id="how-it-works">` goes: `<h2>` (section title) → `<p class="workflow__phase-title">` (phase label, styled to look like a heading but semantically a paragraph) → `<h4>` (step card headings). The `<h3>` level is skipped entirely. A screen reader navigating by headings will see `h2` then jump to `h4` with no intermediate hierarchy, which is an HTML5 outline correctness issue.

**Fix:** Change `.workflow__phase-title` elements from `<p>` to `<h3>`, or change the step card headings from `<h4>` to `<h3>`:

```html
<!-- Option A: promote phase titles -->
<h3 class="workflow__phase-title">In the App</h3>

<!-- Option B: if keeping p for phase title, lower step cards to h3 -->
<h3>Connect</h3>
```

---

_Reviewed: 2026-05-13_
_Reviewer: Claude (gsd-code-reviewer)_
_Depth: standard_
