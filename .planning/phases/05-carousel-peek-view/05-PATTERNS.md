# Phase 5: Carousel Peek-View - Pattern Map

**Mapped:** 2026-05-14
**Files analyzed:** 1 (index.html — four distinct sections modified)
**Analogs found:** 4 / 4 (all within index.html itself; no external analogs needed)

---

## File Classification

| Section to Modify | Role | Data Flow | Closest Analog | Match Quality |
|-------------------|------|-----------|----------------|---------------|
| `index.html` lines 330–418 — carousel CSS | component styles | event-driven (CSS transitions) | Lines 394–417 dot indicator CSS — same BEM modifier pattern | exact |
| `index.html` lines 970–978 — `@media (max-width: 960px)` carousel overrides | responsive override | n/a | Lines 971–978 existing overrides for `.hero-carousel` and `.hero-carousel__arrow` | exact |
| `index.html` lines 1069–1153 — carousel HTML | component markup | n/a | Lines 1085–1131 existing slide markup — same BEM structure | exact |
| `index.html` lines 1692–1757 — carousel JS | component controller | event-driven | Full IIFE at lines 1693–1757 — `goTo()` is the single state function | exact |

---

## Pattern Assignments

### CSS: `.hero-carousel__viewport` (lines 340–345) — make `position: relative`, remove `gap`

**Current code** (lines 340–345):
```css
.hero-carousel__viewport {
  position: relative;
  display: flex;
  align-items: center;
  gap: 1.5rem;
}
```

**Change:** Remove `gap: 1.5rem`. The `position: relative` already exists and is the containing block for absolutely-positioned arrows. No other changes to this rule.

---

### CSS: `.hero-carousel__track-wrap` (lines 347–350) — widen to 380px, keep `overflow: hidden`

**Current code** (lines 347–350):
```css
.hero-carousel__track-wrap {
  overflow: hidden;
  width: 240px;
}
```

**Change:** `width: 240px` → `width: 380px`. `overflow: hidden` stays here (CONTEXT.md D-03 note: overflow can stay on track-wrap since arrows are positioned inside the viewport, above the track-wrap stacking context).

---

### CSS: `.hero-carousel__slide` (lines 358–364) — add dimmed opacity and transition

**Current code** (lines 358–364):
```css
.hero-carousel__slide {
  flex: 0 0 240px;
  width: 240px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
```

**Change:** Add `opacity: 0.5;` and `transition: opacity var(--transition);` to the base rule. Then add the new modifier rule immediately after:
```css
.hero-carousel__slide--active {
  opacity: 1;
}
```

**BEM modifier pattern to copy from** (existing dot modifier, lines 414–417):
```css
.hero-carousel__dot--active {
  background: var(--accent);
  transform: scale(1.25);
}
```
Same `--active` suffix pattern, same placement (modifier rule directly after base rule).

---

### CSS: `.hero-carousel__arrow` (lines 367–392) — switch from flex-child to absolute positioned

**Current code** (lines 367–392):
```css
.hero-carousel__arrow {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: none;
  cursor: pointer;
  background: var(--surface);
  box-shadow: var(--shadow);
  color: var(--accent);
  transition: var(--transition);
  flex-shrink: 0;
}

.hero-carousel__arrow:hover {
  background: var(--accent);
  color: var(--surface);
  transform: scale(1.08);
}

.hero-carousel__arrow:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

**Changes to base rule:** Remove `flex-shrink: 0`. Add:
```css
position: absolute;
top: 50%;
transform: translateY(-50%);
z-index: 2;
visibility: visible;
```

Add selector rules for left/right placement (new rules, after the base rule):
```css
#carousel-prev {
  left: 13px;  /* centres 44px button over 70px peek strip: (70 - 44) / 2 = 13 */
}
#carousel-next {
  right: 13px;
}
```

**Visibility hidden pattern** — use `visibility: hidden` (not `display: none`) per CONTEXT.md D-05 to avoid layout shift. Add new rule:
```css
.hero-carousel__arrow--hidden {
  visibility: hidden;
}
```

**Note on hover transform:** The existing `:hover` rule uses `transform: scale(1.08)`. With `position: absolute` the base rule will use `transform: translateY(-50%)`. Override on hover to combine both:
```css
.hero-carousel__arrow:hover {
  background: var(--accent);
  color: var(--surface);
  transform: translateY(-50%) scale(1.08);
}
```

---

### CSS: `@media (max-width: 960px)` carousel additions (lines 970–978)

**Current mobile carousel overrides** (lines 977–978):
```css
.hero-carousel { margin-top: 3rem; }
.hero-carousel__arrow { width: 36px; height: 36px; }
```

**Add** (same block, after existing two rules):
```css
.hero-carousel__track-wrap { width: 320px; }
#carousel-prev { left: 9px; }  /* (40 - 36) / 2 + 1 ≈ 3px visual buffer */
#carousel-next { right: 9px; }
```

**Pattern for override placement:** All carousel mobile overrides live in the single `@media (max-width: 960px)` block at line 970. No second media block is permitted (ARCHITECTURE.md constraint).

---

### HTML: carousel viewport and arrow placement (lines 1069–1153)

**Current structure** (key nodes only):
```html
<div class="hero-carousel__viewport">
  <button class="hero-carousel__arrow" id="carousel-prev" ...>
  <div class="hero-carousel__track-wrap">
    <div class="hero-carousel__track" id="carousel-track">
      <div class="hero-carousel__slide" ...>  <!-- × 4 -->
    </div>
  </div>
  <button class="hero-carousel__arrow" id="carousel-next" ...>
