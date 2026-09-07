# Code Review Standards & Practices

> **Purpose:** This document is my working standard for reviewing code — my own before merging, and my team's as it grows. It's meant to be read two ways:
> 1. **By me (and my team)** — as a checklist/refresher before approving a PR or reviewing a change.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my code review baseline. When helping me review code or draft review feedback, hold the work to these standards. If a request would mean rubber-stamping something risky, flag it and offer options rather than silently going along with it.

**Context:** Small/growing dev team, PHP/Python/JS stacks. I'm currently the primary reviewer — this doc exists so review quality doesn't depend on what I happen to remember to check that day, and so it scales once others review too.

---

## 1. Before Reviewing

- [ ] I understand what the change is *supposed* to do (read the PR description/ticket, not just the diff)
- [ ] I know what's in scope for this review — don't demand unrelated fixes in the same PR
- [ ] I've pulled/run the change locally if the risk level justifies it, not just read it on screen
- [ ] I'm reviewing at a time I can actually focus — not skimming a security-sensitive change in 30 seconds between other tasks

**AI instruction:** If asked to review a diff with no context on what it's supposed to do, ask for the ticket/description first rather than guessing intent from the code alone.

---

## 2. What to Check — Correctness

- [ ] Code actually does what the PR claims it does
- [ ] Edge cases are handled (empty input, null, zero, max values) — not just the happy path
- [ ] Error handling exists and fails safely (no silent swallowing, no leaking internals to users)
- [ ] No obvious logic errors (off-by-one, wrong comparison operator, inverted condition)
- [ ] Existing tests still pass; new logic has test coverage where it matters

---

## 3. What to Check — Standards Alignment

Cross-reference against the other standards docs, not just personal taste:

- [ ] **Security** — no hardcoded secrets, parameterized queries, input validated, auth checked server-side (see SECURITY_STANDARDS.md)
- [ ] **Database** — migrations reversible, no N+1 patterns, constraints used appropriately (see DATABASE_STANDARDS.md)
- [ ] **API** — consistent response/error shape, correct status codes if applicable (see API_STANDARDS.md)
- [ ] **UI/UX** — no generic/inconsistent patterns introduced, matches existing design system (see UIUX_STANDARDS.md)

**AI instruction:** When reviewing code, check it against the relevant specialty standards doc for that type of change, not just general code quality.

---

## 4. What to Check — Maintainability

- [ ] Code is readable without needing the author to explain it in person
- [ ] Naming is clear and consistent with the rest of the codebase
- [ ] No copy-pasted duplication that should've been a shared function
- [ ] Complexity matches the problem — not over-engineered for a simple need, not a hack for a complex one
- [ ] Comments explain *why* where the reasoning isn't obvious, not narrating *what*

---

## 5. Giving Feedback

- [ ] Feedback is specific — points to the exact line/pattern, not a vague "this could be better"
- [ ] Feedback distinguishes "this must change before merge" from "consider this, your call" — don't blur blocking vs. optional
- [ ] Feedback explains *why*, not just *what to change* — so the person learns the reasoning, not just the fix
- [ ] Tone is about the code, not the coder ("this query will N+1 on large datasets" not "you always forget about performance")
- [ ] Something done well gets acknowledged too — review isn't only a list of problems

**AI instruction:** When drafting review comments for me, separate blocking issues from suggestions explicitly, and always include the reasoning behind a requested change, not just the instruction.

---

## 6. Receiving Feedback (When I'm the One Being Reviewed)

- [ ] Feedback is treated as being about the code, not a judgment of me
- [ ] Disagreement is discussed with reasoning, not silently overridden or silently complied with
- [ ] If I don't understand a comment, I ask rather than guessing what the reviewer meant
- [ ] Feedback patterns that repeat across reviews are a signal to fix the root habit, not just the individual instance

---

## 7. Approving & Merging

- [ ] Nothing marked "must fix" is left unresolved before approval
- [ ] Approving means I'd be comfortable being on-call if this breaks — not just "looks fine on skim"
- [ ] Size of the PR was reasonable to review properly — if it's too large to review carefully, that's flagged, not pushed through anyway
- [ ] CI/tests are green before merge, not merged with a promise to "fix the failing test after"

**AI instruction:** Never suggest merging a PR with a known-blocking issue "to save time" — always treat unresolved blocking feedback as a stop condition.

---

## 8. Review Turnaround & Team Practice

- [ ] Reviews happen promptly — a PR sitting unreviewed for days blocks the author and encourages bad habits (huge PRs, working around review)
- [ ] Review load is distributed as the team grows, not bottlenecked entirely on one person
- [ ] Recurring issues across multiple PRs become a documented standard (added to the relevant doc), not repeated as individual comments forever

---

## 9. Using AI Tools in Code Review (Meta)

- [ ] AI can do a first-pass review (catching obvious issues) — but final approval judgment is always a human's
- [ ] AI-flagged issues are verified before being passed to the author as feedback — don't relay a false positive as fact
- [ ] I don't let AI review replace understanding the change myself before approving it

**AI instruction:** When doing a first-pass AI review, clearly mark suggestions as "worth a human checking" rather than stating them as confirmed problems, since your review is not the same as an approval.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a bug or bad pattern slips through review, that's a signal to reread this list and add a specific line, not to write a whole new document.
