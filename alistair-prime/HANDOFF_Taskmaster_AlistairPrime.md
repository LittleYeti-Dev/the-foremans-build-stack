# HANDOFF: Foreman → Taskmaster
## Subject: Alistair Prime — Sprint Integration into DevOps
## Date: 2026-03-23
## Priority: HIGH — Architecture decisions locked, ready for sprint planning

---

## Taskmaster — Boot Context

You are Taskmaster, the Scrum Master for Yeti Knowledge Systems. You are receiving a handoff from Foreman (Architecture) containing completed architecture work for **Alistair Prime** — the Cognitive Runtime that will serve as the orchestration core for the entire Agentics platform.

Your job: review what was built, validate sprint-readiness, break it into DevOps-actionable work, and integrate it into the active sprint tracker.

---

## What Happened This Session

Foreman conducted a full architecture analysis and codebase audit of Alistair Prime. Five architecture decisions were made with Yeti and locked into the Architecture Register (AD-0011 through AD-0015). The agentics org chart was updated to v2 integrating Prime as the cognitive layer. All artifacts are committed and pushed to `LittleYeti-Dev/the-foremans-build-stack` under `alistair-prime/`.

---

## Repos You Need

| Repo | Access | Contents |
|------|--------|----------|
| `LittleYeti-Dev/the-foremans-build-stack` | Public | Architecture artifacts, org chart, sprint analysis, ADR register |
| `LittleYeti-Dev/alistars-stack` | Private (PAT required) | Implementation code — the actual runtime |

### Key Files in the-foremans-build-stack/alistair-prime/

- `CURRENT_STATE.md` — Original build-shop handoff from Alistair (ChatGPT)
- `Alistair_Prime_Sprint_Analysis.html` — Full sprint breakdown with 18 stories, 3 tracks, 5 phases
- `Agentics_Architecture_OrgChart_v2.html` — Integrated org chart showing Prime's position in the stack
- `Architecture_Register.md` — AD-0001 through AD-0015 (AD-0011–0015 are new, Prime-specific)

### Key Files in alistars-stack/

- `main.py` — Entry point. Boots Config → Logger → SQLiteMemoryStore → Engine
- `src/orchestration/engine.py` — Execution engine. Routes via Request/Response. Has handle(), process(), and built-in command handlers
- `src/orchestration/request.py` — Request/Response dataclasses with RequestKind enum (COMMAND vs INPUT)
- `src/memory/base.py` — Abstract MemoryStore interface (append_event, recent, load_context)
- `src/memory/sqlite_store.py` — SQLite implementation of MemoryStore
- `src/interfaces/cli.py` — Full CLI with REPL, one-shot, JSON output, command routing
- `src/core/config.py` — Config class (env vars: ENV, DEBUG, LOG_LEVEL, MEMORY_DB_PATH)
- `src/core/logging.py` — Structured JSON logger
- `tests/test_engine.py` — Engine routing tests (8 tests)
- `tests/test_cli.py` — CLI integration tests (5 tests)
- `docs/prime_runtime.md` — Runtime documentation
- `docs/decisions/0002-memory-architecture.md` — ADR: local-first, cloud-portable memory
- `MATERIALIZATION_REPORT.md` — Audit confirming all phases are committed

---

## Architecture Decisions (Locked — Do Not Reopen)

These five decisions were made with Yeti. They are accepted. Sprint work must conform to them.

### AD-0011: Unix Socket for Task Submission
- **What:** Local IPC only. No network exposure. No HTTP, no gRPC, no exposed ports.
- **Why:** Smallest attack surface. Prime runs on the same machine/pod as its callers. Outbound HTTPS is used for external services (GitHub, Claude API, Copilot, etc.) — that's the engine layer's job, not the interface layer's.
- **Expansion path:** HTTP REST gets its own ADR if/when a genuine remote-caller use case exists.
- **DevOps impact:** No ingress configuration needed. No TLS cert management. Socket file permissions are the access control.

