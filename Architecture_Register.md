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

*Reconstructed from session transcripts 2026-03-07 and 2026-03-08. Detail may be incomplete.*
*AD-0011 through AD-0015 added 2026-03-23 — Alistair Prime sprint planning session with Yeti.*
