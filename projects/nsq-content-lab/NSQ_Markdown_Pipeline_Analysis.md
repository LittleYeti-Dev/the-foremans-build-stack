# NSQ MARKDOWN PIPELINE — FOREMAN ANALYSIS

**Document:** Architecture Analysis & Design Response
**Date:** 2026-03-24
**Artifact ID:** NSQ-MD-ANALYSIS-001
**Responding to:** NSQ-MD-REQ-001 (nsq-markdown-pipeline.md)
**Prepared by:** Foreman
**For:** Yeti (Justin Kuiper)

---

## EXECUTIVE SUMMARY

The requirements document formalizes a **content operating system** — not a blog redesign. This analysis covers the four deliverables requested: data model, backend recommendation, UI direction, and constraints/gaps. The core finding: the pipeline model is architecturally sound but requires careful scoping to avoid building a CMS from scratch. My recommendation is a Git-native content pipeline backed by GitHub (not GitLab — per AD-0017), with a hybrid wave + Kanban UI that maps directly to the four pipeline stages.

---

## DELIVERABLE 1: DATA MODEL FOR PIPELINE OBJECTS

### 1.1 Core Entity: PipelineItem

Every piece of content in the system is a `PipelineItem`. This is the atomic unit.

```
PipelineItem
├── id              : string (UUID v4)
├── title           : string (required)
├── slug            : string (auto-generated from title, editable)
├── stage           : enum [SIGNAL, VECTOR, WRAPPER, WAVE_DROP]
├── status          : enum [ACTIVE, PAUSED, ARCHIVED]
├── created_at      : datetime (ISO 8601)
├── updated_at      : datetime (ISO 8601)
├── publish_date    : datetime | null (scheduled publish, null = unscheduled)
├── published_at    : datetime | null (actual publish timestamp)
├── category        : enum [AI, CYBER, INNOVATION, FAITH_NEURO, SPACE_AERO, DIGITAL_LIFE]
├── tags            : string[] (freeform)
├── source          : SourceRef (see below)
├── content         : StageContent (see below)
├── outputs         : OutputRef[] (see below)
├── cross_links     : CrossLink[] (see below)
├── decision_gate   : GateDecision | null (see below)
├── thread_id       : string | null (groups related items into a thread)
├── editorial_notes : string | null (Yeti Take / commentary)
└── meta            : object (extensible key-value for future fields)
```

### 1.2 Supporting Types

**SourceRef** — Where the signal originated:
```
SourceRef
├── type        : enum [RSS, MANUAL, SOCIAL, BUILD_LOG, EXTERNAL]
├── url         : string | null
├── name        : string (e.g., "Ars Technica", "manual note")
├── captured_at : datetime
└── raw_content : string | null (original text/snippet)
```

**StageContent** — Content evolves per stage. Each stage appends; nothing is overwritten:
```
StageContent
├── signal    : { detection: string, commentary: string | null, framing: string | null }
├── vector    : { definition: string | null, validation: string | null, direction: string | null } | null
├── wrapper   : { analogies: string | null, cross_domains: string[], synthesis: string | null } | null
└── wave_drop : { synthesis: string | null, application: string | null, deployment: string | null } | null
```

**OutputRef** — Content outputs generated at each stage:
```
OutputRef
├── type        : enum [TWEET, BLOG_SEED, CONCEPT_DEF, DIAGRAM, TEASER, BLOG, WHITE_PAPER, VLOG, PRODUCT, SPEAKING]
├── stage       : enum [SIGNAL, VECTOR, WRAPPER, WAVE_DROP]
├── content     : string | reference (path to file if large)
├── created_at  : datetime
├── published   : boolean
└── platform    : string | null (where it was published)
```

**CrossLink** — Connections to the broader NSQ ecosystem:
```
CrossLink
├── type    : enum [BLOG_POST, VIDEO, BOOK, EXTERNAL, ARCHITECTURE, BUILD]
├── url     : string
├── title   : string
└── context : string | null (why this link is relevant)
```

