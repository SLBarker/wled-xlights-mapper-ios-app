# Phase 5: Carousel Peek-View - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-14
**Phase:** 05-carousel-peek-view
**Areas discussed:** Peek width, Arrow placement, Edge slide behavior, Mobile behavior

---

## Peek Width

| Option | Description | Selected |
|--------|-------------|----------|
| Subtle ~50px each side | Total ~340px visible. Hints at more content. | |
| Balanced ~70px each side | Total ~380px visible. ~30% of adjacent bezel shows. | ✓ |
| Generous ~90px each side | Total ~420px visible. Strong three-slide effect. | |

**User's choice:** Balanced — ~70px each side

---

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — dim peeking slides (opacity ~0.5) | Active slide reads as clearly selected. | ✓ |
| No — same opacity | Position and dots alone communicate active slide. | |

**User's choice:** Yes — dim peeking slides to opacity ~0.5

---

## Arrow Placement

| Option | Description | Selected |
|--------|-------------|----------|
| Overlay on the peeking strips | Absolute-positioned inside 380px viewport, over dimmed peeks. | ✓ |
| Outside the peeked area | Arrows stay as flex siblings; total width grows to ~516px. | |
| Move below alongside dots | Removes arrows from viewport row; placed flanking dots. | |

**User's choice:** Overlay on the peeking strips

---

## Edge Slide Behavior

| Option | Description | Selected |
|--------|-------------|----------|
| Cyclic — slide 4 peeks from left of slide 1 | No visible start/end. Requires clone trick. | |
| Natural edge — no peek at boundaries | Left/right empty at slide 1/4. Simpler implementation. | ✓ |

**User's choice:** Natural edge — no peek at boundaries

---

| Option | Description | Selected |
|--------|-------------|----------|
| Hidden at boundary | Arrow disappears at boundary; cleaner visually. | ✓ |
| Disabled but visible | Arrow stays visible but greyed out; better keyboard UX. | |

**User's choice:** Hidden at boundary (prev hidden at slide 1, next hidden at slide 4)

---

## Mobile Behavior

| Option | Description | Selected |
|--------|-------------|----------|
| Reduced peek ~40px each side | Total 320px; fits iPhone SE. Override in 960px block. | ✓ |
| No peek — single slide view | Reverts to current 240px single-slide behaviour on mobile. | |

**User's choice:** Reduced peek ~40px each side

---

## Claude's Discretion

- Exact `left`/`right` pixel values for overlaid arrows (centre over 70px peek strip)
- `visibility: hidden` vs `display: none` for hidden boundary arrows (recommended: `visibility: hidden` to avoid layout shift)
- Whether `overflow: hidden` stays on `.hero-carousel__track-wrap` or moves to the viewport container

## Deferred Ideas

None — discussion stayed within phase scope.