### AD-0012: Minimal Command Set with Extensible Registry
- **What:** Four commands ship: help, status, memory, exit. Parser is a dictionary registry — new commands are one handler + one registration line.
- **Why:** Commands operate the runtime. Free input is work. Clean separation.
- **Audit finding:** The codebase already has these four commands implemented via if/elif in engine.py. Converting to a dictionary registry is a small refactor.
- **DevOps impact:** No new commands to build from scratch. Refactor only.

### AD-0013: Simple 3-State Task Lifecycle
- **What:** pending → running → done. Success/failure is a result field on the task, not a separate state.
- **Why:** One caller, no concurrency, no queue needed.
- **Expansion path:** `queued` slots before `running`, `cancelled` adds as terminal state. Additive, no breaking changes.
- **DevOps impact:** This is the core new build work. Current Request/Response model needs task ID, state, and result fields added.

### AD-0014: SQLite + PVC for K3s Persistence
- **What:** Keep SQLite as-is. Mount a Persistent Volume Claim on K3s so the DB file survives pod restarts.
- **Why:** Already working. Single writer (socket decision eliminates concurrency). Zero new dependencies.
- **Expansion path:** Memory layer is abstracted behind MemoryStore interface. A postgres_store.py is a clean swap via config.
- **DevOps impact:** PVC manifest needed in K3s config. No code changes for persistence.

### AD-0015: Hybrid NLP Model Source
- **What:** Model-agnostic NLP interface. Claude API default, self-hosted LLM future fallback. Config-driven backend selection.
- **Why:** No vendor lock-in. Interface contract: input = raw text, output = structured intent + entities.
- **DevOps impact:** Design only this sprint. No build. Spec document is the deliverable.

---

## Codebase Audit Findings — What's Actually Built

The architect brief said Interface Layer was "started." The audit found it's **complete**. All five layers are built and working:

| Layer | Status | Key Finding |
|-------|--------|-------------|
| Foundation | DONE | main.py boots cleanly |
| Core | DONE | Config + structured JSON logging wired |
| Orchestration | DONE (scaffold) | Engine routes via Request/Response. process() is placeholder (text.upper()). The routing is real, the processing is scaffold. |
| Memory | DONE | SQLite, abstract interface, events persist across runs |
| Interface | DONE | CLI with REPL, one-shot, JSON, 4 commands, command vs. input classification |
| Connectors | EMPTY | __init__.py placeholder only |
| Security | EMPTY | __init__.py placeholder only |

### What This Means for Sprint Scope

The sprint analysis (HTML doc) proposed 18 stories assuming the Interface Layer wasn't complete. It is. **Revised scope should be:**

1. **Skip B.1.2–B.1.5** (interface completion stories) — already built
2. **AD-0012 refactor** is small — convert if/elif command routing to dictionary registry
3. **AD-0013 task model** is the real build work — add Task object with id, type, payload, state, result to the orchestration layer
4. **Engine refactor** (B.2.3) — replace process() scaffold with task-type-based dispatch
5. **Unix socket interface** (B.3.1 from AD-0011) — new construction
6. **Dockerfile** (B.3.2) — containerize for K3s deployment
7. **Architecture documents** (Docs 1–4) — Foreman produces these, not DevOps

---

## Recommended Sprint Stories for DevOps

### Track 1: Task Model Implementation (Core Build)

| Story | Description | Depends On | Size |
|-------|-------------|------------|------|
| AP-01 | Add Task dataclass to request.py: id (uuid), type (str), payload (dict), state (pending/running/done), result (dict with success bool + data/error) | — | S |
| AP-02 | Refactor engine command routing from if/elif to dictionary registry pattern | — | S |
| AP-03 | Refactor Engine.handle() to create Task from Request, manage state transitions (pending→running→done), log task events to memory | AP-01 | M |
| AP-04 | Replace Engine.process() scaffold with task-type dispatcher — route by task.type to handler functions | AP-03 | M |
| AP-05 | Update test_engine.py — add tests for task lifecycle, state transitions, result fields, registry dispatch | AP-01, AP-03, AP-04 | M |

### Track 2: Unix Socket Interface (New Construction)

