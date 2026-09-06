# QA Standards & Practices

> **Purpose:** This document is the working standard for how QA is done on this team. It's meant to be read two ways:
> 1. **By the team** — as a checklist/refresher before testing a feature, writing a bug report, or signing off on a release.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as the QA baseline for this team. When helping plan test cases, write bug reports, or review test coverage, hold the work (yours or the tester's) to these standards. If a request conflicts with a section below, flag it and offer options rather than silently going along with it.

**Context:** Internal tools + client-facing web systems. Small team, no dedicated QA-only tooling yet — testing is manual-first with automation added where it pays off.

---

## 1. Before Testing Starts

- [ ] Do I understand what the feature is *supposed* to do, not just what changed in the code?
- [ ] Do I have the requirements/spec, or am I guessing at intended behavior?
- [ ] Do I know who the user is (internal staff, client, admin) and what they're allowed to do?
- [ ] Is there a previous version/behavior to compare against, so I know what "regression" would look like?

**AI instruction:** If asked to generate test cases, ask for the spec or acceptance criteria first if not provided. Don't invent expected behavior from the feature name alone.

---

## 2. Test Case Design

- [ ] Happy path covered (the feature working as intended, normal input)
- [ ] Edge cases covered (empty fields, max length, special characters, zero/negative numbers)
- [ ] Negative cases covered (wrong input type, unauthorized access, expired session)
- [ ] Boundary values tested (off-by-one: first/last item, exact limit values)
- [ ] Cross-role testing — does this behave correctly for every user role that touches it (admin vs staff vs client)?
- [ ] Cross-browser / cross-device check for anything client-facing (at minimum Chrome + one mobile view)

**AI instruction:** When generating test cases, always include happy path + edge + negative + boundary categories explicitly labeled — don't just list random scenarios.

---

## 3. Bug Reporting

A good bug report answers all of these — a report missing any of them gets sent back before triage:

- [ ] **Title** — short, specific, searchable (not "form broken")
- [ ] **Steps to reproduce** — numbered, exact, no skipped steps
- [ ] **Expected result** — what should have happened
- [ ] **Actual result** — what actually happened
- [ ] **Environment** — browser/device/OS, account/role used, URL or module
- [ ] **Severity** — blocker / major / minor / cosmetic (defined below)
- [ ] **Evidence** — screenshot, screen recording, or console/log output where relevant

**Severity definitions:**
| Level | Meaning |
|---|---|
| Blocker | Feature or system unusable, no workaround, blocks release |
| Major | Feature broken but workaround exists, or affects many users |
| Minor | Feature partially broken, limited impact |
| Cosmetic | Visual/UI issue only, no functional impact |

**AI instruction:** When helping write or clean up a bug report, enforce this exact structure. If severity is missing or clearly mismatched (e.g., a typo marked "blocker"), point it out rather than accepting it as-is.

---

## 4. Regression Testing

- [ ] After any bug fix, retest the original bug *and* the surrounding feature (not just the one line changed)
- [ ] Before a release, re-run critical-path tests on core features even if untouched — dependencies break things silently
- [ ] Keep a running list of "known fragile areas" that get extra attention every release

**AI instruction:** When helping build a regression checklist, ask what changed in this release cycle and prioritize those areas plus their dependents — don't just reuse a generic template.

---

## 5. Sign-Off Criteria

A feature/release is NOT ready to ship if:

- [ ] Any blocker or major bug is still open
- [ ] Test cases haven't covered all user roles that touch the feature
- [ ] The fix hasn't been verified in the actual target environment (not just localhost/dev)
- [ ] No one has confirmed data integrity (nothing got corrupted/lost/duplicated in the process)

**AI instruction:** If asked to draft a sign-off/release note, check against this list first and flag any unmet criteria instead of assuming it's ready.

---

## 6. Communication & Escalation

- [ ] Blockers are flagged immediately, not batched into end-of-day report
- [ ] Bug reports go to the right owner — don't let ambiguous bugs sit unassigned
- [ ] If a bug can't be reproduced, say so explicitly and note what was tried — don't silently close it
- [ ] Disagreements about severity get resolved by discussion, not by whoever reports it deciding unilaterally

---

## 7. Using AI Tools in QA (Meta)

- [ ] AI can help generate test case lists and bug report drafts — but a human still executes and confirms the actual result
- [ ] Don't let AI mark something "tested" — AI can suggest what to test, not confirm it passed
- [ ] Real client/user data is never pasted into an AI tool without checking it's appropriate to share

**AI instruction:** Never state or imply that a test "passed" — you can suggest test steps and help interpret results a human gives you, but the pass/fail call is always the tester's.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a bug slips through that this checklist should have caught, that's a signal to reread this list and possibly add a line, not to add a whole new document.
