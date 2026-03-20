# The Foreman's Build Stack

**Agentics Framework — Architecture, Project Registry, and Control Plane**

This is the operating system for the Non Sequitur / YKS manufacturing floor. It contains the methodology, templates, and architecture artifacts that make the Agentics distributed AI workforce repeatable and scalable.

## What Is This?

Agentics is a technology stack for building digital products using a distributed workforce of human and AI agents. Each AI agent runs on a separate platform (Claude, ChatGPT, Windsurf, self-hosted LLMs) with its own Google account, context boundary, and specialization. A human orchestrator routes work between agents, bridges context, and makes judgment calls.

This repo is where the blueprints live.

## Repo Structure

```
the-foremans-build-stack/
├── framework/                    # The Agentics methodology (product-agnostic)
│   ├── agent-templates/          # Role definitions for each tower
│   ├── handoff-protocols/        # How agents pass work between each other
│   └── context-packages/         # Boot context for spinning up new agent sessions
├── projects/                     # Project registry (one folder per active project)
│   ├── _template/                # Blank project template — copy for new projects
│   └── the-markdown/             # First project: The Markdown content platform
│       ├── architecture/         # System architecture docs
│       ├── gap-register/         # What's missing vs. what's built
│       └── design/               # Design specs for new features
├── control-plane/                # CI/CD, linting, automation for THIS repo
└── phase-0-artifacts/            # Initial architecture audit (March 2026)
```

## Architectural Principles

1. **Human = Direction, AI = Execution** — The orchestrator defines intent, routes work, and makes final calls. AI agents execute within bounded contexts.

2. **Isolated Context Boundaries** — Each AI agent runs under a separate account with a separate learning trajectory. No cross-contamination between roles.

3. **Architecturally Agnostic** — No vendor lock-in. The framework supports commercial AI (Claude, ChatGPT) and self-hosted open-source models (on Robo Stack K3s) interchangeably. Towers can swap their AI platform without rebuilding workflows.

4. **Repeatable Manufacturing** — The same framework that builds internal products (The Markdown) can build client products. A new project = a new folder in `projects/` + agent assignments from the tower roster.

5. **Mentorship-Ready** — Every tower is a learning station. The structure is designed so a junior team member can rotate through Architecture, Building, Editorial, Creative, and Engineering alongside AI agents.

## Related Repos

| Repo | Role |
|------|------|
| [the-markdown](https://github.com/LittleYeti-Dev/the-markdown) | First product — content curation platform |
| [robo-stack](https://github.com/LittleYeti-Dev/robo-stack) | Shared infrastructure — AWS, K3s, CI/CD, self-hosted LLMs |
| [content-ops](https://github.com/LittleYeti-Dev/content-ops) | Content Refinery Platform (Sprint 0) |

## Current Status

Phase 0 architecture complete. Org chart, gap register (25 items), and system maps produced. Ready for Phase 1: framework formalization and project standup flow.

---

*Non Sequitur / Agentics / ag3ntics / Yeti Knowledge Systems*
