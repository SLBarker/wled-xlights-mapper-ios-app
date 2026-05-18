---
status: complete
phase: 08-discoverability-legal
source: [08-01-SUMMARY.md, 08-02-SUMMARY.md]
started: 2026-05-18T00:00:00Z
updated: 2026-05-18T00:01:00Z
---

## Current Test

[testing complete]

## Tests

### 1. Favicon in browser tab
expected: Open index.html in a browser. The browser tab shows a "WX" monogram favicon with a dark rounded background and blue text. No generic browser globe or blank icon.
result: issue
reported: "favicon not visible at root URL (https://slbarker.github.io/wled-xlights-mapper-ios-app/) but visible at /index.html"
severity: minor

### 2. Social OG meta tags present
expected: View page source of index.html (Cmd+U in browser or open file in editor). The `<head>` block contains OG meta tags for og:type, og:url, og:title, og:description, og:image — and Twitter card tags for twitter:card, twitter:title, twitter:description, twitter:image.
result: pass

### 3. Canonical URL in head
expected: In the same page source, a `<link rel="canonical">` element appears pointing to the GitHub Pages root URL (https://slbarker.github.io/wled-xlights-mapper-ios-app/).
result: pass

### 4. Footer Privacy Policy link visible
expected: On index.html, scroll to the bottom footer. A "Privacy Policy" link appears as the last item in the footer — it is visible (slightly muted color inheriting from footer styling), and on hover its color changes/fades slightly.
result: pass

### 5. Privacy Policy page loads
expected: Click the "Privacy Policy" footer link (or navigate to privacy.html directly). The page loads without errors — no 404, no broken layout — and shows the title "Privacy Policy" and an effective date of "18 May 2026".
result: pass

### 6. Privacy Policy navigation
expected: On privacy.html, the top nav bar shows the app name on the left and a "← Back to landing page" link on the right. Clicking the back link returns you to index.html.
result: pass

### 7. Privacy Policy sections
expected: privacy.html displays 6 sections: "No Server Communication", "No Personal Data Storage", "No Analytics or Tracking", "No Internet Requirement", "Camera — Local Processing Only", and "Contact". Each has a heading and prose content.
result: pass

### 8. Contact email link
expected: In the Contact section of privacy.html, a mailto link to wled.2.xlights@gmail.com is visible and clickable. It opens the default mail client when clicked.
result: pass

## Summary

total: 8
passed: 7
issues: 1
pending: 0
skipped: 0
blocked: 0

## Gaps

- truth: "Favicon (WX monogram) appears in browser tab when visiting the root URL"
  status: failed
  reason: "User reported: favicon not visible at root URL (https://slbarker.github.io/wled-xlights-mapper-ios-app/) but visible at /index.html"
  severity: minor
  test: 1
  root_cause: ""
  artifacts: []
  missing: []
  debug_session: ""