</div>
```

**Changes required:**
1. The prev/next `<button>` elements do not move in the DOM — they remain inside `.hero-carousel__viewport`, which is `position: relative`. CSS absolute positioning lifts them visually over the peek strips.
2. Each `<div class="hero-carousel__slide">` gains the `--active` class on slide 1 at page load (JS sets it via `goTo(0)`). No `--active` class is needed in static HTML.

**No markup restructuring needed** — the existing DOM order is compatible with CSS absolute positioning.

---

### JS: `goTo()` function (lines 1707–1718) — pixel offset + active class + arrow visibility

**Current `goTo()` code** (lines 1707–1718):
```javascript
function goTo(n) {
  current = ((n % total) + total) % total;
  track.style.transform = `translateX(-${current * 100}%)`;
  dots.forEach((dot, i) => {
    const active = i === current;
    dot.classList.toggle('hero-carousel__dot--active', active);
    dot.setAttribute('aria-current', active ? 'true' : 'false');
  });
  slides.forEach((slide, i) => {
    slide.setAttribute('aria-hidden', i === current ? 'false' : 'true');
  });
}
```

**Changes required — replacement `goTo()`:**

1. Line 1709: `translateX(-${current * 100}%)` → `translateX(-${current * 240}px)`
   - Slide width is fixed at 240px; percentage no longer equals one-slide-width once track-wrap is 380px.

2. After the `slides.forEach` aria-hidden block, add active class toggle:
```javascript
slides.forEach((slide, i) => {
  slide.classList.toggle('hero-carousel__slide--active', i === current);
});
```

3. After the active class block, add arrow visibility update:
```javascript
prevBtn.style.visibility = current === 0 ? 'hidden' : 'visible';
nextBtn.style.visibility = current === total - 1 ? 'hidden' : 'visible';
```

**Pattern precedent:** The existing `dots.forEach` toggle at lines 1710–1714 uses the same `classList.toggle(class, boolean)` pattern — copy exactly for the `--active` slide toggle.

**Initial call `goTo(0)` at line 1720** already runs on page load — prev arrow will be hidden on load because `current === 0`. No extra init code needed.

---

## Shared Patterns

### BEM modifier toggle (JS)
**Source:** `goTo()` lines 1710–1714 (dot active class)
**Apply to:** Slide active class toggle (new code in `goTo()`)
```javascript
dot.classList.toggle('hero-carousel__dot--active', active);
```
Copy verbatim; substitute class name and element reference.

### CSS token usage
**Source:** `.hero-carousel__arrow` lines 376–380
**Apply to:** All new CSS rules in Phase 5
```css
background: var(--surface);
box-shadow: var(--shadow);
color: var(--accent);
transition: var(--transition);
```
Never hardcode colour or shadow values. Use `var(--transition)` for all new opacity and transform transitions.

### CSS section banner comment
**Source:** Line 330
**Apply to:** No new sections are added in Phase 5 — all edits are within the existing `/* ── Hero carousel ── */` block. Keep the existing banner; do not add a second banner.
```css
/* ── Hero carousel ── */
```

### Single media query block
**Source:** Line 970
**Apply to:** All mobile overrides for Phase 5 go into the single `@media (max-width: 960px)` block. No second or new media block.

---

## No Analog Found

None — all patterns for Phase 5 exist within the carousel code already present in `index.html`.

---

## Summary of All Line Ranges to Touch

| index.html range | What changes |
|------------------|--------------|
| Lines 340–345 | Remove `gap: 1.5rem` from `.hero-carousel__viewport` |
| Lines 347–350 | `width: 240px` → `width: 380px` on `.hero-carousel__track-wrap` |
| Lines 358–364 | Add `opacity: 0.5; transition: opacity var(--transition);` to `.hero-carousel__slide` |
| After line 364 | Insert `.hero-carousel__slide--active { opacity: 1; }` |
| Lines 367–381 | Add `position: absolute; top: 50%; transform: translateY(-50%); z-index: 2;` to `.hero-carousel__arrow`; remove `flex-shrink: 0` |
| After line 392 | Insert `#carousel-prev { left: 13px; }` and `#carousel-next { right: 13px; }` |
| After line 392 | Insert `.hero-carousel__arrow--hidden { visibility: hidden; }` |
| Lines 383–387 | Update `:hover` transform to `translateY(-50%) scale(1.08)` |
| Lines 977–978 | After existing mobile rules, add `width: 320px` override and arrow `left`/`right` overrides |
| Lines 1707–1718 | Replace `goTo()` body: pixel translateX, slide `--active` toggle, arrow visibility |

---

## Metadata

**Analog search scope:** `index.html` (single-file project; no other source files)
**Files scanned:** 1 (`index.html`)
**Supporting docs read:** `.planning/codebase/CONVENTIONS.md`, `.planning/codebase/ARCHITECTURE.md`
**Pattern extraction date:** 2026-05-14
