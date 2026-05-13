# Testing Patterns

**Analysis Date:** 2026-05-13

## Test Framework

**Runner:** None

No test framework is installed or configured. There is no `package.json`, `jest.config.*`, `vitest.config.*`, `playwright.config.*`, or any other test runner configuration present in this repository.

**Assertion Library:** None

**Run Commands:** Not applicable

## Test File Organization

No test files exist in this repository. There are no `*.test.*`, `*.spec.*`, or `__tests__/` directories.

## Test Coverage

**Requirements:** None enforced

No coverage tooling or thresholds are configured.

## Project Context

This is a single-file static HTML landing page (`index.html`) with:
- Embedded CSS (no build output to validate)
- ~20 lines of vanilla JavaScript (IntersectionObserver scroll-reveal only)
- No server-side logic
- No API endpoints
- No user input or form handling
- No state management
- No module boundaries

The JavaScript surface is extremely small: one `querySelectorAll` call and one `IntersectionObserver` callback that adds a CSS class. There are no functions, classes, data transformations, or business logic to unit-test.

## What Could Be Tested (If Needed)

If testing is introduced in future, the realistic scope is:

**Visual regression / screenshot testing:**
- Tool: Playwright or Percy
- Scope: Full-page screenshots across viewport sizes (mobile 375px, desktop 1280px)
- Value: Catching layout regressions in the CSS

**Accessibility auditing:**
- Tool: `axe-core` via Playwright or `pa11y`
- Scope: ARIA roles, heading hierarchy, skip link, alt text on all images and videos
- Value: Validating the accessibility patterns already present in the markup

**Link checking:**
- Tool: `lychee` or `htmltest`
- Scope: Internal anchor targets (`#hero`, `#features`, etc.), external links, `src` paths for images/videos
- Value: Catching broken resource paths after file moves

**HTML validation:**
- Tool: W3C Markup Validation Service or `vnu` (Nu Html Checker)
- Scope: `index.html`
- Value: Identifying structural issues (duplicate IDs, malformed nesting)

## Known Issues Relevant to Testing

**Duplicate notch divs:** Several `.phone-bezel__screen` / `.phone-bezel__notch` elements are duplicated in the hero and export sections of `index.html` (e.g., lines 843–848, 868, 992–993). An HTML validator would flag this structural redundancy.

**Duplicate `aria-label` on screenshots:** Multiple `<img>` elements share the identical `alt` text "App screenshot showing the controller connect screen with mDNS discovery list" regardless of what they actually depict. An accessibility audit would flag these.

## Recommended First Test (If Introduced)

```bash
# Install and run HTML/accessibility check
npx pa11y index.html

# Or validate HTML structure
npx vnu --format json index.html
```

No `package.json` exists yet — running either command would require initialising the project first:
```bash
npm init -y
npm install --save-dev pa11y
```

---

*Testing analysis: 2026-05-13*
