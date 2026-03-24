# Architecture Register — Foreman

All architecture decisions tracked with status and rationale.

| ID | Date | Decision | Project | Status | Superseded By |
|----|------|----------|---------|--------|---------------|
| AD-0001 | 2026-03-07 | WordPress.com as CMS/hosting platform | The Markdown | ACCEPTED | — |
| AD-0002 | 2026-03-07 | WP RSS Aggregator for feed ingest | The Markdown | ACCEPTED | — |
| AD-0003 | 2026-03-07 | 6-layer architecture (Data/Ingest/AI/Publish/Orchestration/Monitoring) | The Markdown | ACCEPTED | — |
| AD-0004 | 2026-03-07 | Multi-platform social API integration (X, LinkedIn, Instagram, YouTube, Medium) | The Markdown | ACCEPTED | — |
| AD-0005 | 2026-03-07 | Claude API for content scoring and reformatting | The Markdown | ACCEPTED | — |
| AD-0006 | 2026-03-07 | GitLab Premium for version control and CI/CD | Build Stack Workshop | ACCEPTED | — |
| AD-0007 | 2026-03-07 | Squarespace DNS → WordPress.com hosting for justin-kuiper.com | justin-kuiper.com | ACCEPTED | — |
| AD-0008 | 2026-03-07 | GitLab Pages for dev/staging environment | justin-kuiper.com | ACCEPTED | — |
| AD-0009 | 2026-03-08 | Local-first persistence with sync to Drive + GitLab | ALL | ACCEPTED | — |
| AD-0010 | 2026-03-08 | Four-function AI team (Overwatch/Taskmaster/Foreman/Developer) | ALL | ACCEPTED | — |
| AD-0011 | 2026-03-23 | Unix socket for task submission — local IPC only, no network exposure. Outbound HTTPS for external service connectors (GitHub, Claude API, etc.). Future HTTP REST interface gets its own ADR when remote-caller use case materializes. | Alistair Prime | ACCEPTED | — |
| AD-0012 | 2026-03-23 | Minimal command set (help, status, memory, exit) with extensible registry pattern. Commands operate the runtime; free input falls through to engine as task. New commands = one handler + one registration line. Parser never changes. | Alistair Prime | ACCEPTED | — |
| AD-0013 | 2026-03-23 | Simple 3-state task lifecycle: pending → running → done. Success/failure is a result field, not a state. Queued and cancelled states are additive future expansions — state machine grows without breaking. | Alistair Prime | ACCEPTED | — |
| AD-0014 | 2026-03-23 | SQLite + PVC for K3s persistence. No code changes from local to cluster. Memory layer abstracted behind sqlite_store.py — postgres_store.py is a clean future swap via config-driven backend selection. | Alistair Prime | ACCEPTED | — |
| AD-0015 | 2026-03-23 | Hybrid NLP model source — model-agnostic interface contract (input: raw text, output: structured intent + entities). Claude API as default backend, self-hosted LLM as future fallback. Config-driven backend selection. Design only this sprint, no build. | Alistair Prime | ACCEPTED | — |
| AD-0016 | 2026-03-24 | Platform evaluation (WordPress vs Cloudflare vs Hybrid) — Gate 1 blocker. Full cost/benefit evaluation required before platform lock. | Non Sequitur Web | ACCEPTED | — |
| AD-0017 | 2026-03-24 | GitHub as primary VCS/CI — supersedes GitLab. All repos, CI/CD, and content ops run through GitHub. | ALL | ACCEPTED | AD-0006 |
| AD-0018 | 2026-03-24 | The Markdown as integrated subsystem in non-sequitur-web. Not a separate site — lives within the NSQ ecosystem. | Non Sequitur Web | ACCEPTED | — |
| AD-0019 | 2026-03-24 | Content pipeline: Signal → Vector → Wrap → Drop. Four-stage content operating system. Non-negotiable pipeline contract. | Non Sequitur Web | ACCEPTED | — |
| AD-0020 | 2026-03-24 | Editorial Board: Batch / Passive / Intervention modes. Three concurrent operating modes for content management. | Non Sequitur Web | ACCEPTED | — |
| AD-0021 | 2026-03-24 | Distribution Layer — 3-phase automation. Semi-auto Phase 2, full-auto Phase 3 for social publishing. | Non Sequitur Web | ACCEPTED | — |
| AD-0022 | 2026-03-24 | Agentics as ecosystem lane — commercial/service layer. Revenue architecture for YKS portfolio. | Agentics | ACCEPTED | — |
| AD-0023 | 2026-03-24 | Agentics distribution via LinkedIn + Upwork. Go-to-market strategy for agentics services. | Agentics | ACCEPTED | — |
| AD-0024 | 2026-03-24 | Branch strategy: main + feature/*. Dev workflow locked for all active repos. | ALL | ACCEPTED | — |
| AD-0025 | 2026-03-24 | Content Lab as formal backend subsystem with Git-native persistence. New subsystem within non-sequitur-web. | Non Sequitur Web | ACCEPTED | — |
| AD-0026 | 2026-03-24 | PipelineItem data model (YAML frontmatter + Markdown, stage-as-directory). Flat files, Git is the database. | Non Sequitur Web | ACCEPTED | — |
| AD-0027 | 2026-03-24 | GitHub Actions as workflow engine — 6 workflows (ingest, classify, advance, publish, buffer-check, distribute). | Non Sequitur Web | ACCEPTED | — |
| AD-0028 | 2026-03-24 | Wave + Kanban hybrid UI for The Markdown page. Wave flow primary (reader), Kanban secondary (creator). | Non Sequitur Web | ACCEPTED | — |
| AD-0029 | 2026-03-24 | Comment system deferred — Giscus as interim solution. Custom comments Phase 6+ only if Giscus proves insufficient. | Non Sequitur Web | ACCEPTED | — |
| AD-0030 | 2026-03-24 | Content migration: existing WordPress items → /content/published/ with inferred frontmatter. Migration strategy locked. | Non Sequitur Web | ACCEPTED | — |
| AD-0031 | 2026-03-24 | Content Pipeline Security Controls — sprint-operational. Secrets management, content integrity, input validation. Built during Phase 4/5, not deferred. Routed to Overwatch. | Non Sequitur Web | ACCEPTED | — |

*Reconstructed from session transcripts 2026-03-07 and 2026-03-08. Detail may be incomplete.*
*AD-0011 through AD-0015 added 2026-03-23 — Alistair Prime sprint planning session with Yeti.*
*AD-0016 through AD-0031 added 2026-03-24 — NSQ Content Pipeline architecture session. Greenlit by Yeti. All moved to ACCEPTED.*
