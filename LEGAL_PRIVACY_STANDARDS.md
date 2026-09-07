# Legal & Data Privacy Standards & Practices

> **Purpose:** This document is my personal working standard for handling data responsibly and staying aware of basic legal/compliance boundaries. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before building anything that stores or processes personal data.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my data-handling baseline. When helping me design systems that touch personal data, hold the work to these standards. If a request would create a compliance risk, flag it and offer options rather than silently going along with it.

**Context:** BPO company context — systems may touch employee data and client/customer data (potentially spanning multiple countries' data if clients are international). I am not a lawyer; this doc is engineering-level awareness, not legal advice — it's meant to flag when I should actually ask someone qualified.

---

## 1. What Counts as Sensitive Data (Awareness)

- [ ] I can name what personal data a given system actually stores (names, emails, IDs, financial info, health info, etc.)
- [ ] I distinguish between data I need to store and data I'm storing "just in case" — the latter is a liability, not an asset
- [ ] I know if any system touches data from a jurisdiction with specific rules (e.g., data from EU clients implicates GDPR-style obligations, even if the company itself isn't EU-based)

**AI instruction:** When designing a data model, ask what category of data each field represents (identifying, financial, health, etc.) so sensitivity is a deliberate design input, not an afterthought.

---

## 2. Data Minimization

- [ ] Only data actually needed for the feature is collected — no "let's grab everything in case we need it later"
- [ ] Optional fields are actually optional, not collected by default and never used
- [ ] Data no longer needed is deleted or archived per a real retention decision — not kept forever by default

**AI instruction:** When designing a form/data model, ask "is this field actually needed for the stated purpose" before including it — flag anything that looks like data collected without a clear use.

---

## 3. Storage & Access

- [ ] Sensitive fields are access-controlled — not every internal user can see everything by default
- [ ] Access to sensitive data is logged where feasible (who viewed/exported client PII, and when)
- [ ] Data isn't duplicated across systems without a reason — every copy is another place it can leak from
- [ ] Exported data (CSVs, reports) containing PII is handled carefully — not left in unprotected shared drives

---

## 4. Consent & Transparency

- [ ] If a system collects personal data directly from a person (not just internal employee data), there's some form of notice about what's collected and why
- [ ] Data used for a purpose different from what it was originally collected for is a flag to double check, not just proceed
- [ ] If in doubt about whether something needs explicit consent, that's a question for whoever handles compliance/legal — not a decision to make solo

**AI instruction:** If a request involves collecting data from external users/clients without clear notice of what's collected and why, flag that as worth checking rather than proceeding silently.

---

## 5. Third Parties & Data Sharing

- [ ] I know which third-party services (analytics, email providers, hosting) touch personal data as a byproduct
- [ ] Data sent to AI tools, external APIs, or SaaS platforms is data I've actually confirmed is okay to share there
- [ ] Vendor data-handling terms are at least skimmed for anything storing meaningful personal data, not assumed fine

**AI instruction:** If I'm about to paste real client/employee personal data into an AI tool for help with a task, flag that this may not be appropriate to share rather than just processing it.

---

## 6. Breach Awareness

- [ ] I know, roughly, what I'd need to do if a system holding personal data was breached (who to tell, how fast)
- [ ] "Best effort security" is documented as what it is — not oversold to stakeholders as bulletproof
- [ ] Any actual incident involving personal data gets escalated to whoever handles compliance, not just quietly patched

---

## 7. When to Stop and Ask a Professional

This document is engineering awareness, not legal advice. These situations mean "ask HR/legal/compliance," not "decide alone":

- [ ] A system will store data protected by a named regulation (GDPR, HIPAA, or similar) for the first time
- [ ] A client contract mentions specific data-handling requirements I haven't seen before
- [ ] A suspected data breach or leak has occurred
- [ ] I'm unsure whether a data retention period or deletion request is being handled correctly

**AI instruction:** Never present engineering-level guidance in this document as a substitute for legal advice. If a question sounds like it needs a lawyer or compliance officer, say so explicitly instead of answering as if it settles the matter.

---

## 8. Using AI Tools with Data (Meta)

- [ ] Real personal data (client, employee) is never pasted into an AI tool without confirming it's appropriate to share
- [ ] AI-suggested data handling patterns are treated as a starting point, not a compliance guarantee
- [ ] Any AI output claiming legal/regulatory compliance is treated with skepticism until verified

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a compliance question comes up that this doc doesn't answer, that's a signal to go ask a professional AND add a note here, not to guess and move on.
