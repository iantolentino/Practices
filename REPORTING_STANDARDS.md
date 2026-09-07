# Report & Data Presentation Standards & Practices

> **Purpose:** This document is my personal working standard for building reports, dashboards, and any data presented for someone else to make a decision from. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before building a dashboard, exporting a report, or presenting metrics/scores to a stakeholder.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my reporting baseline. When helping me design reports, dashboards, or scoring systems, hold the work to these standards. If a request would produce a misleading or context-free number, flag it and offer options rather than silently going along with it.

**Context:** Several of my systems produce scores/metrics people act on directly (ICP scoring, employee performance tracker, client health tracker). Getting these right matters more than most features, since a bad chart can drive a real bad decision.

---

## 1. Before Building a Report

- [ ] I know who's reading this and what decision it's meant to inform — a report with no decision behind it is probably not needed
- [ ] I know what "good" and "bad" look like for this metric before I build the display — not deciding after seeing the data
- [ ] I've asked whether the underlying data is actually reliable enough to report on (garbage in, confident-looking garbage out)

**AI instruction:** Before designing a dashboard/report, ask what decision it supports and who reads it. Don't default to "show everything" without a stated purpose.

---

## 2. Data Accuracy

- [ ] The calculation behind every number is something I can explain in one sentence, not a black box even to me
- [ ] Aggregations (averages, sums, percentages) are appropriate for the data (e.g., average of a skewed distribution can mislead — consider median too)
- [ ] Sample size / data volume is visible where it matters (a "100% satisfaction" score from 1 response is misleading without that context)
- [ ] Time periods are labeled explicitly (this month vs. all-time vs. rolling 90 days) — never an unlabeled number that could mean any of these

**AI instruction:** When building a metric/score, always surface the sample size or data volume behind it alongside the number itself, not just the final aggregate.

---

## 3. Avoiding Misleading Presentation

- [ ] Chart type matches the data (trend over time → line, comparison across categories → bar, part-of-whole → pie only if few categories)
- [ ] Y-axis doesn't start at a misleading non-zero point to exaggerate a difference, unless there's a clear labeled reason
- [ ] Color coding is consistent and meaningful (red always means the same kind of "bad" across the whole report, not reused for different meanings)
- [ ] Comparisons are apples-to-apples (don't compare this month's partial data to last month's complete data without noting it)

**AI instruction:** Flag any chart design that could visually exaggerate a difference (truncated axis, cherry-picked time range) even if I didn't ask for it to be flagged.

---

## 4. Scoring Systems Specifically

(Relevant to ICP scoring, performance evaluations, client health scores)

- [ ] Scoring criteria are documented and explainable to the person being scored/evaluated — not a mystery formula
- [ ] Weightings between criteria are deliberate choices, not arbitrary defaults
- [ ] Edge cases are considered (what happens with missing data — does it silently count as zero/worst, or get excluded/flagged?)
- [ ] Scores are periodically sanity-checked against reality (does a "high score" client/employee actually match what people who know them would say?)

**AI instruction:** When helping design a scoring formula, ask how missing/partial data should be handled explicitly — don't let it silently default to the worst or best case without a decision.

---

## 5. Presenting to Stakeholders

- [ ] The headline number/insight is stated up front — don't bury the point in a wall of charts
- [ ] Context is included (compared to what? is this good or concerning?) — a number alone isn't information
- [ ] Uncertainty is acknowledged where it exists ("based on limited data so far") rather than presented with false confidence
- [ ] Actionable takeaway is clear, if there is one — "here's what this suggests we do," not just "here's data"

**AI instruction:** When drafting a report summary, lead with the headline insight and its context, not a list of every number available.

---

## 6. Access & Sensitivity

- [ ] Reports containing personal/performance data (employee scores, client health data) are access-controlled appropriately
- [ ] Aggregate/anonymized views are used where individual-level detail isn't actually needed for the decision
- [ ] Exported reports (PDF/CSV) are treated with the same sensitivity as the source data — an export is still the data

---

## 7. Automation & Refresh

- [ ] Automated/recurring reports are checked periodically for whether they're still accurate as the underlying system changes
- [ ] Stale data is labeled as stale ("last updated X") rather than presented as if it's live when it isn't
- [ ] Manual reports have a documented process so they're reproducible, not "however I happened to build it that one time"

---

## 8. Using AI Tools for Reports (Meta)

- [ ] AI-generated summaries of data are checked against the actual numbers before sharing — AI can misstate or overstate a trend
- [ ] I don't let AI's narrative framing overstate certainty ("this proves X" when the data merely "suggests X")
- [ ] Real client/employee data isn't pasted into an AI tool to "help write the report" without checking that's appropriate to share

**AI instruction:** When summarizing data for me, use careful language calibrated to the actual strength of the pattern (e.g., "suggests," "one possible explanation") rather than overstating certainty. Never state a causal claim the data only shows as correlation.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a report misleads someone into a bad decision, that's a signal to reread this list and add a specific line, not to write a whole new document.
