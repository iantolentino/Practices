# UI/UX Standards & Practices

> **Purpose:** This document is my personal working standard for UI/UX decisions. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before designing a screen, reviewing a mockup, or approving something before it ships.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my UI/UX baseline. When helping me design or build interfaces, hold the work (mine or yours) to these standards — especially the anti-generic-AI-design section. If a request would produce something generic or unclear, flag it and offer options rather than silently defaulting to it.

**Context:** Internal tools + client-facing web systems (PHP/JS stacks, sometimes React). No dedicated designer — I make these calls myself, so this doc is a substitute for a design review.

---

## 1. Before Designing Anything

- [ ] Who is the actual user, and what's their context (rushed employee, careful client, admin doing bulk work)?
- [ ] What's the ONE primary action on this screen? Everything else is secondary.
- [ ] Is there an existing pattern in this project I should reuse, or is this genuinely a new pattern?
- [ ] Am I designing for the realistic data (long names, empty states, 200 rows) — not just the happy-path demo data?

**AI instruction:** Before generating UI, ask what the primary action/goal of the screen is if I haven't said. Don't default to a generic dashboard/form layout without knowing the actual use case.

---

## 2. Preventing "AI Slop" Design

This section exists specifically to catch generic, templated-feeling output — the stuff that looks like every other AI-generated app.

- [ ] No default purple/indigo gradient hero sections unless I specifically asked for that aesthetic
- [ ] No unnecessary decorative icons next to every label "for visual interest" — icons need to carry meaning, not decoration
- [ ] Typography has actual hierarchy — not everything is the same size with just bold/color changes
- [ ] Spacing is intentional (a real scale — 4/8/16/24/32px), not everything crammed to a default padding
- [ ] The layout reflects THIS project's content and priorities — not a reusable "centered card with shadow" template applied to everything
- [ ] Color choices have a reason (brand, semantic meaning, contrast need) — not "AI's default blue/teal palette"
- [ ] I can tell what this screen does at a glance, without reading every label

**AI instruction:** Actively avoid: centered cards with heavy box-shadow as a default container; purple-to-blue gradients as a default accent; emoji used as functional icons; generic rounded-corner-everything with no other distinguishing style; Inter/system-font-with-no-personality unless that's a deliberate choice for this project. If you're not sure the design choice is intentional vs. a "safe AI default," ask me instead of shipping it.

---

## 3. Usability Basics

- [ ] Forms tell the user what went wrong, specifically — not just "error occurred"
- [ ] Every destructive action (delete, remove, cancel) has a confirmation step or an undo
- [ ] Loading states exist for anything that takes >300ms — no frozen-looking screens
- [ ] Empty states are designed, not just a blank white area ("No results yet — here's what to do")
- [ ] Buttons/links describe the action ("Save changes" not just "Submit" / "OK")
- [ ] Tab order and keyboard navigation work on forms, not just mouse-only
- [ ] Focus states are visible (don't strip default outlines without replacing them)

**AI instruction:** When building a form or interactive component, include error states, loading states, and empty states by default — don't wait for me to ask for the "unhappy path."

---

## 4. Visual Consistency

- [ ] One color palette used project-wide, defined once (CSS variables/theme file), not re-picked per screen
- [ ] One spacing scale used consistently, not arbitrary pixel values scattered through the code
- [ ] Consistent button styles for the same action-type across the app (primary/secondary/destructive always look the same)
- [ ] Consistent iconography — one icon set, not mixed styles (outline vs filled, different libraries)

**AI instruction:** When adding a new component, check it against the existing color/spacing/button patterns in the project rather than introducing a new one-off style.

---

## 5. Responsiveness

- [ ] Tested at actual mobile width (375px), not just a shrunk desktop browser
- [ ] Touch targets are large enough (~44px) on mobile — not tiny desktop-sized click targets
- [ ] Tables/data-heavy views have a mobile strategy (stack, scroll, or card view) — not just squeezed columns
- [ ] Text doesn't require horizontal scrolling on mobile

---

## 6. Accessibility Baseline

- [ ] Color contrast meets at least WCAG AA for text (4.5:1 normal text, 3:1 large text)
- [ ] Color is never the only signal (error states also use icon/text, not just red)
- [ ] Images have alt text; icon-only buttons have accessible labels
- [ ] Interactive elements are actually focusable/keyboard-operable, not divs pretending to be buttons

**AI instruction:** Don't rely on color alone to communicate state (success/error/warning). Always pair it with an icon or text label.

---

## 7. Content & Copy

- [ ] Labels are specific to the context ("Client name" not just "Name" if there are multiple name fields on screen)
- [ ] Error/help text sounds like a helpful person, not a stack trace ("Email already in use" not "ERR_DUPLICATE_ENTRY_23505")
- [ ] Confirmation messages are specific ("Ticket #482 closed" not just "Success")

---

## 8. Review Before Shipping

- [ ] Viewed at actual target screen sizes (not just my dev monitor)
- [ ] Clicked through the unhappy paths (wrong input, no data, slow network) not just the demo flow
- [ ] Asked: if I saw this cold, with no context, would I understand what to do?
- [ ] Compared against the rest of the app — does this look like it belongs, or like a different app got pasted in?

---

## 9. Using AI Tools for UI/UX (Meta)

- [ ] AI-generated UI gets reviewed against Section 2 (anti-slop) before I accept it, not just skimmed for "does it look nice"
- [ ] I ask AI to explain a design choice if it looks like a generic default rather than a deliberate one
- [ ] Real client branding/screens aren't pasted into AI tools without checking it's appropriate to share

**AI instruction:** If I accept a generated UI without comment, don't assume that means every default choice in it was deliberate — if you notice a generic pattern from Section 2 slipped through, it's fine to mention it even after the fact.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a shipped screen ends up looking generic or someone struggles to use it, that's a signal to reread this list and possibly add a line, not to add a whole new document.
