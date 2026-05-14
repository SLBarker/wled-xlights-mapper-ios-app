# Phase 3: Contact Section - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-14
**Phase:** 3-contact-section
**Areas discussed:** Email address, Section content & copy, Section background, Nav link

---

## Email Address

| Option | Description | Selected |
|--------|-------------|----------|
| barker.simon.l@gmail.com | Personal Gmail — simple, works immediately | |
| A dedicated support address | Keeps app feedback separate from personal email | ✓ |
| Something else | Specific address in mind | |

**User's choice:** `wled.2.xlights@gmail.com` (dedicated support address, provided as free text)

**Follow-up — mailto format:**

| Option | Description | Selected |
|--------|-------------|----------|
| Subject only | Clean, minimal. User writes whatever they want. Matches CONT-03 exactly. | ✓ |
| Subject + short body prompt | e.g. "App version: " or "Describe your feedback:" | |

**Notes:** mailto format locked as `mailto:wled.2.xlights@gmail.com?subject=Feedback%20for%20WLED%20xLights%20Mapper`.

---

## Section Content & Copy

| Option | Description | Selected |
|--------|-------------|----------|
| Minimal — headline + button | Centered heading, one button. Clean and focused. | |
| Headline + short paragraph + button | Adds 1-2 sentences inviting feedback before the button | ✓ |
| You decide | Claude picks based on page fit | |

**User's choice:** Headline + short paragraph + button

**Follow-up — section label:**

| Option | Description | Selected |
|--------|-------------|----------|
| Contact | Matches the nav item label — clear and consistent | ✓ |
| Feedback | More inviting, softer tone | |
| Get in Touch | Warmer, conversational | |

**Follow-up — section heading:**

| Option | Description | Selected |
|--------|-------------|----------|
| Share your feedback | Warm, action-oriented | ✓ |
| Get in touch | Generic but familiar | |
| You decide | Claude writes something fitting | |

**Notes:** Paragraph copy left to Claude's discretion — warm, brief, 1-2 sentences (e.g. "Found a bug or have a feature request? I'd love to hear from you."). Button label "Send Feedback" is locked by CONT-02.

---

## Section Background

| Option | Description | Selected |
|--------|-------------|----------|
| Dark — same as Privacy | Continues dark rhythm at bottom; blends into footer | |
| Light — same as Features/Tips | Creates visual break between dark Privacy and dark footer | ✓ |
| Accent/gradient | Hero-style blue gradient — signals CTA moment | |

**User's choice:** Light — same as Features/Tips sections

**Notes:** Creates deliberate visual break between `#privacy` (dark) and `<footer>` (very dark).

---

## Nav Link

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — add a Contact nav link | Consistent with all major sections; both desktop and hamburger | ✓ |
| No — skip the nav link | Keep nav focused on product content | |

**User's choice:** Yes — add "Contact" to both desktop nav and hamburger drawer

---

## Claude's Discretion

- Paragraph copy (1–2 sentences, warm and inviting — brief encouragement to share bugs or feature requests)
- Scroll-reveal animation on the CTA or section body (optional, following existing IntersectionObserver pattern)

## Deferred Ideas

None — discussion stayed within phase scope.