| Story | Description | Depends On | Size |
|-------|-------------|------------|------|
| AP-06 | Implement Unix socket listener in src/interfaces/socket.py — accepts JSON task submissions, routes through Engine.handle() | AP-03 | L |
| AP-07 | Socket file permission management — create with 0600, configurable path via SOCKET_PATH env var | AP-06 | S |
| AP-08 | Add socket integration tests — submit task via socket, verify response and memory persistence | AP-06 | M |

### Track 3: Containerization (K3s Readiness)

| Story | Description | Depends On | Size |
|-------|-------------|------------|------|
| AP-09 | Create Dockerfile for Prime runtime — Python slim base, copy src/, expose socket path as volume mount | AP-06 | M |
| AP-10 | Create K3s PVC manifest for SQLite persistence | AP-09 | S |
| AP-11 | Create K3s deployment manifest — pod spec with socket volume + PVC | AP-09, AP-10 | M |
| AP-12 | Verify container runs locally with docker-compose — boot, submit task via socket, confirm memory persistence across restart | AP-09 | M |

### Track 4: Documentation (Foreman Delivers, DevOps Reviews)

| Story | Description | Owner | Size |
|-------|-------------|-------|------|
| AP-13 | System Architecture document (Doc 1) | Foreman | L |
| AP-14 | Interface Layer Specification (Doc 2) | Foreman | M |
| AP-15 | Task Model & Routing Specification (Doc 3) | Foreman | M |
| AP-16 | NLP Layer Placement & Future Integration Plan (Doc 4) | Foreman | M |

---

## Sequencing

```
Phase 1 (Days 1–3):  AP-01, AP-02 in parallel → then AP-03 → AP-04 → AP-05
Phase 2 (Days 3–6):  AP-06, AP-07 → AP-08
Phase 3 (Days 5–8):  AP-09, AP-10, AP-11 → AP-12
Docs (parallel):     AP-13 through AP-16 (Foreman, not DevOps)
```

Phases 1 and 2 can overlap starting day 3. Phase 3 can start as soon as AP-06 is done.

---

## Risks to Track

| Risk | Impact | Mitigation |
|------|--------|------------|
| K3s not operational on Robo Stack | Blocks AP-10, AP-11 deployment | Design manifests now, deploy when K3s is live. Container runs locally via docker-compose. |
| Engine refactor scope creep | Could balloon if process() replacement tries to do too much | AP-04 only implements task-type dispatch. Actual task handlers (GitHub, Claude, etc.) are future sprint work. |
| Socket security model insufficient | Unix permissions may not be granular enough for multi-service pod | Acceptable for now — solo operator. Revisit when multi-caller scenario materializes. |

---

## Integration with Sprint 6 (Agentics MVP)

Alistair Prime is a **parallel track** to Sprint 6. No cross-dependencies today. The future convergence point is AP-06 (socket interface) — when tower agents can submit tasks to Prime. That's not in scope for this sprint but the socket interface is being built with that use case in mind.

---

## What I Need from You, Taskmaster

1. **Validate the 12 DevOps stories** (AP-01 through AP-12) against your sprint capacity
2. **Sequence against existing Sprint 6 work** — is there contention on the Builder or Engineer towers?
3. **Flag any stories that need decomposition** — I sized them from the architecture side, you may see different complexity from the build side
4. **Confirm the dependency chain** — does the sequencing work for your sprint rhythm?
5. **Integrate into the sprint tracker** — these stories need to be tracked alongside Sprint 6

---

## Files Delivered

All artifacts are in `LittleYeti-Dev/the-foremans-build-stack/alistair-prime/`:

```
alistair-prime/
├── CURRENT_STATE.md                          ← original handoff from Alistair
├── Alistair_Prime_Sprint_Analysis.html       ← full sprint breakdown (18 stories, pre-audit)
├── Agentics_Architecture_OrgChart_v2.html    ← integrated org chart + architecture
├── Architecture_Register.md                  ← AD-0001 through AD-0015
└── HANDOFF_Taskmaster_AlistairPrime.md       ← THIS DOCUMENT
```

Implementation code is in `LittleYeti-Dev/alistars-stack` (private, PAT required).

---

*Handoff from Foreman to Taskmaster — 2026-03-23*
*"The mission defines the architecture. The architecture secures the mission."*
