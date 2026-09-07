# Documentation & Technical Writing Standards & Practices

> **Purpose:** This document is my personal working standard for writing documentation — READMEs, code comments, internal wikis, and technical explanations. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before writing docs for a new project or updating stale ones.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my documentation baseline. When helping me write or review docs, hold the work to these standards. If a request would produce vague or padded documentation, flag it and offer options rather than silently going along with it.

**Context:** Sole developer writing docs for future-me, teammates I'm now bringing on, and occasionally non-technical stakeholders. No dedicated technical writer — this doc is a substitute for one.

---

## 1. README Essentials

Every project README should answer, in this order:

- [ ] **What is this?** — one or two sentences, plain language
- [ ] **How do I run it?** — exact setup steps, assume zero prior context
- [ ] **How do I deploy it?** — or a link to the deployment doc
- [ ] **What are the key pieces?** — brief map of the structure (not a full file listing)
- [ ] **Who do I ask / where do I look** if something's unclear or broken

**AI instruction:** When generating a README, follow this order. Don't pad with generic sections ("Contributing," "License," "Built With") unless they're actually relevant to this project.

---

## 2. Writing Style

- [ ] Plain language over jargon where jargon isn't necessary
- [ ] Active voice, short sentences — not padded corporate phrasing
- [ ] Specific and concrete over vague ("run `npm install` then `npm run dev`" not "set up the project")
- [ ] Written for the reader's actual context — future-me with no memory, or a new teammate with no history, not "someone who already knows everything"
- [ ] No filler sentences that exist just to sound thorough ("This project uses modern best practices to deliver a robust solution")

**AI instruction:** Avoid generic filler phrasing entirely — every sentence should convey a specific, true fact about this project. If you don't have a specific fact to state, don't write a placeholder sentence.

---

## 3. Code Comments

- [ ] Comments explain *why*, not *what* — the code already shows what it does
- [ ] Non-obvious business logic gets a comment (a client requirement, a workaround, an edge case) so it isn't "fixed" by someone later who doesn't know why it's there
- [ ] TODOs include enough context to act on later, not just `// TODO: fix this`
- [ ] Commented-out dead code is removed, not left "just in case" — git history is the "just in case"

**AI instruction:** When generating code, only add comments that explain non-obvious reasoning. Don't narrate obvious code line-by-line.

---

## 4. API/Function Documentation

- [ ] Every public function/endpoint states: what it does, what it takes in, what it returns, what can go wrong
- [ ] Examples included for anything with non-trivial usage, not just a type signature
- [ ] Edge cases and gotchas are documented where they exist (e.g., "returns null instead of throwing on X")

---

## 5. Keeping Docs Current

- [ ] Docs updated in the same change as the code, not as a separate "catch up later" task
- [ ] Outdated docs are corrected or deleted when noticed — a wrong doc is worse than no doc
- [ ] Setup instructions are periodically re-tested from scratch (do they actually still work on a clean machine?)

**AI instruction:** When I change code that a doc describes, flag that the doc likely needs updating too, rather than treating the two as unrelated.

---

## 6. Internal Knowledge / Runbooks

- [ ] Recurring manual processes (deploys, backups, common fixes) are written down as step-by-step runbooks
- [ ] Runbooks assume the reader is stressed/under pressure (e.g., site is down) — clear, numbered, no ambiguity
- [ ] "Tribal knowledge" — things only I know — gets written down as the team grows, not kept as a bottleneck

---

## 7. Documentation for a Growing Team

- [ ] New team members have something to read before asking me things I've already written down
- [ ] Standards docs (like this whole set) are kept in a place the team can actually find and reference
- [ ] Docs are structured for someone to self-serve an answer, not requiring me to explain in person every time

---

## 8. Using AI Tools for Documentation (Meta)

- [ ] AI-generated docs are checked for accuracy against the actual code/system — not accepted just because they sound polished
- [ ] I strip out generic AI filler phrasing before publishing anything AI helped draft
- [ ] AI is used to speed up structure/first drafts — the specific facts still come from me or the actual codebase

**AI instruction:** Never invent specifics (setup steps, config values, behavior) you're not certain of just to make documentation sound complete — state what you don't know or ask, rather than filling the gap with a plausible-sounding guess.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when someone (including future-me) gets confused or blocked by missing/wrong docs, that's a signal to reread this list and add a specific line, not to write a whole new document.
