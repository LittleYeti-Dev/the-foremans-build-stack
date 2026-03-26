# Integration Map — Foreman

All cross-project integration points tracked with status.

## Active Integration Points

| ID | Source | Target | Type | Status | Notes |
|----|--------|--------|------|--------|-------|
| INT-001 | WP RSS Aggregator | WordPress.com (The Markdown) | RSS Ingest → CMS | ACTIVE | Current production pipeline. 32+ items/day ingest. Under review per AD-0016. |
| INT-002 | WordPress.com | justin-kuiper.com | CMS → Public Site | ACTIVE | WordPress.com managed hosting serves all pages. |
| INT-003 | Squarespace DNS | WordPress.com hosting | DNS → Hosting | ACTIVE | Domain routing. Under review per AD-0016/AD-0007. |
| INT-004 | Jetpack | WordPress.com | Analytics + CDN | ACTIVE | Stats, likes, sharing. Platform-dependent. |
| INT-005 | WordPress.com | RSS Feed (/feed/the-markdown/) | CMS → RSS Output | ACTIVE | Public RSS feed for The Markdown content. |

## Planned Integration Points (Non Sequitur Ecosystem)

| ID | Source | Target | Type | Status | Notes |
|----|--------|--------|------|--------|-------|
| INT-010 | RSS Feeds (external) | Editorial Board | Signal Capture | PLANNED | Replaces INT-001 with ecosystem-native ingest. |
| INT-011 | Editorial Board | Content Queue (/content/queue/) | Classification → Queue | PLANNED | New. Signal → Vector → Wrap → Drop pipeline. |
| INT-012 | GitHub Actions | Platform API (TBD) | Scheduled Publish | PLANNED | Autonomous publishing. Platform depends on AD-0016. |
| INT-013 | GitHub Actions | Claude API | AI Processing | PLANNED | Content scoring, classification, reformatting. |
| INT-014 | Content Queue | The Markdown (site) | Queue → Live Signal | PLANNED | Published content surfaces on /the-markdown. |
| INT-015 | GitHub Actions | Social APIs | Cross-post | PLANNED | X, LinkedIn, etc. Per AD-0004 (still valid). |
| INT-016 | GitHub Actions | Buffer Health Monitor | Queue → Monitoring | PLANNED | Weekly check: is 2–4 week buffer maintained? |
| INT-017 | non-sequitur-web repo | GitHub Pages/Cloudflare Pages | Build → Deploy | PLANNED | Site deployment. Platform depends on AD-0016. |
| INT-020 | Published Content | LinkedIn API | Distribution → Expanded Insight (1–3 paragraphs) | PLANNED | Per AD-0021. Phase 1: manual/Loomly. Phase 3: pipeline-driven. |
| INT-021 | Published Content | X API | Distribution → Short Signal (1–3 posts) | PLANNED | Per AD-0021. Phase 1: manual/Loomly. Phase 3: pipeline-driven. |
| INT-022 | Published Content | Instagram API | Distribution → Visual + Short Caption | PLANNED | Per AD-0021. Phase 1: manual/Loomly. Phase 3: pipeline-driven. |
| INT-023 | Published Content | Medium API | Distribution → Long-form + Canonical Link | PLANNED | Per AD-0021. Canonical link always back to main site. |
| INT-024 | Published Content | GitHub Pages | Distribution → Static Mirror / Archive | PLANNED | Per AD-0021. Optional technical/archive layer. |
| INT-030 | The Markdown | Agentics (/agentics) | Thought Leadership → Service Credibility | PLANNED | Per AD-0022. Markdown signal content feeds Agentics proof layer. |
| INT-031 | Builds | Agentics (/agentics) | Execution Proof → Service Portfolio | PLANNED | Per AD-0022. Active builds demonstrate capability on Agentics page. |
| INT-032 | Architecture | Agentics (/agentics) | Frameworks → Service Methodology | PLANNED | Per AD-0022. Architecture decisions feed Agentics consulting credibility. |
| INT-033 | Agentics | LinkedIn | Service Positioning → Lead Generation | PLANNED | Per AD-0023. Profile optimization + content-to-lead conversion. |
| INT-034 | Agentics | Upwork | Service Packages → Marketplace Listings | PLANNED | Per AD-0023. Defined offerings with clear scope and pricing. |

## Planned Integration Points (ACI / GCOL — per NSQ-GAP-001)

| ID | Source | Target | Type | Status | Notes |
|----|--------|--------|------|--------|-------|
| INT-040 | ACI (React app) | GCOL State Machine | Command → Routing | PLANNED | ACI command lane sends intent to GCOL for tower routing. Per AD-0033. |
| INT-041 | ACI (React app) | Content Pipeline (GitHub Actions) | Status Poll → Live Ops Board | PLANNED | ACI reads pipeline state for live ops status display. |
| INT-042 | ACI (React app) | Alistair Prime (Socket IPC) | Command → Interpretive Layer | PLANNED | ACI-to-Alistair bridge. Persona context passed with command. Phase 3. |
| INT-043 | ACI (React app) | RoboStack (K3s API) | Command → Execution Substrate | PLANNED | ACI-to-RoboStack bridge. Thin API for command/status. Phase 3. |
| INT-044 | GCOL State Machine | Tower Personas | Routing → Bounded Domain | PLANNED | GCOL routes work to appropriate tower based on intent + persona. Phase 3. |
| INT-045 | GCOL State Machine | ACI Live Ops Board | State Events → Visibility | PLANNED | GCOL emits events at each transition for ops status board. Phase 3. |
| INT-046 | Skipjack Middleware | All Integration Points | Validation → Halt/Continue | PLANNED | Cross-cutting. Every INT-xxx gets Skipjack compliance check. Phase 2. |
| INT-047 | ACI (React app) | Cloudflare Pages | Build → Deploy | PLANNED | ACI deployed as standalone app on Cloudflare Pages. Per AD-0033. |

## Deprecated (Pending Gate 1)

| ID | Source | Target | Type | Status | Notes |
|----|--------|--------|------|--------|-------|
| INT-006 | GitLab | GitLab Pages | VCS → Staging | DEPRECATED | Superseded by AD-0017 (GitHub). |

---

*Created 2026-03-23 — Non Sequitur Ecosystem rebuild session.*
*Integration points INT-010 through INT-017 are PLANNED pending Gate 1 (AD-0016 platform decision).*
*INT-020 through INT-024 added 2026-03-23 — Distribution Layer (AD-0021) integration points.*
*INT-030 through INT-034 added 2026-03-23 — Agentics commercial layer (AD-0022, AD-0023) integration points.*
*INT-040 through INT-047 added 2026-03-26 — ACI/GCOL/Skipjack integration points per NSQ-GAP-001 reconciliation.*
