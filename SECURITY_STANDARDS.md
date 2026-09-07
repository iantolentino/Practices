# Security Standards & Practices

> **Purpose:** This document is my personal working standard for security across every layer of what I build. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before shipping anything that touches user data, auth, or a public endpoint.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my security baseline. When helping me write code, review architecture, or handle data, hold the work (mine or yours) to these standards. This section overrides convenience — if a request would weaken security, flag it and offer a safer option rather than silently complying.

**Context:** Internal tools + client-facing web systems for a BPO company, meaning real client and employee data passes through my systems. Sole developer — no dedicated security team, so this doc is a substitute for one.

---

## 1. Authentication & Sessions

- [ ] Passwords hashed with a strong algorithm (bcrypt/argon2) — never plaintext, never reversible encryption
- [ ] Session tokens are random and long enough to resist guessing; regenerated on login (prevent session fixation)
- [ ] Sessions expire after reasonable inactivity, especially for admin/privileged accounts
- [ ] Failed login attempts are rate-limited/locked out — no unlimited brute-force attempts
- [ ] Password reset flows use expiring, single-use tokens — never "email the password back"
- [ ] MFA available/enabled for admin and privileged accounts where feasible

**AI instruction:** Never suggest storing passwords in plaintext or reversible encryption, even "temporarily" or "just for testing." Always use hashing.

---

## 2. Authorization

- [ ] Every protected route/endpoint checks permissions server-side — never rely on the UI just hiding a button
- [ ] Role checks happen on every request, not cached from login and assumed still valid
- [ ] Object-level checks exist ("can THIS user access THIS record") — not just "is logged in"
- [ ] Admin/privileged actions are logged (who did what, when)

**AI instruction:** When building an endpoint, always ask/assume server-side authorization is required unless I explicitly say the route is public.

---

## 3. Input Handling

- [ ] All user input validated server-side (client-side validation is UX only, never the security boundary)
- [ ] SQL via parameterized queries — never string concatenation
- [ ] Output escaped/encoded before rendering (XSS prevention) — especially anything from user-submitted content
- [ ] File uploads validated (type, size, extension) and stored outside web-executable paths
- [ ] No `eval()` or dynamic code execution on anything derived from user input

**AI instruction:** Any code you write that touches user input must validate/sanitize server-side by default. If asked to skip this "for speed," flag it and confirm before proceeding.

---

## 4. Secrets & Configuration

- [ ] No API keys, passwords, or tokens hardcoded in source — always env vars or a secrets manager
- [ ] `.env` and credential files are gitignored before first commit, not after a leak
- [ ] Different secrets for dev/staging/production — production secrets never reused locally
- [ ] Secrets rotated if ever exposed (accidental commit, shared screen, etc.) — not just "hope no one noticed"

**AI instruction:** Never write example code with real-looking API keys or credentials, even as a placeholder pattern that could be mistaken for real. Use obvious placeholders like `YOUR_API_KEY_HERE`.

---

## 5. Data Protection

- [ ] Sensitive data (PII, client data) identified and known — I can name what's sensitive in each system
- [ ] Data encrypted in transit (HTTPS everywhere, no plain HTTP for anything with login or data entry)
- [ ] Sensitive fields encrypted at rest where appropriate, not just relying on DB access control alone
- [ ] Data retention has a limit — old/unneeded sensitive data isn't kept indefinitely "just in case"
- [ ] Client/production data never copied to a local/dev machine without stripping or masking sensitive fields first

**AI instruction:** If a design stores sensitive data (SSNs, financial info, health data, credentials) flag encryption/retention as an open question rather than assuming plain storage is fine.

---

## 6. Dependencies & Supply Chain

- [ ] Dependencies checked for known vulnerabilities periodically (`npm audit`, `composer audit`, Dependabot alerts)
- [ ] Unused dependencies removed, not left as dead weight and dead attack surface
- [ ] Dependency updates aren't blindly auto-merged for major versions without a changelog check
- [ ] Only well-maintained, reasonably popular packages used for anything security-sensitive (auth, crypto, payments)

---

## 7. Common Vulnerabilities Checklist

Quick-reference — check these explicitly before shipping anything public-facing:

- [ ] **SQL Injection** — parameterized queries everywhere
- [ ] **XSS** — output escaped, Content-Security-Policy considered
- [ ] **CSRF** — state-changing requests protected (CSRF tokens or SameSite cookies)
- [ ] **IDOR** (Insecure Direct Object Reference) — object-level authorization checked, not just "logged in"
- [ ] **Broken auth** — session handling reviewed per Section 1
- [ ] **Security misconfiguration** — default credentials changed, debug mode off in production, directory listing disabled
- [ ] **Sensitive data exposure** — error messages don't leak stack traces/internal paths to end users

**AI instruction:** When reviewing code for security, explicitly check it against this list rather than giving a general "looks fine" pass.

---

## 8. Incident Response (Even as a Solo Dev)

- [ ] I know where logs live and how to check them quickly if something looks wrong
- [ ] If a breach/leak is suspected: rotate affected secrets first, then investigate scope, then notify affected parties/stakeholders
- [ ] Backups exist and are restorable, so a compromised system can be rolled back
- [ ] Incidents get written down after the fact (what happened, why, what changed) — not just fixed and forgotten

---

## 9. Using AI Tools for Security (Meta)

- [ ] AI-generated code involving auth, crypto, or sensitive data handling gets manually reviewed line-by-line — never trusted blindly
- [ ] I don't paste real credentials, tokens, or client data into an AI tool to "debug faster"
- [ ] I ask AI to explain *why* a security pattern is recommended, not just accept it because it sounds authoritative

**AI instruction:** Treat this entire document as non-negotiable by default. If I ask for something that conflicts with it, push back once with a brief reason, then follow my explicit override if I confirm — never comply silently without flagging it first.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a vulnerability or close call happens, that's a signal to reread this list and add a specific line, not to write a whole new document.
