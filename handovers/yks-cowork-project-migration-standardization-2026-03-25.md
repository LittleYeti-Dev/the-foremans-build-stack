# HANDOVER PACKET: YKS Cowork Project Migration & Standardization

**From:** Taskmaster (Cowork Instance — Scrum Master)  
**To:** Iteration Lab → Foreman  
**Date:** 2026-03-25  
**Classification:** Operational Architecture — Project Ecosystem Buildout  
**Purpose:** Flush out the full Cowork project roster so the Foreman can write up each project configuration with standardized boot sequences, persona definitions, and MCP connector requirements.

---

## 1. THE SITUATION

Yeti operates 8 Claude Projects that function as specialized personas for the YKS portfolio. These were originally built in Claude Chat (claude.ai Projects). Four have been migrated to Cowork. Four remain in Chat.

**The problem:** The Chat-based projects lack the boot protocol, deploy key access, MCP connectors, mounted workspace, and tool access that make the Cowork-migrated projects (especially Taskmaster) operationally effective. The Chat projects are basically prompt-only — no hands, no tools.

**The goal:** Bring all 8 projects into Cowork with standardized boot sequences, proper tool access, and clear lanes of responsibility. Each project becomes a fully operational Cowork instance that Yeti can activate based on the work being done.

---

## 2. FULL PROJECT ROSTER

### 2.1 Already in Cowork (4) — Boot Protocol Active