**GateDecision** — Creator decision at each stage transition:
```
GateDecision
├── action    : enum [ADVANCE, HOLD, ARCHIVE, PUBLISH_PARTIAL, EXPAND_SERIES, KEEP_PRIVATE]
├── decided_at: datetime
├── notes     : string | null
└── stage_from: enum [SIGNAL, VECTOR, WRAPPER, WAVE_DROP]
```

### 1.3 Thread Model

Items can be grouped into **threads** via `thread_id`. A thread represents a persistent line of inquiry that accumulates signals over time.

```
Thread
├── id          : string (UUID v4)
├── title       : string
├── description : string
├── created_at  : datetime
├── status      : enum [ACTIVE, DORMANT, COMPLETED]
├── item_count  : int (computed)
└── categories  : string[] (computed from member items)
```

### 1.4 File Representation (Git-Native)

In the repo, each PipelineItem is a Markdown file with YAML frontmatter:

```yaml
---
id: "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
title: "NVIDIA's Agent Architecture Shift"
slug: "nvidia-agent-architecture-shift"
stage: "vector"
status: "active"
created_at: "2026-03-24T14:00:00Z"
updated_at: "2026-03-24T16:30:00Z"
publish_date: null
category: "ai"
tags: ["nvidia", "agent-systems", "inference", "architecture"]
source:
  type: "rss"
  url: "https://example.com/nvidia-article"
  name: "Ars Technica"
  captured_at: "2026-03-24T14:00:00Z"
thread_id: "gpu-compute-evolution"
decision_gate:
  action: "advance"
  decided_at: "2026-03-24T16:30:00Z"
  stage_from: "signal"
outputs:
  - type: "tweet"
    stage: "vector"
    published: false
  - type: "concept_def"
    stage: "vector"
    published: false
cross_links:
  - type: "blog_post"
    url: "/the-markdown/agentic-compute-paradigm"
    title: "The Agentic Compute Paradigm"
---

## Signal
**Detection:** NVIDIA announces inference-time compute scaling for agent workloads...
**Commentary:** This confirms the architectural split we've been tracking...
**Framing:** GPU → Agent Runtime, not just GPU → Training

## Vector
**Definition:** Inference-time compute scaling — allocating GPU resources dynamically during agent execution rather than pre-training.
**Validation:** Confirmed by Jensen's GTC keynote. Real deployments at scale.
**Direction:** This reshapes the build stack for anyone running agent systems.

## Yeti Take
The training-centric GPU thesis is dead. The new thesis is runtime. If you're building agents, your architecture just changed.
```

### 1.5 Directory Structure

```
/content/
├── pipeline/
│   ├── signal/           ← Stage 1: raw captures
│   ├── vector/           ← Stage 2: defined + validated
│   ├── wrapper/          ← Stage 3: cross-domain synthesis
│   └── wave-drop/        ← Stage 4: final outputs
├── queue/                ← Ready-to-publish items (publish_date set)
├── published/            ← Published items (archived from queue)
├── threads/              ← Thread index files
└── archive/              ← Paused/archived items
```

Items move between directories as they advance stages. Git history tracks all transitions.

### 1.6 Design Rationale

Why this model:

- **Flat files, not database.** Every item is a Markdown file with frontmatter. Git is the database. No external persistence needed. Matches AD-0017 (GitHub as truth) and AD-0019 (content pipeline contract).
- **Stage-as-directory.** Physical file location = pipeline stage. No query layer needed to know what's where. `ls content/pipeline/vector/` shows you every item in Vector stage.
- **Append-only content.** Stage content accumulates — Signal doesn't get deleted when Vector is added. The full evolution is always visible. This directly supports the wave visualization (you can see where an idea started and where it is now).
- **Outputs are first-class.** Every stage can produce publishable outputs. A tweet from Vector stage and a white paper from Wave Drop are both tracked as outputs of the same PipelineItem.
- **Threads are lightweight.** Just a shared `thread_id` field. No complex relational model. Thread metadata lives in `/content/threads/` as index files.
- **Extensible via `meta`.** Future fields don't require schema migration. Add to `meta` first, promote to top-level when proven.

---

## DELIVERABLE 2: BACKEND IMPLEMENTATION RECOMMENDATION

