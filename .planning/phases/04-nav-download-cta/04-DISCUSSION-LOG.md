# Phase 4: Nav & Download CTA - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-14
**Phase:** 04-nav-download-cta
**Areas discussed:** Hamburger breakpoint, Mobile CTA appearance, Drawer cleanup

---

## Hamburger Breakpoint

### Q1: New breakpoint threshold

| Option | Description | Selected |
|--------|-------------|----------|
| 960px | Covers most tablets; hamburger shows at iPad-portrait widths and below | ✓ |
| 1024px | More aggressive — hamburger shows at small laptops too | |
| 900px | More conservative — more desktop-like widths keep full nav | |

**User's choice:** 960px
**Notes:** Chosen as the recommended option.

### Q2: Icon at mobile widths

| Option | Description | Selected |
|--------|-------------|----------|
| Icon + text at all widths | Same icon+label pill whether hamburger is showing or not | ✓ |
| Icon-only at mobile, icon+text at desktop | Saves horizontal space at mobile — requires two CSS states | |

**User's choice:** Icon + text at all widths
**Notes:** Consistent appearance across all widths.

---

## Mobile CTA Appearance

### Q1: CTA position in .site-nav__inner

| Option | Description | Selected |
|--------|-------------|----------|
| Between links and hamburger | [logo] [links→hidden] [Download CTA] [hamburger] | ✓ |
| Right of logo | [logo] [Download CTA] [links] [hamburger] | |

**User's choice:** Between links and hamburger
**Notes:** Natural reading order; follows the convention of the most important action being near the toggle.

### Q2: CTA class approach

| Option | Description | Selected |
|--------|-------------|----------|
| Keep .site-nav__cta, update hover | Less disruptive; compact pill size correct for 56px nav bar | ✓ |
| Switch to .btn .btn-primary | Reuses hero classes but oversized for nav bar | |

**User's choice:** Keep .site-nav__cta, update its hover
**Notes:** The hero button padding (0.875rem 1.75rem) is too large for the nav bar; .site-nav__cta's compact padding (0.4rem 1rem) is the right fit.

---

## Drawer Cleanup

### Q1: Download CTA in drawer

| Option | Description | Selected |
|--------|-------------|----------|
| Remove from drawer entirely | CTA always visible in nav bar; no reason to duplicate | ✓ |
| Keep in drawer as well | Adds discoverability but contradicts NAV-02 | |

**User's choice:** Remove it from the drawer entirely
**Notes:** Follows NAV-02 explicitly; the permanently-visible nav bar CTA removes any need for a drawer duplicate.

---

## Claude's Discretion

- Minor gap/padding tuning on `.site-nav__cta` if icon+text pill looks cramped.
- Whether to extend `transition` on `.site-nav__cta` to include `box-shadow` (recommended yes).
- Removal of orphaned `.site-nav__drawer .site-nav__cta` CSS block after drawer entry is removed.

## Deferred Ideas

None — discussion stayed within phase scope.
