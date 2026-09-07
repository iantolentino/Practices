# API Design Standards & Practices

> **Purpose:** This document is my personal working standard for designing and building APIs. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before designing a new endpoint or reviewing an existing API.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my API baseline. When helping me design or build APIs, hold the work to these standards. If a request would produce an inconsistent or unclear API, flag it and offer options rather than silently going along with it.

**Context:** Mostly internal tool APIs (PHP/Node) consumed by my own frontends, occasionally exposed to client integrations. No dedicated API gateway/platform team — I make these calls myself.

---

## 1. Structure & Naming

- [ ] Resource-based URLs, plural nouns (`/tickets`, not `/getTicket` or `/ticket_get`)
- [ ] Consistent naming convention across all endpoints (all snake_case or all camelCase, not mixed)
- [ ] Nesting reflects real relationships (`/clients/:id/tickets`) but doesn't go more than 2 levels deep
- [ ] HTTP methods used correctly: GET (read), POST (create), PUT/PATCH (update), DELETE (remove) — not everything as POST

**AI instruction:** When designing endpoints, default to REST conventions above unless I've specified GraphQL or another style. Flag any verb-in-URL pattern (`/getUser`) as inconsistent with the rest.

---

## 2. Request & Response Format

- [ ] Consistent response envelope across all endpoints (e.g., always `{ data, error, meta }` or whatever shape I've picked) — not ad hoc per endpoint
- [ ] Consistent date/time format (ISO 8601) across all responses
- [ ] Pagination format is consistent project-wide (same param names, same response shape) wherever it's used
- [ ] Only necessary data returned — no leaking internal fields (password hashes, internal IDs meant to stay internal)

**AI instruction:** Once a response shape is established in a project, reuse it exactly for new endpoints rather than inventing a new shape per feature.

---

## 3. Error Handling

- [ ] HTTP status codes used correctly (400 bad request, 401 unauthenticated, 403 unauthorized, 404 not found, 409 conflict, 500 server error) — not everything returning 200 with an error flag buried in the body
- [ ] Error responses have a consistent shape: a machine-readable code/type plus a human-readable message
- [ ] Error messages are specific enough to debug but don't leak internals (stack traces, SQL, file paths) to the client
- [ ] Validation errors specify which field failed and why, not just "invalid input"

**AI instruction:** Always map errors to the correct HTTP status code. Don't default to 200 for error cases, and don't leak stack traces or internal paths in error responses.

---

## 4. Authentication & Authorization

- [ ] Every non-public endpoint requires authentication, checked server-side
- [ ] Auth method is consistent across the API (all token-based, or all session-based — not mixed without reason)
- [ ] Tokens/keys have a clear expiration and refresh strategy where applicable
- [ ] Rate limiting exists on public or client-facing endpoints, especially auth endpoints (login, password reset)

---

## 5. Versioning

- [ ] API has a versioning strategy decided upfront (`/v1/...`, header-based, etc.) even if v1 is the only version for now
- [ ] Breaking changes get a new version, not a silent change to an existing one that could break consumers
- [ ] Deprecated endpoints are marked/communicated before removal, not deleted without warning

**AI instruction:** If a change to an existing endpoint would break current consumers (renamed field, changed type, removed field), flag it as a breaking change and suggest versioning instead of silently modifying it.

---

## 6. Documentation

- [ ] Every endpoint documented: method, path, required params, request/response shape, possible error codes
- [ ] Example requests/responses included, not just field lists
- [ ] Auth requirements stated explicitly per endpoint
- [ ] Docs updated in the same change as the code — not left to catch up "later"

**AI instruction:** When adding or changing an endpoint, generate or update the corresponding doc entry in the same response, not as an afterthought.

---

## 7. Performance & Reliability

- [ ] N+1 query patterns avoided in endpoint handlers (joins/batch fetches instead of per-item queries)
- [ ] Expensive operations are paginated or async (background job + polling/webhook) rather than blocking a request indefinitely
- [ ] Timeouts set on any outbound calls the API makes (to other services/DBs) — no indefinite hangs
- [ ] Idempotency considered for anything that could be retried (payments, ticket creation) — repeated calls shouldn't duplicate the effect

---

## 8. Security (API-Specific)

- [ ] Input validated server-side on every endpoint, same as general dev standard
- [ ] CORS configured deliberately (specific origins), not wide open (`*`) for anything handling auth/sensitive data
- [ ] Sensitive data never passed in URL query params (goes in logs/browser history) — use body or headers
- [ ] API keys/tokens treated as secrets — not embedded in frontend code where they'd be publicly visible, unless designed to be public

**AI instruction:** Never suggest putting sensitive data (tokens, passwords, PII) in a URL query string. Flag wide-open CORS (`*`) if the endpoint handles auth or sensitive data.

---

## 9. Using AI Tools for API Work (Meta)

- [ ] AI-generated endpoint code is reviewed against this doc before merging, not just checked for "does it return the right data"
- [ ] I ask AI to explain any deviation from the established response/error shape rather than accepting it silently
- [ ] Real API keys/tokens aren't pasted into AI tools for debugging

**AI instruction:** If asked to build an endpoint that doesn't match this project's existing conventions (naming, response shape, error format), point out the inconsistency before proceeding.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when an API inconsistency causes a real bug or integration headache, that's a signal to reread this list and add a specific line, not to write a whole new document.