### 2.1 Recommendation: GitHub-Native Content Ops

**Not GitLab.** AD-0017 locked GitHub as VCS/CI platform. The requirements doc mentions "GitLab or Git-style content ops" — the answer is GitHub with GitHub Actions as the workflow engine.

### 2.2 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    CONTENT LAB (Backend)                     │
├──────────────┬──────────────┬───────────────┬───────────────┤
│  INGEST      │  PROCESS     │  MANAGE       │  PUBLISH      │
│              │              │               │               │
│  ingest.yml  │  classify.py │  Pipeline CLI │  publish.yml  │
│  ingest.py   │  Claude API  │  (future)     │  publish.py   │
│              │              │               │  distribute.py│
│  RSS feeds   │  Scoring     │  Stage moves  │               │
│  Manual      │  Tagging     │  Gate checks  │  Site build   │
│  Social      │  Vectoring   │  Buffer health│  Social dist  │
└──────┬───────┴──────┬───────┴───────┬───────┴───────┬───────┘
       │              │               │               │
       ▼              ▼               ▼               ▼
┌─────────────────────────────────────────────────────────────┐
│                  GitHub Repository (Truth)                    │
│  /content/pipeline/{signal,vector,wrapper,wave-drop}/        │
│  /content/queue/                                             │
│  /content/published/                                         │
│  /.github/workflows/                                         │
│  /scripts/                                                   │
└─────────────────────────────────────────────────────────────┘
       │
       ▼