| # | Project | Persona | Role | Boot Status | Notes |
|---|---------|---------|------|-------------|-------|
| 1 | **Scrum Master (TM)** | Taskmaster | Sprint orchestration, issue triage, planning, cross-project awareness, document generation | **DIALED IN** — SSH deploy keys, PAT, gh CLI, GitHub API, MCP connectors, memory system, prompt SOP | Reference implementation. All other projects should inherit this boot pattern. |
| 2 | **Build Stack Workshop** | Foreman | Systems architecture, ADRs, technical design authority, eval gates | Migrated | Needs boot protocol audit — does it have deploy keys, gh, API access? |
| 3 | **Space Cyber (Overwatch)** | Overwatch | Cyber threat modeling, MITRE ATT&CK mapping, red/blue team, security review | Migrated | Needs boot protocol audit — security persona needs read access to all repos |
| 4 | **Chief (Yeti's Chief of Staff)** | Chief of Staff | Personal assistant, calendar, messaging, daily command review | Migrated | Different tool profile — Google Cal, Gmail, Notion are primary. Less GitHub, more PIM. |

### 2.2 Still in Claude Chat (4) — Need Cowork Migration

| # | Project | Persona | Role | Migration Priority | Notes |
|---|---------|---------|------|--------------------|-------|
| 5 | **DevOps** | Gunther Dev | Build execution, CI/CD, external dev coordination | **HIGH** — Robo Stack depends on this | Heaviest infrastructure persona. Needs: deploy keys, Terraform context, K8s awareness, Cloudflare MCP, GitHub Actions access. Strong candidate for Claude Code bare-metal handoff pattern. |
| 6 | **Business Foundry** | Business Foundry Master | Venture orchestration for NSQ business ops, 4 sub-personas | MEDIUM | Business strategy layer. Needs: Notion (business plans, OKRs), Google Drive, Gmail, possibly Canva for collateral. Less code, more documents. |
| 7 | **Content Lab** | Content Lab Master | Content production orchestration, 4 sub-personas | MEDIUM | Content pipeline. Needs: WordPress MCP (already connected), Canva, possibly social media MCPs, Google Drive. Paired with NSQ content strategy. |
| 8 | **Book Builder** | Book Builder Master | Fiction & non-fiction production pipeline | LOW | Creative production. Needs: Google Drive/Docs, document generation skills (docx, pdf, pptx). Minimal GitHub needs. Can wait until content pipeline is active. |

---

## 3. WHAT EACH COWORK PROJECT NEEDS

Every Cowork project should have a standardized configuration that includes:

### 3.1 Boot Protocol (Inherited from Taskmaster Pattern)

```text
1. Load SSH deploy keys from .secrets/deploy-keys/ (scoped per project need)
2. Configure SSH agent
3. Auth gh CLI with PAT (SSH protocol for git ops)
4. Pull live state from GitHub API (issues, milestones relevant to this persona)
5. Read boot context from .taskmaster/ (static reference)
6. Read project-specific instructions
7. Activate persona
8. Brief Yeti
```

**Key decision for iteration lab:** Do ALL projects share the same `.secrets/` directory and deploy keys, or do we scope key access per persona? (Security vs. convenience tradeoff — Overwatch should weigh in.)

### 3.2 MCP Connector Matrix

| Connector | Taskmaster | Foreman | Overwatch | Chief | DevOps | Biz Foundry | Content Lab | Book Builder |
|-----------|-----------|---------|-----------|-------|--------|-------------|-------------|-------------|
| GitHub (SSH + API) | ✅ | ✅ | ✅ (read) | — | ✅ | — | ✅ | — |
| Cloudflare | ✅ | — | ✅ | — | ✅ | — | ✅ | — |
| WordPress | ✅ | — | — | — | — | — | ✅ | — |
| Google Calendar | ✅ | — | — | ✅ | — | — | — | — |
| Gmail | ✅ | — | — | ✅ | — | ✅ | — | — |
| Notion | ✅ | ✅ | — | ✅ | — | ✅ | ✅ | ✅ |
| Google Drive | — | — | — | ✅ | — | ✅ | ✅ | ✅ |
| Canva | — | — | — | — | — | ✅ | ✅ | ✅ |
| Claude Code (bare metal) | handoff | handoff | — | — | **PRIMARY** | — | — | — |

### 3.3 Skill Requirements

| Skill | Taskmaster | Foreman | Overwatch | Chief | DevOps | Biz Foundry | Content Lab | Book Builder |
|-------|-----------|---------|-----------|-------|--------|-------------|-------------|-------------|
| docx | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ | ✅ |
| pdf | ✅ | ✅ | ✅ | ✅ | — | ✅ | — | ✅ |
| pptx | — | ✅ | ✅ | — | — | ✅ | — | — |
| xlsx | — | — | — | — | — | ✅ | — | — |

### 3.4 Mounted Workspace

All projects mount the same workspace folder (`Scrum Master (TM)`) which contains:
- `.secrets/` — deploy keys and PAT
- `prompts/` — organized by project and sprint
- `.taskmaster/` — boot context, persona configs, status cache

**Question for iteration lab:** Should each Cowork project have its own mounted workspace folder, or should they all share the Scrum Master folder? Sharing means shared secrets and shared state. Separate means duplicated secrets but cleaner isolation.

---

## 4. DUAL-AGENT ARCHITECTURE IMPACT

The dual-agent decision made today (2026-03-25) affects how projects are configured:

**DevOps (Gunther Dev)** is the primary candidate for heavy Claude Code bare-metal delegation. When this project is active in Cowork, it should be generating task prompts that Yeti feeds to Claude Code on the Mac for: Terraform ops, K8s management, Docker builds, CI/CD pipeline work, Cloudflare Wrangler deploys.

**Taskmaster** already uses the handoff pattern — writes prompt files to workspace, Claude Code executes on bare metal.

**All other projects** are primarily Cowork-native. They don't need bare-metal delegation unless they're generating code that needs to be tested/deployed.

---

## 5. MIGRATION SEQUENCE (RECOMMENDED)

| Phase | Project | Rationale |
|-------|---------|-----------|
| **Phase 1 (Now)** | Audit Foreman, Overwatch, Chief | Already in Cowork but need boot protocol verification. Bring them up to Taskmaster standard. |
| **Phase 2 (Next)** | Migrate DevOps | Highest priority — Robo Stack Sprint 1 depends on build execution capability. Pairs with Claude Code bare-metal. |
| **Phase 3** | Migrate Business Foundry | NSQ commercialization depends on business strategy tooling. |
| **Phase 4** | Migrate Content Lab | Activates when content pipeline goes live. WordPress MCP already connected. |
| **Phase 5** | Migrate Book Builder | Lowest urgency. Creative production can wait until pipeline infra is built. |

---

## 6. WHAT THE ITERATION LAB NEEDS TO PRODUCE

For each of the 8 projects, the iteration lab should produce:

1. **Project Instruction Set** — the custom instructions block that goes into the Cowork project settings (equivalent to what Taskmaster has now)
2. **Persona Definition** — name, role, activation triggers, tools, output expectations, quality standard, handoff protocol (per standing orders Section 2)
3. **Boot Sequence** — project-specific boot steps (what repos to clone, what API state to pull, what persona to activate)
4. **MCP Connector List** — exactly which connectors this project needs enabled
5. **Memory Seed** — initial MEMORY.md entries for the project's auto-memory system
6. **Workspace Structure** — what folder structure this project expects in its mounted workspace

---

## 7. WHAT THE FOREMAN NEEDS FROM THE ITERATION LAB

The Foreman's deliverable is the actual Cowork project configurations — written up and ready for Yeti to paste into each project's settings. The iteration lab output is the research and architecture. The Foreman turns that into build-ready specs.

Specifically:
- 8 project instruction documents (markdown, ready to paste into Cowork project settings)
- Updated `.taskmaster/personas/` directory in yks-hub with all 8 persona configs
- Updated boot-context.md reflecting the full 8-project ecosystem
- MCP connector requirements doc (so Yeti knows what to enable for each project)

---

## 8. EXEMPLARS — THE TWO REFERENCE IMPLEMENTATIONS

Every new Cowork project must be built from these two proven patterns. The iteration lab should study both before producing any project configuration.

### 8.1 EXEMPLAR A: Taskmaster Boot Protocol (Scrum Master Project)

This is the operational boot sequence that runs at session start. It establishes tool access, live state, and situational awareness before any work begins.

**What it does:**

```text
1. Load SSH deploy keys from .secrets/deploy-keys/ → ssh-add each key
2. Configure ~/.ssh/config for github.com (StrictHostKeyChecking no)
3. Install gh CLI (arm64 binary to ~/gh if no sudo)
4. Auth gh CLI with PAT from .secrets/github-pat.txt (SSH protocol for git ops)
5. Pull live state from GitHub API:
   - Open issue counts across all 9 repos
   - Milestone status for active projects
   - Recent commits on active branches
6. Read auto-memory (MEMORY.md index → relevant memory files)
7. Read project instructions (standing orders, persona definition)
8. Read boot-context.md from .taskmaster/ (static reference: repo map, architecture decisions, dependency graph)
9. Activate persona
10. Brief Yeti with live operational picture
```

**Key files and paths:**
- Deploy keys: `[workspace]/.secrets/deploy-keys/` (one per repo, private + .pub pairs)
- PAT: `[workspace]/.secrets/github-pat.txt` (API calls only, not git auth)
- Auto-memory: `[workspace root]/.auto-memory/MEMORY.md` (index) + individual memory files
- Boot context: `.taskmaster/boot-context.md` in yks-hub repo
- Project instructions: Cowork project settings custom instructions block
- Status cache: `.taskmaster/status/` (write-only — never read as source of truth at boot)

**Critical standing order:** GitHub Issues is the ONLY source of truth for dynamic state. Status files are session-end cache only. Boot-context.md is static reference only. Every boot pulls live from the GitHub API.

**What each new project inherits from this exemplar:**
- The full SSH + gh CLI setup sequence
- The "pull live, don't trust cache" discipline
- The memory system pattern (MEMORY.md index → typed memory files)
- The brief-at-boot pattern (don't start work until Yeti has the operational picture)

### 8.2 EXEMPLAR B: Daily Command Review Scheduled Task (Chief of Staff Project)

This is the structured operational task that demonstrates how a Cowork project uses scheduled tasks, MCP connectors, file persistence, and interactive walkthrough to deliver real operational value.

**What it does:**
- Fires every weekday at 7:30 AM ET
- Runs a 3-phase protocol: silent intake → interactive walkthrough → wrap-up with artifact updates

**Phase 0 (Silent Intake — before presenting anything to Yeti):**

```text
Pull in parallel:
- Google Calendar: full week via gcal_list_events (calendarId: primary, tz: America/New_York)
- Gmail: overnight emails via gmail_search_messages (since 6 PM previous day)
- Notion Quick Capture: new captures via notion-search (data_source_id for Quick Capture DB)
- Action Items: read Chief of Staff/ACTION_ITEMS.md from workspace
- Priority Board: read Chief of Staff/Priority_Board.md from workspace
- Cross-repo checks: git log + git status on the-markdown, content-ops, robo-stack, the-foremans-build-stack
- Operational state files: SESSION_INDEX.md, Commitment_Register.md, Decision_Log.md, Weekly_Review.md
```

Then triage into: Today / This Week / Waiting / Blocked / Parked

**Phase 1 (Interactive Walkthrough — one section at a time):**

```text
1. Calendar (full week, priority window = Mon-Tue-Wed) → ask for direction
2. Email triage (🔴 Action / 🟡 Awareness / ⚪ Low) → ask for direction
3. Action items & priorities → ask what gets worked TODAY
4. Notion quick capture → ask to promote, park, or kill
5. Cross-repo status → ask to prioritize, delegate, or park
6. Decisions & commitments → ask to close out
7. Alignment report (mismatches, missing tasks, stale work) → ask to correct
```

**Phase 2 (Wrap-up):**

```text
- Summarize all decisions from walkthrough
- Update artifacts in workspace: SESSION_INDEX, daily log, ACTION_ITEMS, Priority_Board
- Append Command Brief to handover/daily-command-review.md
- Commit locally (never auto-push)
```

**SOPs baked into the DCR (apply to all projects):**
- SOP-1: Batch deadlines apply to ALL items unless stated otherwise
- SOP-2: Top priorities stay at top. Yeti decides daily focus, not the agent.
- SOP-3: DCR-surfaced items go to a separate triage section, not into active work
- SOP-4: Parked items stay parked and don't resurface unless something changes
- SOP-5: Cross-repo checks every session
- SOP-6: ACTION_ITEMS.md and Priority_Board.md must stay in sync

**What each new project inherits from this exemplar:**
- The scheduled task pattern (timed trigger → silent intake → interactive walkthrough → artifact update)
- The MCP connector usage pattern (parallel pulls from Calendar, Gmail, Notion, GitHub)
- The dual-write discipline (workspace folder + git repo)
- The "surface, don't solve" principle (present → ask → get direction → act)
- The SOP structure (numbered, enforceable, referenced in execution)
- The triage system (🔴🟡⚪ for prioritization)
- The artifact persistence model (SESSION_INDEX, daily logs, registers, boards)
- The interactive walkthrough format (one section → ask → move on, never dump everything at once)

### 8.3 How the Two Exemplars Combine

For any new Cowork project, the build recipe is:

```text
EXEMPLAR A (Boot Protocol)     → HOW the project starts each session
                                  SSH keys, gh CLI, live state pull, memory, persona activation

EXEMPLAR B (DCR Task Pattern)  → HOW the project operates during a session
                                  Scheduled tasks, MCP intake, interactive walkthrough,
                                  artifact persistence, SOPs, dual-write

PROJECT-SPECIFIC CONFIG        → WHAT the project does
                                  Persona definition, repo scope, connector list,
                                  output types, handoff protocols
```

The iteration lab produces the project-specific config layer. The boot protocol and operational patterns are inherited from the exemplars.

**CONFLICT RESOLUTION:** If the two exemplars conflict on operational details (file paths, auth methods, boot steps, persistence locations), **Taskmaster is the source of truth.** The Taskmaster instance is the most current and proven operational configuration. The DCR may have paths or procedures that predate the current setup. Use Taskmaster for infrastructure/connectivity patterns. Use DCR for workflow/interaction patterns.

---

## 9. CONTEXT THE ITERATION LAB ALREADY HAS

- **Taskmaster project instructions:** Already in this Cowork instance — use as the reference implementation
- **DCR scheduled task:** Full text available in the Chief of Staff project — use as the operational task exemplar
- **Standing orders:** Sections 1-9 of the YKS Project Instructions (in knowledge source)
- **Dual-agent architecture:** Decided 2026-03-25, diagram at `yks-dual-agent-architecture.html`
- **Ubiquitous access spec:** At `prompts/robo-stack/ubiquitous-access/handover-ubiquitous-access-architecture.md` — the infra layer that all projects will eventually run on
- **Claude Code on bare metal:** Installed on Yeti's Mac as of 2026-03-25. Prompts targeting infra/deploy work must be structured for Claude Code execution.
- **Live repo state:** Pull from GitHub API, not from cached files (standing order #1)

---

## 10. DECISIONS YETI NEEDS TO MAKE (VIA ITERATION LAB RECOMMENDATION)

1. **Shared workspace vs. per-project workspace?** One mounted folder for all, or separate folders per project?
2. **Shared secrets vs. scoped secrets?** All projects see all deploy keys, or key access scoped by persona?
3. **DevOps persona: Cowork-primary or Claude Code-primary?** Does DevOps live mainly in Cowork with handoff to bare metal, or does it live mainly in Claude Code with Cowork just for planning?
4. **Sub-personas:** Business Foundry and Content Lab each have 4 sub-personas in their Chat definitions. Do these become separate Cowork projects or stay as modes within the parent project?
5. **Boot protocol: full clone or API-only?** Do all projects clone all repos at boot, or do some just pull API state for the repos they care about?

---

## 11. ITERATION LAB RECOMMENDATIONS (RESOLVED)

### 11.1 Workspace Strategy → Shared Core + Scoped Overlays

**Decision:** Use a shared root workspace with persona-scoped subdirectories.

```text
/workspace-root/
│
├── .secrets/
├── .taskmaster/
├── prompts/
├── repos/
│
├── personas/
│   ├── taskmaster/
│   ├── foreman/
│   ├── overwatch/
│   ├── chief/
│   ├── devops/
│   ├── bizfoundry/
│   ├── contentlab/
│   └── bookbuilder/
```

**Rule:** Shared infra is visible to all. Persona folders are write-scoped by SOP.

### 11.2 Secrets Strategy → Scoped Access, Not Fully Shared

**Decision:** Central storage, persona-scoped usage.

```text
.secrets/
├── github/
│   ├── taskmaster.key
│   ├── devops.key
│   ├── read-only.key
│
├── cloudflare/
├── google/
```

**Access model:**
- Taskmaster → all repos (admin)
- DevOps → infra repos (write)
- Foreman → design repos (write)
- Overwatch → read-only all
- Chief → no SSH, API only
- Others → minimal

### 11.3 DevOps Architecture → Claude Code Primary

**Decision:** DevOps lives on bare metal via Claude Code. Cowork DevOps handles orchestration and prompt generation only.

```text
Cowork (DevOps Persona)
    ↓ generates
Structured Execution Prompt
    ↓
Claude Code (Mac - bare metal)
    ↓ executes
Infra / CI / Deploy
```

**Rule:** DevOps never executes infra inside Cowork. DevOps always emits executable prompts.

### 11.4 Sub-Personas → Keep Inside Parent For Now

**Decision:** Do not split sub-personas into separate Cowork projects yet.

Business Foundry modes:
- Strategist
- Operator
- Finance
- Legal

Content Lab modes:
- Writer
- Editor
- Producer
- Distributor

Activation pattern:

```text
Activate Content Lab → Producer Mode
```

### 11.5 Boot Strategy → Hybrid (API-first + Selective Clone)

**Decision:** Default boot = API pull. Clone only when needed.

| Persona | Boot Mode |
|--------|----------|
| Taskmaster | API + selective clone |
| DevOps | FULL CLONE |
| Foreman | selective clone |
| Overwatch | API (read-only) |
| Chief | API only |
| Content Lab | API |
| Biz Foundry | API |
| Book Builder | none / documents only |

---

## 12. ARCHITECTURAL SUMMARY

This is not 8 tools. It is a distributed agent operating system with role-based execution nodes.

- Taskmaster = scheduler
- DevOps = execution engine
- Foreman = architect
- Overwatch = auditor
- Chief = interface layer
- Others = domain engines

The design behaves more like a control plane than a loose collection of assistants.
