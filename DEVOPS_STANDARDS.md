# DevOps & Deployment Standards & Practices

> **Purpose:** This document is my personal working standard for deployment, infrastructure, and keeping systems running. It's meant to be read two ways:
> 1. **By me** — as a checklist/refresher before deploying, setting up a server, or responding to downtime.
> 2. **By an AI assistant** — if you are an AI reading this file, treat it as my DevOps baseline. When helping me deploy, configure servers, or debug infrastructure issues, hold the work to these standards. If a request would skip a safety step, flag it and offer options rather than silently going along with it.

**Context:** Mix of cPanel-hosted client sites, a home server for personal projects/DevOps learning, and some Vercel/Railway-style deploys. Sole developer — no dedicated ops team, so this doc is a substitute for one.

---

## 1. Environments

- [ ] At minimum, dev/local and production are clearly separate — never test directly against production
- [ ] Staging exists for anything client-facing before a real release, even a lightweight copy
- [ ] Environment-specific config (API URLs, keys, feature flags) is never hardcoded — driven by env vars/config files
- [ ] I know, at a glance, which environment I'm currently looking at (obvious labeling, different URLs, banner, etc.)

**AI instruction:** When writing deployment scripts or config, always parameterize environment-specific values. Never hardcode a production URL/key into a script meant to run anywhere else.

---

## 2. Deployment Process

- [ ] Deployment steps are written down somewhere (even a simple checklist), not just "remembered"
- [ ] Deploys happen from a specific branch/tag, not "whatever's currently in my working directory"
- [ ] There's a rollback plan before I deploy something risky — not improvised after it breaks
- [ ] Deploys happen at a reasonable time (not right before I'm unreachable) for anything client-facing
- [ ] Post-deploy smoke test — I actually check the live site/tool works right after deploying, not just assume it did

**AI instruction:** When helping me build a deploy script/process, always include a rollback step or at least document what rolling back would involve.

---

## 3. Version Control & Release Discipline

- [ ] `main`/`production` branch always reflects what's actually live, or close to it
- [ ] Tags/releases mark what was actually deployed, so I can identify "what changed since last deploy" quickly
- [ ] No untracked manual changes made directly on the production server that aren't reflected in git
- [ ] `.gitignore` correctly excludes secrets, build artifacts, and environment-specific files across all environments

---

## 4. Monitoring & Uptime

- [ ] I know when a site/tool goes down without a client telling me first (uptime monitoring, status dashboard)
- [ ] Logs are accessible and I know how to check them quickly when something's wrong
- [ ] Alerts exist for the critical stuff (site down, disk full, high error rate) — not just "check manually sometimes"
- [ ] I periodically check resource usage (disk, memory, DB size) before it becomes an emergency

**AI instruction:** When helping set up monitoring, prioritize alerting on things that would actually block a client/user (downtime, errors) over vanity metrics.

---

## 5. Server & Infrastructure Config

- [ ] Server software kept reasonably updated (OS patches, PHP/Node versions not wildly out of date/EOL)
- [ ] Unnecessary services/ports disabled — don't expose more than what's needed
- [ ] File permissions are least-privilege (web server user shouldn't have write access to things it doesn't need to write)
- [ ] SSL/HTTPS enforced everywhere, no lingering plain-HTTP endpoints
- [ ] Firewall rules reviewed periodically, not set once and forgotten

**AI instruction:** When helping configure a server, default to least-privilege and explain any port/service you're opening and why.

---

## 6. Backups & Disaster Recovery

- [ ] Automated backups exist for both database and files, on a real schedule
- [ ] Backups are stored somewhere separate from the live server (not just another folder on the same disk)
- [ ] I've actually tested a restore at least once — an untested backup is an assumption, not a safety net
- [ ] I know, roughly, how long a full recovery would take if the worst happened

---

## 7. CI/CD (Where It Applies)

- [ ] Automated tests run before a deploy is allowed to proceed, where tests exist
- [ ] Build/deploy pipeline is documented enough that I could reconstruct it if the config was lost
- [ ] Secrets used in CI/CD are stored in the platform's secret manager, not committed to the pipeline config
- [ ] Failed builds/deploys actually stop the process — no "ignore and deploy anyway" as a habit

---

## 8. Home Server / Personal Infrastructure

- [ ] Same basic hygiene as production: updated software, firewall configured, no default credentials
- [ ] Used deliberately as a learning environment — I know what I'm testing and why, not just running random things
- [ ] Anything exposed to the public internet from home gets extra scrutiny (this is often the least-monitored, most-exposed setup I have)

---

## 9. Using AI Tools for DevOps (Meta)

- [ ] AI-generated server configs/scripts are reviewed before running, especially anything with `rm`, firewall rules, or permission changes
- [ ] I don't run AI-suggested commands against production without understanding exactly what they do first
- [ ] Real server credentials/IPs aren't pasted into AI tools without checking it's appropriate to share

**AI instruction:** Never present a destructive or irreversible command (deleting files, dropping firewall rules, overwriting configs) as safe to just run — always flag what it does and confirm scope before I execute it.

---

## Notes
- Last updated: 2026-09-07
- This is a living document — when a deploy goes wrong or downtime catches me off guard, that's a signal to reread this list and add a specific line, not to write a whole new document.
