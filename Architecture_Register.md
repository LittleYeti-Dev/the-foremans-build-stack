# Architecture Register — Foreman

All architecture decisions tracked with status and rationale.

| ID | Date | Decision | Project | Status | Superseded By |
|----|------|----------|---------|--------|---------------|
| AD-0001 | 2026-03-07 | WordPress.com as CMS/hosting platform | The Markdown | SUPERSEDED | AD-0016 (Cloudflare Pages + Hugo) |
| AD-0002 | 2026-03-07 | WP RSS Aggregator for feed ingest | The Markdown | SUPERSEDED | AD-0027 (GitHub Actions pipeline) |
| AD-0003 | 2026-03-07 | 6-layer architecture (Data/Ingest/AI/Publish/Orchestration/Monitoring) | The Markdown | ACCEPTED | — |
| AD-0004 | 2026-03-07 | Multi-platform social API integration (X, LinkedIn, Instagram, YouTube, Medium) | The Markdown | ACCEPTED | — |
| AD-0005 | 2026-03-07 | Claude API for content scoring and reformatting | The Markdown | ACCEPTED | — |
| AD-0006 | 2026-03-07 | GitLab Premium for version control and CI/CD | Build Stack Workshop | SUPERSEDED | AD-0017 |
| AD-0007 | 2026-03-07 | Squarespace DNS → WordPress.com hosting for justin-kuiper.com | justin-kuiper.com | SUPERSEDED | DNS now on Cloudflare via Google Domains integration. TD-003 CLOSED 2026-03-26. |
| AD-0008 | 2026-03-07 | GitLab Pages for dev/staging environment | justin-kuiper.com | SUPERSEDED | AD-0017 |
| AD-0009 | 2026-03-08 | Local-first persistence with sync to Drive + GitLab | ALL | ACCEPTED | — |
| AD-0010 | 2026-03-08 | Four-function AI team (Overwatch/Taskmaster/Foreman/Developer) — expanded to full Tower Model per NSQ-ECO SSOT: Yeti Admin, Architect, Builder, Editor, Creative, Engineer, Legal Ops, Fin/Compliance, Taskmaster. Original 4 functions remain as core; 5 additional tower personas added for ecosystem governance. | ALL | ACCEPTED | Updated 2026-03-26 per NSQ-GAP-001 |
| AD-0011 | 2026-03-23 | Unix socket for task submission — local IPC only, no network exposure. Outbound HTTPS for external service connectors (GitHub, Claude API, etc.). Future HTTP REST interface gets its own ADR when remote-caller use case materializes. | Alistair Prime | ACCEPTED | — |
| AD-0012 | 2026-03-23 | Minimal command set (help, status, memory, exit) with extensible registry pattern. Commands operate the runtime; free input falls through to engine as task. New commands = one handler + one registration line. Parser never changes. | Alistair Prime | ACCEPTED | — |
| AD-0013 | 2026-03-23 | Simple 3-state task lifecycle: pending → running → done. Success/failure is a result field, not a state. Queued and cancelled states are additive future expansions — state machine grows without breaking. | Alistair Prime | ACCEPTED | — |
| AD-0014 | 2026-03-23 | SQLite + PVC for K3s persistence. No code changes from local to cluster. Memory layer abstracted behind sqlite_store.py — postgres_store.py is a clean future swap via config-driven backend selection. | Alistair Prime | ACCEPTED | — |
| AD-0015 | 2026-03-23 | Hybrid NLP model source — model-agnostic interface contract (input: raw text, output: structured intent + entities). Claude API as default backend, self-hosted LLM as future fallback. Config-driven backend selection. Design only this sprint, no build. | Alistair Prime | ACCEPTED | — |