┌─────────────────────────────────────────────────────────────┐
│                  MARKDOWN PAGE (Frontend)                     │
│  Reads from /content/ at build time                          │
│  Static site generation (SSG) → deploy                       │
└─────────────────────────────────────────────────────────────┘
```

### 2.3 Workflow Engine: GitHub Actions

| Workflow | Trigger | Function |
|----------|---------|----------|
| `ingest.yml` | Scheduled (every 6 hours) | RSS fetch → normalize → create Signal files → commit |
| `classify.yml` | On push to `content/pipeline/signal/` | Claude API scoring + tagging → update frontmatter → commit |
| `advance.yml` | Manual dispatch or scheduled | Check items with gate decisions → move between stage directories |
| `publish.yml` | Scheduled (daily 0800 ET) | Scan queue for items with reached publish_date → move to published → trigger site build |
| `buffer-check.yml` | Scheduled (weekly Monday) | Count queue items by category, alert if below 2-week threshold |
| `distribute.yml` | On publish completion | Transform published content per platform template → post to social APIs |
| `digest.yml` | Scheduled (daily) | Generate pipeline state summary → commit to /reports/ |

### 2.4 Kanban Mapping

The requirements ask for Kanban-style movement. Git directories ARE the Kanban board:

| Kanban Column | Directory | Transition Trigger |
|---------------|-----------|-------------------|
| Backlog | `/content/pipeline/signal/` | `ingest.yml` creates file |
| In Progress | `/content/pipeline/vector/` | Gate decision: ADVANCE from Signal |
| Review | `/content/pipeline/wrapper/` | Gate decision: ADVANCE from Vector |
| Done / Ready | `/content/pipeline/wave-drop/` | Gate decision: ADVANCE from Wrapper |
| Scheduled | `/content/queue/` | `publish_date` set on Wave Drop item |
| Published | `/content/published/` | `publish.yml` moves on publish_date |
| Archive | `/content/archive/` | Gate decision: ARCHIVE at any stage |

Stage transitions are Git operations: `git mv content/pipeline/signal/item.md content/pipeline/vector/item.md` + commit. Full audit trail in Git history.

### 2.5 Creator Interaction Model

For the decision gates (Advance / Hold / Archive / etc.), two modes:

**Mode 1 — Batch (Active Session):**
Yeti works through items directly. Could be via:
- Direct file editing in VS Code / GitHub web editor
- Future: CLI tool (`nsq advance item-slug --to vector --notes "Validated, real signal"`)
- Future: Web dashboard with buttons (Phase 6+ scope)

**Mode 2 — Passive (Autonomous):**
GitHub Actions handles ingestion, classification, and queue management. Items with AI confidence score above threshold auto-advance from Signal to Vector. Items below threshold hold for manual review.

**Mode 3 — Intervention (Manual Override):**
Manual commit: edit frontmatter `stage` field, move file, push. System respects manual overrides — no automation will revert a manual decision.

### 2.6 Why Not a Database?

The requirements suggest Kanban-style tracking with tags and phases. A database (Postgres, D1, etc.) could do this. But:

- **Git gives you versioning for free.** Every stage transition, every edit, every decision is a commit. No audit log to build.
- **GitHub is already the truth.** Per AD-0017. Adding a database creates split-brain — which is authoritative?
- **Solo-operator.** One system to maintain, not two. No database migrations, no backup strategy, no connection strings.
- **Content-as-code.** The entire pipeline state is visible by browsing the repo. `ls content/pipeline/vector/` answers "what's in Vector?" No query language needed.
- **CI/CD native.** GitHub Actions can read/write files and commit. Adding a database means every action needs a database client.

**Trade-off accepted:** Git is not optimized for high-frequency writes. At 32+ items/day ingest, this is fine — Git handles thousands of files without issue. If volume scales to thousands per day, revisit with a database layer. Add that as a revisit trigger.

### 2.7 Comment System Approach

The requirements specify open comments with moderation. **Recommendation: defer to Phase 6+.**

Rationale: Comments are a social feature. The pipeline is a content operations system. Building comments into the pipeline architecture couples two different concerns. Instead:

- **Phase 1 (now):** No comments. The Markdown is a publishing surface, not a forum.
- **Phase 2 (post-launch):** Add Giscus (GitHub Discussions-backed comments). Free, moderable, GitHub-native. Aligns with AD-0017.
- **Phase 3 (if needed):** Custom comment system with tiered access. Only build if Giscus proves insufficient.

This keeps the pipeline clean and the comment system as an additive layer, not a structural dependency.

---

## DELIVERABLE 3: UI DIRECTION — WAVE + KANBAN HYBRID

### 3.1 Design Concept

The Markdown page has two visual modes that coexist:

**Primary: Wave Flow (Reader Experience)**
Left-to-right horizontal visualization showing ideas progressing through pipeline stages. This is the public-facing, narrative view. Readers see the journey of ideas from raw signal to finished output.

**Secondary: Kanban Grid (Creator/Power User)**
Traditional vertical columns showing pipeline stage counts and item lists. Toggle-accessible. This is the operational view for Yeti and anyone who wants to see pipeline state at a glance.

### 3.2 Wave Flow Layout

```
┌─────────────────────────────────────────────────────────────────────┐
│  THE MARKDOWN                                                       │
│  "It does not follow… or does it?"                                 │
│  [Status Bar: 32 ingested | 6 vectors | Next refresh: 0900 ET]     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐     │
│  │  SIGNAL   │───▶│  VECTOR  │───▶│ WRAPPER  │───▶│WAVE DROP │     │
│  │           │    │          │    │          │    │          │     │
│  │ ░░░░░░░░ │    │ ░░░░░░░░ │    │ ░░░░░░░░ │    │ ░░░░░░░░ │     │
│  │ ░░░░░░░░ │    │ ░░░░░░░░ │    │ ░░░░░░░░ │    │ ░░░░░░░░ │     │
│  │ ░░░░░░░░ │    │          │    │          │    │          │     │
│  │  [24]     │    │  [8]     │    │  [3]     │    │  [2]     │     │
│  └──────────┘    └──────────┘    └──────────┘    └──────────┘     │
│                                                                     │
│  ═══════════════════ WAVE THREADS ════════════════════════════      │
│                                                                     │
│  ─── GPU Compute Evolution ────────────────────────▶ [WRAPPER]     │
│  ─── Zero Trust Architecture ──────▶ [VECTOR]                      │
│  ─── Quantum Risk Timeline ─────────────────────────────▶ [DROP]   │
│  ─── Faith & Neural Mapping ──▶ [SIGNAL]                           │
│                                                                     │
│  Each thread is a horizontal line. Color = category.                │
│  Line opacity/width indicates activity level.                       │
│  Endpoints show current stage position.                             │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│  TODAY'S SIGNAL (cards, same as current mockup)                     │
├─────────────────────────────────────────────────────────────────────┤
│  BY VECTOR (6-column grid, same as current mockup)                  │
├─────────────────────────────────────────────────────────────────────┤
│  ACTIVE THREADS (card grid, same as current mockup)                 │
├─────────────────────────────────────────────────────────────────────┤
│  EDITOR'S PICKS / DEEP SIGNAL (same as current mockup)             │
├─────────────────────────────────────────────────────────────────────┤
│  ARCHIVE (filter + search, lighter treatment)                       │
└─────────────────────────────────────────────────────────────────────┘
```

### 3.3 Wave Visualization Spec

The wave section is a **horizontal swim lane view** positioned below the stage summary and above the existing signal grid:

**Per-Thread Wave:**
- Each thread renders as a horizontal line spanning from its origin stage to its current stage
- Line color = primary category color (Electric Blue for AI, Cyan for Cyber, etc.)
- Line width: 3px active, 2px in-progress, 1px paused
- Line style: solid = active, dashed = paused
- Opacity: 1.0 active, 0.6 in-progress, 0.3 paused/stale
- Endpoint: circle marker at current stage position, pulsing if active
- Hover: expands to show thread title + item count + latest item headline

**Stage Columns (background):**
- Four columns with subtle vertical dividers
- Column headers: SIGNAL | VECTOR | WRAPPER | WAVE DROP
- Count badges showing items per stage
- Column background: very subtle gradient (dark → slightly lighter left to right, conveying progression)

**Waveform Connector:**
- SVG path connecting stage columns with a gentle sine wave curve
- Threads don't travel in straight lines — they follow gentle wave arcs
- Multiple threads layer vertically, spaced 24px apart
- When threads cross stages, the wave amplitude increases slightly (visual energy)

**State Visualization (per requirements):**

| State | Visual Cue |
|-------|------------|
| Active | Full opacity, animated pulse on endpoint, bright category color |
| In-progress | Solid gradient, steady glow, slightly muted |
| Paused | Wave tapers — line becomes dashed, opacity drops to 0.3, gray-shift |
| Archived | Not shown in wave section. Lives in archive below with lighter styling. |

### 3.4 Color Arc System

Each of the six content categories has a defined color (from the locked design system):

| Category | Color | Hex |
|----------|-------|-----|
| AI / GenAI | Electric Blue | #2F6BFF |
| Cyber | Cyan | #00A8B5 |
| Innovation | Amber | #F5A623 |
| Faith / Neuro | Violet | #7A5CFF |
| Space / Aero | Orange-Red | #E85D3A |
| Digital Life | Green | #4CAF50 |

**Gradient progression:** As an item moves through stages, its color treatment shifts:
- Signal: color at 40% opacity (faint — just captured)
- Vector: color at 60% opacity (taking shape)
- Wrapper: color at 80% opacity (synthesizing)
- Wave Drop: color at 100% opacity + subtle glow (fully realized)

This creates a natural visual gradient where the left side of the wave section is dimmer and the right side is brighter — ideas literally brighten as they mature.

### 3.5 Mobile Adaptation

On screens < 768px:
- Wave section collapses to a **vertical timeline** (top = Signal, bottom = Wave Drop)
- Thread lines become vertical
- Stage summary becomes a horizontal scrollable strip
- Everything else (signal grid, vector columns, etc.) stacks vertically per existing mockup responsive behavior

### 3.6 Kanban Toggle (Creator View)

A toggle in the status bar switches to **Kanban mode**:

```
┌──────────┬──────────┬──────────┬──────────┬──────────┐
│ SIGNAL   │ VECTOR   │ WRAPPER  │ WAVE DROP│ QUEUE    │
│ (24)     │ (8)      │ (3)      │ (2)      │ (5)      │
├──────────┼──────────┼──────────┼──────────┼──────────┤
│ ┌──────┐ │ ┌──────┐ │ ┌──────┐ │ ┌──────┐ │ ┌──────┐ │
│ │ Card │ │ │ Card │ │ │ Card │ │ │ Card │ │ │ Card │ │
│ └──────┘ │ └──────┘ │ └──────┘ │ └──────┘ │ └──────┘ │
│ ┌──────┐ │ ┌──────┐ │ ┌──────┐ │ ┌──────┐ │ ┌──────┐ │
│ │ Card │ │ │ Card │ │ │ Card │ │ │ Card │ │ │ Card │ │
│ └──────┘ │ └──────┘ │ └──────┘ │ └──────┘ │ └──────┘ │
│ ...      │ ...      │          │          │ ...      │
└──────────┴──────────┴──────────┴──────────┴──────────┘
```

Cards in Kanban mode show: title, category tag, source, age, and gate action buttons (Advance / Hold / Archive). This is the operational dashboard for editorial sessions.

**Note:** Kanban mode is Phase 6+ scope — the wave flow is the primary MVP visual. Kanban is additive.

---

## DELIVERABLE 4: CONSTRAINTS AND GAPS

### 4.1 Hard Constraints

| # | Constraint | Impact | Source |
|---|-----------|--------|--------|
| C-1 | GitHub is VCS/CI platform, not GitLab | Backend must be GitHub Actions, not GitLab CI | AD-0017 |
| C-2 | Platform decision (TD-001) is unresolved | Frontend implementation tech is unknown — wave visualization must be platform-agnostic | AD-0016 |
| C-3 | Solo-operator | No team to maintain complex infrastructure. Every system must be debuggable by one person. | Standing order |
| C-4 | Content pipeline is Signal → Vector → Wrapper → Wave Drop | Non-negotiable. Locked by Gunther handover. | TD-008 |
| C-5 | 3–7 day autonomous operation | System must run without human input. Batch mode is supplementary, not required. | Locked requirement |
| C-6 | Design system is locked | Colors, typography, tone — no deviations. | TD-009 |
| C-7 | 2–4 week content buffer | Buffer health is a system requirement, not a nice-to-have. | Locked requirement |

### 4.2 Identified Gaps

| # | Gap | Severity | Resolution Path |
|---|-----|----------|----------------|
| G-1 | **No RSS feed list defined.** Requirements say "RSS feeds across multiple domains" but don't specify which feeds, how many, or what categories they map to. Current WP RSS Aggregator has feeds — need to export that list. | HIGH | Extract current RSS feed list from WordPress admin. Document feed → category mapping. |
| G-2 | **AI classification confidence threshold undefined.** The pipeline assumes AI can auto-advance items from Signal → Vector. What confidence score triggers auto-advance vs. hold-for-review? | HIGH | Define threshold during Phase 4. Recommend starting conservative (manual review for all) and loosening as AI classification proves reliable. |
| G-3 | **Wave visualization is custom engineering.** No off-the-shelf component renders horizontal multi-thread wave flows with stage progression. This is bespoke SVG/Canvas work. | HIGH | Budget 3–5 days of frontend engineering specifically for the wave component. Use D3.js or raw SVG. Do not attempt with CSS alone. |
| G-4 | **Comment system requirements are premature.** Moderated comments with tiered access is a social platform feature. Building this alongside a content pipeline is scope creep. | MEDIUM | Defer to Phase 6+. Use Giscus (GitHub Discussions) as interim. See Deliverable 2 recommendation. |
| G-5 | **Cross-domain requirement in Wrapper is vague.** "Each item in Wrapper should connect to at least one additional domain" — who validates this? Is it enforced by the system or by editorial judgment? | MEDIUM | Make it an editorial guideline, not a system enforcement. Add a `cross_domains` frontmatter field (string array) but don't block stage transition on it. |
| G-6 | **No content migration path defined.** 2,000+ existing feed items need to become PipelineItems. What stage do they enter at? What metadata do they get? | HIGH | Define a migration script that maps existing WordPress posts to PipelineItems at `published` stage (they've already completed the pipeline). Existing items go to `/content/published/` with inferred frontmatter. |
| G-7 | **Output delivery mechanism undefined.** The model tracks outputs (tweets, blogs, white papers) but doesn't specify how they get published. Is the output just metadata on the PipelineItem, or does it become its own file? | MEDIUM | Outputs ≤ 280 chars (tweets, teasers) live inline in the PipelineItem frontmatter. Outputs > 280 chars (blogs, white papers) become their own files in `/content/published/` with a `parent_id` reference back to the originating PipelineItem. |
| G-8 | **Vlog production pipeline is out of scope.** Requirements list vlogs as a Wave Drop output. Video production, hosting (YouTube), and distribution are an entirely different pipeline. | LOW | Acknowledge vlog as an output TYPE but defer the production pipeline. PipelineItem can reference a YouTube URL as an OutputRef. The video itself is produced outside this system. |
| G-9 | **"Products" as an output is undefined.** Wave Drop outputs include "products." What does this mean? Digital products? Templates? Code? | LOW | Treat as a future output type. The `meta` field on OutputRef handles this extensibility. Don't design for it now. |
| G-10 | **No authentication/authorization model.** Who can advance items? Just Yeti? Future contributors? | LOW (for now) | Solo-operator = Yeti has full access. GitHub repo permissions are the auth model. If contributors are added later, GitHub branch protection + PR reviews become the gate. |

### 4.3 Architectural Risks

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| Wave visualization scope creep | HIGH | HIGH | Define MVP: static wave rendering with 5–10 active threads. No animation until V2. |
| Git repo bloat from high-volume ingest | MEDIUM | LOW | At 32 items/day, ~12K files/year. Git handles this fine. Monitor repo size quarterly. |
| Claude API rate limits during classification | MEDIUM | MEDIUM | Batch classification runs. Queue items and process in batches of 10–20. Add retry with backoff. |
| Pipeline becomes too rigid for creative work | MEDIUM | MEDIUM | Decision gates include "Keep Private" and "Hold" — Yeti can park items indefinitely. The pipeline suggests, not enforces. |
| Comment system becomes a moderation burden | MEDIUM | HIGH | That's why it's deferred. Giscus first. Custom later. Only if proven necessary. |

### 4.4 Reconciliation with Existing Architecture

The requirements doc creates several conflicts with standing decisions that need explicit resolution:

| Requirement | Existing Decision | Conflict | Resolution |
|-------------|------------------|----------|------------|
| "GitLab or Git-style content ops" | AD-0017: GitHub as VCS | Minor — intent is Git-style, not specifically GitLab | Use GitHub. Requirements satisfied. |
| Detailed stage content model | AD-0019: Signal → Vector → Wrap → Drop | Aligned but expanded. AD-0019 defined stages; requirements add subsections and outputs per stage. | Update AD-0019 to reference this analysis for stage detail. |
| Comment system | Not in any existing AD | New requirement. No conflict. | New ADR needed when comment system is designed (Phase 6+). |
| Wave visualization | Existing mockup (nsq_the_markdown.html) uses card grids, not waves | Requirements supersede mockup for The Markdown page. Mockup sections (Today's Signal, By Vector, etc.) remain valid below the wave. | The wave section is NEW — inserted between masthead and existing signal grid. Mockup is not invalidated, it's augmented. |
| Content Lab as backend | Not in any existing AD | New subsystem. Needs its own ADR. | Propose AD-0025: Content Lab as backend subsystem. |

---

## PROPOSED NEW ARCHITECTURE DECISIONS

Based on this analysis, the following ADRs should be created:

| ID | Title | Status |
|----|-------|--------|
| AD-0025 | Content Lab as formal backend subsystem with Git-native persistence | PROPOSED |
| AD-0026 | PipelineItem data model (frontmatter + Markdown, stage-as-directory) | PROPOSED |
| AD-0027 | GitHub Actions as workflow engine for ingest, classify, publish, distribute | PROPOSED |
| AD-0028 | Wave + Kanban hybrid UI for The Markdown page | PROPOSED |
| AD-0029 | Comment system deferred — Giscus as interim solution | PROPOSED |
| AD-0030 | Content migration: existing WordPress items → /content/published/ with inferred frontmatter | PROPOSED |

---

## TAGGING (Per Requirements Doc Section 19)

- **Signal:** Handover email received, requirements doc analyzed
- **Wrap:** Data model designed, backend recommended, UI direction proposed, constraints mapped
- **Analysis:** Full pipeline, UX, and operating breakdown complete
- **Iteration:** Ready for Yeti review and refinement
- **Implementation:** Pending Yeti approval → sprint decomposition by Taskmaster

---

## NEXT STEPS

1. **Yeti reviews this analysis.** Confirm, challenge, or redirect any of the four deliverables.
2. **Resolve G-1 (RSS feed list).** I need the current feed list from WordPress to design the ingest layer properly.
3. **AD-0025 through AD-0030 — accept or modify.** These need to land in the Architecture Register.
4. **Hand to Taskmaster for sprint decomposition** once Yeti approves the approach.
5. **Prototype the wave visualization.** This is the highest-risk UI component. A standalone HTML prototype should be built before full page integration.

---

---

## ADDENDUM A: CONTENT PIPELINE OPERATING PLAN

A formal operating plan has been produced as a standalone deliverable:

**Document:** `NSQ_Content_Pipeline_Operating_Plan.docx` (NSQ-OPPLAN-001)
**Location:** Build Stack Workshop/

This document covers:

1. **Concept of Operations** — Three operating modes (Passive, Batch, Intervention) defined with clear boundaries. Default state is Passive with periodic Batch sessions.

2. **Daily Operations Cycle** — Eight automated workflow runs per day (four ingest cycles, classification, digest, publish, distribute). All scheduled via GitHub Actions. Zero human touch required.

3. **Batch Session Protocol** — Structured editorial work block (80–150 min). Seven tasks from digest review through buffer check. Recommended 2–3 sessions per week.

4. **Weekly Operating Rhythm** — Day-by-day cadence. Total human time: 2.5–4.5 hours per week, decreasing as automation matures.

5. **Automation Target Map** — Every pipeline operation mapped to current and target automation level, implementation mechanism, output, and phase. Clear distinction between what gets automated (volume work) and what stays manual (judgment work).

6. **Failure Modes and Responses** — Eight failure scenarios with defined responses and human intervention triggers. The 2–4 week buffer is the shock absorber — any single component can fail for days without visible impact.

7. **Security Operations Cadence** — Per AD-0031. Eight controls with frequency, procedure, and owner. Monthly through quarterly cadence. Overwatch reviews before automation goes live.

8. **Pipeline Health Metrics** — Eight KPIs: buffer depth, ingest rate, advancement rate, publication rate, distribution reach, classification accuracy, throughput time, thread activity. Yellow and red thresholds defined.

9. **Revision Triggers** — Seven conditions that trigger operating plan update (platform decision, scale change, operator addition, automation maturity, security findings).

---

## ADDENDUM B: REPO STRATEGY CLARIFICATION

Per Yeti directive (2026-03-24): The Content Lab and pipeline system live inside `non-sequitur-web`, not a separate repo. This is consistent with AD-0017 (GitHub as umbrella monorepo) and AD-0018 (The Markdown as integrated subsystem). TD-002 resolved: fold `the-markdown` into `non-sequitur-web`, archive GitLab repo.

---

## ADDENDUM C: SECURITY CONTROLS — SPRINT-OPERATIONAL

Per Yeti directive (2026-03-24): AD-0031 (Content Pipeline Security Controls) is sprint-operational — built during Phase 4/5, not deferred. Three layers:

1. **Secrets Management:** GitHub Secrets for all API keys. No secrets in code/logs/frontmatter. 90-day rotation (social tokens), 180-day rotation (Claude API).
2. **Content Integrity:** Repo private. Branch protection on main. GPG-signed commits. PR-only merges.
3. **Pipeline Input Validation:** RSS input sanitization. Allowlisted feed URLs. Claude API sandboxing (structured output only). Confidence threshold for auto-advance with manual review fallback.

Routed to Overwatch for formal review before any automation goes live.

---

*FOREMAN — Build Stack Workshop — 2026-03-24*
*Responding to NSQ-MD-REQ-001. Four deliverables + operating plan complete. Standing by for review.*
