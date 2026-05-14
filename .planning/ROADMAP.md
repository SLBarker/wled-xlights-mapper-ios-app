# Roadmap: WLED xLights Mapper — Landing Page

## Overview

Three focused phases take the landing page from its current desktop-only, bug-riddled state to a polished, mobile-first page with a proper carousel hero and a way for users to contact the developer. Each phase delivers a complete, independently verifiable improvement to the live page.

## Phases

- [x] **Phase 1: Mobile & Polish** - Make the page fully usable on mobile, fix all known bugs, and lazy-load videos *(completed 2026-05-13)*
- [ ] **Phase 2: Hero Carousel** - Replace the static phone mockup strip with a sliding carousel
- [ ] **Phase 3: Contact Section** - Add a styled Contact/Feedback section with a pre-filled mailto CTA

## Phase Details

### Phase 1: Mobile & Polish
**Goal**: The page is fully usable on mobile — nav is accessible, videos do not auto-download, and all known copy and markup bugs are eliminated
**Depends on**: Nothing (first phase)
**Requirements**: MOBL-01, MOBL-02, MOBL-03, MOBL-04, MOBL-05, PERF-01
**Success Criteria** (what must be TRUE):
  1. On a 375px viewport a visitor can tap a hamburger icon to open/close nav links and reach any page section
  2. All page sections are readable with no clipped or overflowing content at 375px–768px widths
  3. Both autoplay videos are not fetched on page load — they load only when scrolled near the viewport
  4. Copy reads correctly ("its" not "it's") and every image has accurate, unique alt text
**Plans**: 3 plans

**Wave 1**
- [x] 01-01-PLAN.md — Markup bug fixes (nested DOM, alt text, grammar)

**Wave 2**
- [x] 01-02-PLAN.md — Mobile responsive CSS + hamburger nav toggle

**Wave 3**
- [x] 01-03-PLAN.md — Lazy video loading via IntersectionObserver

**UI hint**: yes

### Phase 2: Hero Carousel
**Goal**: The hero section presents a conventional sliding carousel — visitors see one phone mockup at a time and can advance through them
**Depends on**: Phase 1
**Requirements**: HERO-01, HERO-02, HERO-03
**Success Criteria** (what must be TRUE):
  1. The hero no longer shows a horizontal fan/strip — a single phone mockup is displayed at a time
  2. Prev/next arrow buttons and dot indicators are visible and advance the carousel
  3. On mobile, swiping left or right advances the carousel
**Plans**: 2 plans

**Wave 1**
- [x] 02-01-PLAN.md — Remove old hero__phones CSS; add full hero-carousel CSS section

**Wave 2**
- [ ] 02-02-PLAN.md — Replace hero__phones HTML with 4-slide carousel markup + carousel JS IIFE

**UI hint**: yes

### Phase 3: Contact Section
**Goal**: Visitors can reach the developer directly from the page via a one-tap email CTA
**Depends on**: Phase 2
**Requirements**: CONT-01, CONT-02, CONT-03
**Success Criteria** (what must be TRUE):
  1. A Contact/Feedback section is visible on the page, styled consistently with existing sections
  2. Tapping "Send Feedback" opens the visitor's email client addressed and ready to send
  3. The pre-filled email subject reads exactly: "Feedback for WLED xLights Mapper"
**Plans**: TBD
**UI hint**: yes

## Progress

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Mobile & Polish | 3/3 | Complete | 2026-05-13 |
| 2. Hero Carousel | 0/2 | Not started | - |
| 3. Contact Section | 0/? | Not started | - |