| AD-0016 | 2026-03-23 | Platform Decision: WordPress.com (refactor) vs Cloudflare stack (migrate) vs Hybrid (headless WP + static). Full evaluation required before implementation. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0017 | 2026-03-23 | GitHub as primary VCS and CI/CD platform for non-sequitur-web. Supersedes GitLab (AD-0006, AD-0008). Umbrella monorepo strategy with the-markdown folded in. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0018 | 2026-03-23 | The Markdown as integrated subsystem within non-sequitur-web, not a separate property. Content lives in /content/, surfaces at /the-markdown. Editorial Board is intake system, GitHub Actions is publish mechanism. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0019 | 2026-03-23 | Content pipeline contract: Signal → Vector → Wrap → Drop. All content follows this flow. Frontmatter schema enforced at queue entry. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0020 | 2026-03-23 | Editorial Board as formal subsystem with three execution modes: Batch (active sessions), Passive (autonomous drip), Intervention (manual override). | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0021 | 2026-03-23 | Distribution Layer as post-pipeline amplification subsystem. Platforms: LinkedIn, X, Instagram, Medium, GitHub Pages. Content originates in pipeline, never on-platform. Per-platform transformation rules. 3-phase automation: manual/Loomly → templated semi-auto → pipeline-driven. Refines AD-0004. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0022 | 2026-03-23 | Agentics as core ecosystem lane — commercial/service layer. Page at /agentics. Fed by The Markdown (thought leadership), Builds (execution proof), Architecture (frameworks). Integrated with content pipeline, not standalone. Services: AI agent dev, automation pipelines, web builds, cyber-secure architecture. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0023 | 2026-03-23 | Agentics distribution and work generation via LinkedIn (primary professional channel) and Upwork (freelance marketplace). Dedicated sprint phase for profile build-out, service positioning, package definition, and lead generation workflow. Content-to-lead conversion: site → social → inbound. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0024 | 2026-03-23 | Branch strategy: main + feature/* branches. Main = production-ready (push to main triggers deploy). Feature branches for all work, merged to main via PR. Solo-operator simplified model — no dev branch unless team scales. PRs serve as review gate. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0025 | 2026-03-24 | Content Lab as formal backend subsystem with Git-native persistence. Owns ingestion, pipeline tracking, phase management, and Kanban-style workflow. GitHub repo is the database — no external persistence layer. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0026 | 2026-03-24 | PipelineItem data model: YAML frontmatter + Markdown body. Stage-as-directory structure (/content/pipeline/{signal,vector,wrapper,wave-drop}/). Append-only stage content. Outputs and cross-links as first-class fields. Extensible via meta field. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0027 | 2026-03-24 | GitHub Actions as workflow engine for content pipeline: ingest.yml, classify.yml, advance.yml, publish.yml, distribute.yml, digest.yml. Scheduled and event-driven triggers. All automation runs in CI, no external orchestrator. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0028 | 2026-03-24 | Wave + Kanban hybrid UI for The Markdown page. Primary: horizontal swim lane wave visualization (reader experience). Secondary: Kanban toggle (creator/operational view). Wave section augments existing mockup sections, does not replace them. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0029 | 2026-03-24 | Comment system deferred to Phase 6+. Giscus (GitHub Discussions-backed) as interim solution. Custom tiered commenting only if Giscus proves insufficient. Comments are an additive layer, not a pipeline dependency. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0030 | 2026-03-24 | Content migration: existing 2,000+ WordPress items enter as PipelineItems at published stage (/content/published/) with inferred frontmatter. Migration script maps WP taxonomy to pipeline categories. Git history starts at migration — WP history is archived reference. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0031 | 2026-03-24 | Content Pipeline Security Controls. Three layers: (1) Secrets management — GitHub Secrets for all API keys, no secrets in code/logs/frontmatter, 90-day rotation for social tokens, 180-day for Claude API. (2) Content integrity — repo private, branch protection on main, GPG-signed commits, PR-only merges per AD-0024. (3) Pipeline input validation — RSS input sanitization, allowlisted feed URLs in config, Claude API call sandboxing (structured output only), AI confidence threshold for auto-advance with manual review fallback. Routed to Overwatch for formal review before any automation goes live. Sprint-operational — built during Phase 4/5, not deferred. | Non Sequitur Ecosystem | ACCEPTED | — |

| AD-0032 | 2026-03-26 | NSQ-ECO (LittleYeti-Dev/NSQ-ECO) as governing doctrine repository. SSOT-core-requirements.md (v1 + v2), executive-mediation.md, taskmaster-persona-and-command-routing.md, and live-operations-status-board.md are the requirement baseline. All architecture decisions must reconcile with SSOT requirements. If as-built diverges from SSOT, the register is updated — the SSOT is amended only by Yeti directive. | Non Sequitur Ecosystem | ACCEPTED | — |
| AD-0033 | 2026-03-26 | ACI (Agentics Command Interface) as standalone React application, separate from Hugo public site (non-sequitur-web). Deployed on Cloudflare Pages. ACI is the operational cockpit — Morning Command Deck, persona routing, command lane, live ops status, approvals, agent handoffs. Hugo site is the public UXL surface. Both deploy on Cloudflare but are distinct applications with distinct purposes. | Non Sequitur Ecosystem | ACCEPTED | — |

*Reconstructed from session transcripts 2026-03-07 and 2026-03-08. Detail may be incomplete.*
*AD-0011 through AD-0015 added 2026-03-23 — Alistair Prime sprint planning session with Yeti.*
*AD-0016 through AD-0020 added 2026-03-23 — Non Sequitur Ecosystem rebuild handover from Gunther.*
*AD-0021 added 2026-03-23 — Distribution Layer requirement from Yeti mid-session.*
*AD-0022, AD-0023 added 2026-03-23 — Agentics commercial layer requirement from Yeti.*
*AD-0024 added 2026-03-23 — Branch strategy locked per Yeti approval.*
*AD-0025 through AD-0030 added 2026-03-24 — Markdown Pipeline analysis response (NSQ-MD-ANALYSIS-001).*
*AD-0031 added 2026-03-24 — Content Pipeline Security Controls, per Yeti directive. Sprint-operational, not deferred.*
*AD-0016 through AD-0031 status changed from PROPOSED to ACCEPTED — 2026-03-24. Greenlit by Yeti.*
*AD-0001 SUPERSEDED, AD-0002 SUPERSEDED, AD-0007 updated, AD-0010 expanded — 2026-03-26. Reconciliation per NSQ-GAP-001.*
*AD-0032, AD-0033 added 2026-03-26 — NSQ-ECO doctrine recognition and ACI architecture decision.*
