# Database Standards & Practices

> **Purpose:** This document is my personal working standard for database design, querying, and maintenance. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before designing a schema, writing a migration, or optimizing a slow query.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my database baseline. When helping me design schemas, write queries, or review migrations, hold the work (mine or yours) to these standards. If a request conflicts with a section below, flag it and offer options rather than silently going along with it.

**Context:** Mostly MySQL/MariaDB (cPanel-hosted client work) and SQLite (smaller internal tools). PHP/Python backends. Sole developer — no DBA safety net, so these standards substitute for one.

---

## 1. Schema Design

- [ ] Every table has a primary key — never rely on row order or implicit rowid alone
- [ ] Foreign keys are declared explicitly (not just "implied" by a matching column name)
- [ ] Data types match the data — no storing dates/numbers/booleans as generic strings
- [ ] Nullable columns are a deliberate choice, not a default — ask "can this actually be empty?"
- [ ] Normalize by default (3NF); only denormalize when I can name the specific performance reason
- [ ] Naming is consistent — one convention for table names (singular/plural), column names (snake_case vs camelCase), and stick to it project-wide
- [ ] Timestamps (`created_at`, `updated_at`) on tables where history/audit could ever matter

**AI instruction:** When designing a schema with me, default to 3NF and ask before denormalizing. Flag any nullable column that looks like it should be required, and any missing foreign key relationship.

---

## 2. Indexing

- [ ] Indexes exist on foreign keys and any column used in `WHERE`, `JOIN`, or `ORDER BY` on large tables
- [ ] I'm not over-indexing — every index has a cost on writes, so each one should map to an actual query pattern
- [ ] Composite indexes are ordered correctly (most selective / most-queried column first)
- [ ] I've checked query plans (`EXPLAIN`) on anything slow before guessing at the fix

**AI instruction:** When suggesting an index, name the specific query pattern it helps with. Don't suggest indexing every column "just in case."

---

## 3. Queries

- [ ] Parameterized queries / prepared statements always — no string-concatenated SQL, ever
- [ ] `SELECT *` avoided in application code — select only needed columns
- [ ] Joins used instead of N+1 query loops (fetch related data in one query, not one query per row)
- [ ] Pagination (`LIMIT`/`OFFSET` or keyset pagination) on anything that could grow unbounded
- [ ] Transactions wrap any multi-step write that must succeed/fail together
- [ ] Aggregate queries (`COUNT`, `SUM`, etc.) pushed to the database, not pulled into app code and looped

**AI instruction:** Any SQL you write for me must be parameterized by default. If you write a query with `SELECT *` or a loop-based N+1 pattern, treat that as something to flag, not ship silently.

---

## 4. Migrations & Schema Changes

- [ ] Every schema change is a tracked migration file, not a manual edit run once on the live DB
- [ ] Migrations are reversible where possible (have a rollback path)
- [ ] Destructive changes (dropping columns/tables) are backed up first, and delayed from the "add" step when possible (add new → migrate data → drop old, not all at once)
- [ ] Migrations tested on a copy/staging environment before running on production data

**AI instruction:** When writing a migration, always include the down/rollback step unless I explicitly say it's not needed. Flag destructive changes explicitly rather than combining add+drop in one step.

---

## 5. Data Integrity

- [ ] Constraints enforced at the DB level (NOT NULL, UNIQUE, foreign keys) — not just validated in app code
- [ ] Cascading deletes/updates are deliberate choices, not defaults left unexamined
- [ ] Enum-like columns use an actual constrained type or a lookup table, not a free-text string trusted to stay consistent
- [ ] No silent data duplication — if the same fact lives in two places, there's a clear reason and a sync strategy

**AI instruction:** If a design relies on app-code validation alone for something a DB constraint could enforce, point that out as a gap.

---

## 6. Backups & Recovery

- [ ] Backups run on a schedule, not "whenever I remember"
- [ ] I've actually tested restoring a backup at least once — a backup that's never been restored is unverified
- [ ] Before any risky migration/bulk update, a manual backup point exists specifically for that change
- [ ] Backup storage is separate from the live server (not just another folder on the same machine)

---

## 7. Performance & Maintenance

- [ ] Slow queries are found proactively (slow query log / periodic review), not just when a client complains
- [ ] Table sizes and growth are checked periodically — what's small now may not stay small
- [ ] Connection pooling / connection limits understood for the hosting environment (especially shared cPanel hosting)
- [ ] Old/unused tables and columns get cleaned up, not left as silent debt

---

## 8. Security

- [ ] DB credentials never hardcoded — env vars/config only, never committed to git
- [ ] DB user accounts follow least privilege (app user shouldn't have DROP/ALTER rights in production if it doesn't need them)
- [ ] Sensitive fields (passwords, tokens) never stored in plaintext or reversible encryption — hashed where appropriate
- [ ] Client/production data never copied to a local/dev machine without a reason and without stripping sensitive fields first

**AI instruction:** Treat this section as non-negotiable, same as the security section in my dev standards. Push back once if a request would violate it, explain why briefly, then follow my explicit override if I confirm — don't silently comply by default.

---

## 9. Using AI Tools for Database Work (Meta)

- [ ] I read and understand any AI-generated SQL/migration before running it — no blind execution, especially on production
- [ ] AI is used for scaffolding queries/schemas and explaining query plans — not for making the final call on production-affecting changes without my review
- [ ] I ask "why" when AI suggests an index or schema pattern I don't recognize
- [ ] Never run AI-suggested destructive SQL (DROP, DELETE without WHERE, TRUNCATE) against production without a manual double-check

**AI instruction:** Never present a destructive query (DROP/TRUNCATE/DELETE without WHERE) as something to just run — always call out that it's destructive and confirm scope with me first.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a schema decision or query bites me later, that's a signal to reread this list and possibly add a line, not to add a whole new document.
