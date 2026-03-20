# Agent Definition Template

Use this template when standing up a new tower or assigning an AI instance to a role.

## Identity

- **Agent Name:** [e.g., The Architect]
- **Tower:** [e.g., Architecture & Strategy]
- **Type:** Human / AI / Hybrid

## Platform

- **AI Platform:** [e.g., Claude, ChatGPT, Windsurf, Self-hosted LLM]
- **Account:** [e.g., Google Account — architect@ag3ntics.dev]
- **Context Boundary:** [What this agent knows / doesn't know]

## Function

- **Primary Role:** [One sentence]
- **Responsibilities:**
  - [Responsibility 1]
  - [Responsibility 2]
  - [Responsibility 3]

## Inputs

- What does this agent receive to do its work?
- [e.g., Architecture docs from the Foreman's Build Stack]
- [e.g., GitHub issues assigned to its repos]

## Outputs

- What does this agent produce?
- [e.g., Design specs, gap registers, architecture decisions]
- [e.g., Commits to specific repos]

## Repos

- [repo-name-1] — read/write
- [repo-name-2] — read only

## Dependencies

- Which other agents does this one depend on?
- Which other agents depend on this one?

## Boot Context

- Link to context package: `framework/context-packages/[agent-name]-boot.md`

## Status

- [ ] Agent account created
- [ ] Context package written
- [ ] Repos assigned
- [ ] First task assigned
