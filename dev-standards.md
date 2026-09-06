# Developer Standards & Practices

> **Purpose:** This document is a personal reference and working standard for how I build software. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before starting work, during code review, or when I feel like I'm cutting corners.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my baseline engineering standard. When helping me plan, write, or review code, hold my work (and yours) to these standards. If something I'm doing or asking for conflicts with a section below, flag it and give me options rather than silently going along with it.

**Stack context:** PHP, Python, JavaScript/Node. Windows 11 + Git Bash. Sole developer role — no built-in code review safety net, so these standards substitute for that.

---

## 1. Before Writing Code

- [ ] Do I understand the actual problem, not just the requested feature?
- [ ] Have I checked for an existing pattern/util/component in this codebase before writing a new one?
- [ ] Is this the smallest reasonable version of the change, or am I scope-creeping?
- [ ] If ambiguous, have I written down my assumption instead of guessing silently?

**AI instruction:** Before generating code, ask me to confirm scope if my request is ambiguous. Don't assume the biggest possible interpretation.

---

## 2. Writing Code

- [ ] Input validation on anything from a user, form, or external API
- [ ] No hardcoded secrets, API keys, or credentials — use `.env` / config, never committed
- [ ] SQL via parameterized queries / prepared statements — never string-concatenated
- [ ] Escaping output (XSS prevention) on anything rendered from user data
- [ ] Meaningful variable/function names — no `$temp2`, `data1`
- [ ] Functions do one thing; if I can't name it cleanly, it's doing too much
- [ ] Errors are handled, not swallowed silently (no empty `catch {}`)
- [ ] Comments explain *why*, not *what* (the code already shows what)

**AI instruction:** When generating code for me, apply this list by default without me having to ask. Flag if a request would require violating one of these (e.g. "you asked for raw SQL interpolation — here's the parameterized version instead").

---

## 3. Version Control

- [ ] Commit messages describe *why*, not just *what* ("fix null check on client email" not "fix bug")
- [ ] Small, focused commits — not "end of day dump"
- [ ] No direct commits to `main`/`production` branch for anything non-trivial
- [ ] `.gitignore` covers env files, node_modules, vendor, build artifacts before first commit

**AI instruction:** When drafting commit messages or PR descriptions for me, follow this style.

---

## 4. Testing & Verification

- [ ] Manually tested the happy path
- [ ] Manually tested at least one failure/edge case (empty input, wrong type, no auth)
- [ ] If a bug was fixed, I understand *why* it happened, not just that the symptom went away
- [ ] For anything client-facing or production-bound: tested on the actual target environment, not just localhost

**AI instruction:** When I say something "works," ask me what I tested it against if I haven't said. Don't assume "works" means fully tested.

---

## 5. Documentation

- [ ] README explains: what this is, how to run it, how to deploy it — assume the reader is future-me in 6 months with no memory of this
- [ ] Any non-obvious business logic has a comment explaining the "why" (client requirement, edge case, workaround)
- [ ] Environment setup steps are written down somewhere, not just in my head

**AI instruction:** When asked to document a project, default to this structure. Don't pad with generic boilerplate ("This project uses modern technologies") — write what's actually true and specific.

---

## 6. Security Baseline

- [ ] Auth checks on every protected route/endpoint, not just the UI hiding the button
- [ ] Passwords hashed (never plaintext, never reversible encryption)
- [ ] File uploads validated (type, size) and stored outside web root or with execution disabled
- [ ] Rate limiting / basic abuse protection on public-facing forms
- [ ] Dependencies checked for known vulnerabilities periodically (`npm audit`, `composer audit`)

**AI instruction:** Treat this section as non-negotiable. If I ask for something that skips a security basic (e.g., "just store the password as plaintext for now"), push back once, explain the risk briefly, then follow my explicit call if I still confirm — but don't do it silently by default.

---

## 7. Using AI Tools (Meta)

- [ ] I understand what AI-generated code does before committing it — no blind paste
- [ ] AI is used for scaffolding/boilerplate/tests/docs — not for architecture or security decisions without my own review
- [ ] I've asked "why" when AI suggests a pattern I don't recognize, instead of assuming it's correct because it's confident
- [ ] Sensitive data (real client data, credentials) is never pasted into an AI tool without checking it's appropriate to share

**AI instruction:** If I paste code and ask "what does this do," answer plainly — don't assume I already understand it just because I wrote/pasted it.

---

## 8. Senior-Level Habits (Aspirational — Not Yet Automatic)

- [ ] Before building, ask if this feature *should* exist, not just how to build it
- [ ] Write code assuming someone else (or future me) will maintain it with zero context
- [ ] Default to the boring, well-understood solution over the clever one
- [ ] When stuck for >30 min, write down what I've tried before asking for help — clarifies the actual question
- [ ] Periodically review old projects for tech debt, not just new ones for features

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when I catch myself skipping a step above and it costs me, that's a signal to reread this list, not to add a new one.
