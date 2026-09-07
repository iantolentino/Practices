# Performance & SEO Standards & Practices

> **Purpose:** This document is my personal working standard for site performance and search visibility. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before launching a public-facing page or investigating why something feels slow.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my performance/SEO baseline. When helping me build or review public-facing pages, hold the work to these standards. If a request would hurt load time or discoverability, flag it and offer options rather than silently going along with it.

**Context:** Applies mainly to public-facing marketing/client sites, not internal tools (internal tools care about performance but not SEO). PHP/JS stacks, cPanel hosting.

---

## 1. Core Performance Basics

- [ ] Images compressed and appropriately sized (not a 4000px image displayed at 400px)
- [ ] Modern image formats used where possible (WebP/AVIF with fallback)
- [ ] CSS/JS minified for production builds
- [ ] Render-blocking scripts avoided or deferred/async where possible
- [ ] Fonts loaded efficiently (subset if possible, `font-display: swap` to avoid invisible text)

**AI instruction:** When generating frontend code, default to modern image formats, deferred non-critical scripts, and minification-ready structure unless told this is a quick prototype.

---

## 2. Loading Experience

- [ ] Largest Contentful Paint (LCP) target: under ~2.5s on a reasonable connection
- [ ] No layout shift as things load (reserve space for images/ads/embeds — avoid Cumulative Layout Shift)
- [ ] Critical content visible without waiting on non-essential scripts (analytics, chat widgets) to load first
- [ ] Tested on throttled/mobile network conditions, not just fast office wifi

---

## 3. Caching

- [ ] Static assets (images, CSS, JS) have appropriate cache headers set
- [ ] Browser caching leveraged so repeat visits are faster
- [ ] Server-side caching considered for expensive/repeated queries (especially on shared cPanel hosting where DB load matters)
- [ ] Cache invalidation strategy exists — updates actually show up, not stuck on stale cached versions

---

## 4. On-Page SEO Basics

- [ ] Every page has a unique, descriptive `<title>` and meta description
- [ ] One clear `<h1>` per page, logical heading hierarchy after that (not skipping levels or using headings for styling)
- [ ] URLs are readable and descriptive (`/services/web-development` not `/page?id=42`)
- [ ] Images have descriptive alt text (also an accessibility requirement, does double duty)
- [ ] Internal links use descriptive anchor text, not just "click here"

**AI instruction:** When generating page markup, always include a unique title/meta description placeholder and proper heading hierarchy by default — don't skip these as an afterthought.

---

## 5. Technical SEO

- [ ] `sitemap.xml` exists and is submitted/kept current
- [ ] `robots.txt` correctly allows the pages that should be indexed and blocks the ones that shouldn't (admin panels, internal tools)
- [ ] Canonical tags used where duplicate/similar content could exist
- [ ] Mobile-friendly/responsive — this is a hard requirement for search ranking, not optional
- [ ] HTTPS enforced site-wide (ranking factor, and a security requirement anyway)
- [ ] Structured data (schema.org markup) added where relevant (business info, articles, FAQs) if it's worth the effort for that page

---

## 6. Content & Discoverability

- [ ] Page content actually answers what the target search query is asking — not just keyword-stuffed
- [ ] Each important page has a clear, singular topic/purpose (not several unrelated topics competing on one page)
- [ ] Broken links and 404s are checked periodically, not left to accumulate

---

## 7. Measurement

- [ ] Real performance data checked periodically (Lighthouse, PageSpeed Insights, or real user monitoring) — not just "feels fast to me on my machine"
- [ ] Search visibility checked periodically (Search Console or equivalent) — not just assumed based on effort put in
- [ ] Performance regressions are caught before they compound (a slow page rarely gets fixed once traffic already dropped)

---

## 8. Using AI Tools for Performance/SEO (Meta)

- [ ] AI-suggested SEO tactics are sanity-checked against current best practices — SEO advice ages quickly and AI training data can be stale
- [ ] AI-generated meta descriptions/titles are reviewed for accuracy, not just "sounds good"
- [ ] Performance claims from AI ("this will speed things up") are verified with actual measurement, not taken on faith

**AI instruction:** If you're not confident a specific SEO tactic is still current best practice (rules change), say so explicitly rather than presenting it with full confidence.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a page turns out to be slow or invisible in search after launch, that's a signal to reread this list and add a specific line, not to write a whole new document.
