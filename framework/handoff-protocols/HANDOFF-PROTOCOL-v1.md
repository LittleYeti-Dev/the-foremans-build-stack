# Agent Handoff Protocol v1

## Purpose

When the Founder (orchestrator) needs to pass work from one AI agent tower to another, this protocol ensures context isn't lost and the receiving agent can pick up cleanly.

## The Problem

Each AI agent runs in an isolated context. When Tower A finishes work that Tower B needs, the Founder currently has to manually re-explain everything. This doesn't scale.

## Handoff Package Structure

Every handoff between towers should include a **Handoff Package** committed to the relevant repo or posted as a GitHub Issue. The package contains:

### 1. Summary (3-5 sentences)
What was done, why, and what the receiving tower needs to do next.

### 2. Artifacts Produced
List of files, commits, or deliverables with paths:
- `path/to/file-1.md` — what it is
- `path/to/file-2.php` — what it is
- Commit hash: `abc1234` — what changed

### 3. Decisions Made
Key decisions the sending tower made that the receiving tower needs to know:
- Decision 1: [What was decided and why]
- Decision 2: [What was decided and why]

### 4. Open Questions
Things the sending tower couldn't resolve that the receiving tower should address:
- Question 1
- Question 2

### 5. Context References
Links to docs, wiki pages, or previous work the receiving tower should read:
- [Link 1](url) — why it's relevant
- [Link 2](url) — why it's relevant

### 6. Acceptance Criteria
How will we know the receiving tower's work is done?
- [ ] Criterion 1
- [ ] Criterion 2

## Handoff Channels

| Method | When to Use |
|--------|-------------|
| GitHub Issue | Standard work handoff — create an issue in the receiving tower's repo with the handoff package |
| Wiki Page | Architecture or design handoff — put the package in the wiki for persistence |
| Commit Message | Code handoff — include the package summary in the commit message |
| Pull Request | Code review handoff — PR description contains the package |

## Example

**Architect → Builder Handoff:**

> **Summary:** Designed the Signal Schema (SIG-01 gap closure). Schema defines 14 fields for signal objects with JSON Schema validation. Builder needs to implement this as a WordPress custom post type with meta fields.
>
> **Artifacts:** `projects/the-markdown/design/signal-schema-v1.json`
>
> **Decisions:** Used JSON Schema instead of database-native schema for portability. Domain field is an enum, not free text. Intent is an array (signals can have multiple intents).
>
> **Open Questions:** Should we migrate existing WP posts to the new schema, or only apply to new signals going forward?
>
> **Context:** See gap register SIG-01, Agentics Rev 2 Section 6 (Domain & Intent Model)
>
> **Acceptance:** Schema validation passes on all new signals. Editorial board reads from new fields. Backward compatible with existing board data.
