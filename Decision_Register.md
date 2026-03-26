# Decision Register — Foreman

Standing technical decisions with context and trade-offs.

## Open Decisions (Require Resolution)

| ID | Decision | Status | Owner | Blocking | Age (days) |
|----|----------|--------|-------|----------|------------|
| ~~TD-003~~ | ~~DNS provider~~ | CLOSED | — | — | — |

*TD-001, TD-004, TD-005 moved to Closed — 2026-03-26. TD-002 previously closed.*

## Closed Decisions

| ID | Decision | Resolution | Date | Rationale |
|----|----------|-----------|------|-----------|
| TD-006 | Repo name for ecosystem | `non-sequitur-web` on GitHub | 2026-03-23 | Per Gunther handover. Umbrella repo for site, content, editorial, automation, docs. |
| TD-007 | VCS platform for ecosystem | GitHub (supersedes GitLab per AD-0006) | 2026-03-23 | GitHub Actions required for automation pipeline. Source of truth must be GitHub-based per handover requirements. |
| TD-008 | Content pipeline model | Signal → Vector → Wrap → Drop | 2026-03-23 | Locked by Gunther handover. Non-negotiable. |
| TD-009 | Design system color palette | Locked (see handover) | 2026-03-23 | 90% neutral / 10% signal. Electric Blue #2F6BFF primary. |
| TD-010 | Editorial Board execution modes | Batch / Passive / Intervention | 2026-03-23 | Locked by Gunther handover. System must support all three. |
| TD-011 | the-markdown repo strategy | Fold into non-sequitur-web. Archive GitLab repo as historical reference. | 2026-03-24 | One ecosystem, one repo, one truth. Split repos create sync burden incompatible with solo-operator model and 3–7 day autonomous operation. Per Yeti directive. |
| TD-012 | Content Lab backend platform | GitHub-native (not GitLab). GitHub Actions as workflow engine. Git-as-database for pipeline state. | 2026-03-24 | Reconciles requirements doc "GitLab or Git-style" with standing AD-0017. GitHub locked. |
| TD-013 | Security controls timing | Sprint-operational — built during Phase 4/5, not deferred. | 2026-03-24 | Per Yeti directive. AD-0031 is part of the sprint, not a post-launch concern. |
| TD-001 | Platform choice | Cloudflare Pages + Hugo. Deployed and running in production. wrangler.toml + deploy workflow in non-sequitur-web. Pipeline Bot LIVE. | 2026-03-26 | Resolved in code by Sprint 5-6. Cloudflare free tier covers Pages + Workers + R2. Hugo chosen as SSG. Gate 1 passed by build. |
| TD-004 | Content migration strategy | Incremental via GitHub Actions pipeline. Pipeline Bot auto-publishes. Content directory populated in non-sequitur-web. | 2026-03-26 | Pipeline-driven incremental migration. No bulk export needed. Legacy WP content archived as reference. |
| TD-005 | Static site generator | Hugo. hugo.toml in non-sequitur-web repo root. NSQ Wave theme built. | 2026-03-26 | Hugo selected and deployed. Fast builds, Go templating, strong content organization. Deployed via Wrangler to Cloudflare Pages. |
| TD-003 | DNS provider | Cloudflare. Domain registered at Google Domains (ex-Squarespace), DNS attached to Cloudflare via integration. No manual nameserver migration required. | 2026-03-26 | Domain expires 2026-04-08 — verify auto-renew is active at Google Domains. feed.justin-kuiper.com also mapped. |

---

*Created 2026-03-23 — Non Sequitur Ecosystem rebuild session.*
*TD-011 through TD-013 added 2026-03-24 — Pipeline analysis session decisions.*
*TD-001, TD-004, TD-005 closed 2026-03-26 — Reconciliation session. Resolved in code, documented retroactively per NSQ-GAP-001.*
*TD-003 closed 2026-03-26 — DNS attached to Cloudflare via Google Domains integration. No manual nameserver migration needed.*
