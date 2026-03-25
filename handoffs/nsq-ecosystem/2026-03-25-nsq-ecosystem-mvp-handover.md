# NSQ Ecosystem MVP Handover

Date: 2026-03-25
Owner: Justin Kuiper (Yeti)
Target repo for architecture and MVP buildout: `nsq-ecosystem`
Primary architect workspace: `the-foremans-build-stack`

## Purpose

This handover defines the high-level requirements for the NSQ Ecosystem MVP before implementation. The system is a human-governed, tower-based, agentic operating environment composed of three layers:

1. **ACI — Agentics Command Interface**
2. **GCOL — Generative & Coordination Operational Layer**
3. **UXL — User Experience Layer**

The MVP must be governed by the **HGC³AE² Framework** and the **Skipjack Protocol**, with explicit operational visibility and daily human governance initialized during standup.

---

## Core requirements

### 1) Three-layer architecture
- **ACI** is the human command surface and morning operating deck.
- **GCOL** is the governed agent-to-agent control plane.
- **UXL** is the NSQ-based interaction and external experience layer.

### 2) Governance-first doctrine
The system must be built around the HGC³AE² framework:
- **Human authority** is absolute and final.
- **Governance** determines what may execute.
- **Curated context + collaboration** are continuous, not assumed.
- **Engineered + executed alignment** must hold across design intent and runtime state.

### 3) Skipjack Protocol enforcement
The control plane must enforce:
- live validation of dynamic state
- no silent fallback
- explicit uncertainty signaling
- reconciliation checkpoints
- governance-gated execution
- runtime verification of assumptions
- iterative reapplication over time

**Hard law:** no silent continuation when context, validation, or authority is unresolved.

### 4) Tower-based control model
The control plane must operate through bounded tower domains:
- Architect — design authority
- Builder — implementation / manufacturing
- Editor — QA / validation
- Creative — design / content generation
- Engineer — infrastructure / deployment / runtime
- Client Towers — isolated delivery domains

Work must move between towers through governed pathways, not freeform agent chatter.

### 5) Morning Command Deck (ACI)
The command interface must support:
- My Day / morning brief
- persona-aware context (Yeti Admin, Architect, etc.)
- action ledger
- approvals queue
- handoff inbox
- command input / execute action
- click-through into Operations View

### 6) Persona-based context system
The system must render the same canonical work through different persona lenses without duplicating truth. Decisions made in one persona must propagate with context-aware interpretation into the others.

### 7) Operations visibility (non-optional)
When the user issues a command and executes it, the system must expose:
- intent ingestion
- context binding
- routing path
- active work state
- validation steps
- checkpoints
- uncertainty signals
- escalations
- completion / package state

This is operational telemetry, not chain-of-thought.

### 8) Unified production backbone
Both **Content Lab** and **Code Lab** must support distinct intake experiences but converge into the same production spine.

#### Divergent intake examples
- Are you building a digital product?
- Are you building digital content?
- Are you refining an existing artifact?
- Are you starting from a signal, brief, or existing asset?

#### Shared backbone
After intake, both paths must normalize into the same governed flow:
- framing / scope
- staging / workbench
- build / generation
- QA / validation
- assembly / packaging
- approval
- release (publish / deploy / deliver)
- iteration / feedback

### 9) Workbench / staging layer
The system must support temporary parking of work in progress across personas, devices, and workflows.

### 10) Ubiquitous interface requirement
The experience must be consistent across:
- MacBook
- iPad
- iPhone
- browser
- terminal

The system may render differently by surface, but the core objects, workflows, and controls must remain coherent.

### 11) RoboStack execution backbone
When the user says ubiquitous interface, that includes the ability to code from anywhere at near the same efficiency. This means:
- ACI is the command and control surface
- RoboStack is the execution substrate / remote dev backbone
- Remote environments, containers, and build resources likely live in or are brokered through RoboStack
- The MVP should define integration points for remote execution even if not all infra is implemented in the first sprint

### 12) Standup as governance initialization
Standup is not merely informational. It initializes:
- daily intent
- priorities
- authority boundaries
- risk posture
- execution constraints
- expected handoffs

### 13) ACI as front door
ACI must remain the front door for all major workflows. Content Lab and Code Lab should not become separate disconnected systems.

### 14) Tabletop-first workflow design
Before implementation hardens the workflow, the architect must walk the user through tabletop simulation:
- present the mock interface or flow
- user clicks through step by step
- the architect explains what happens next
- the flow is refined iteratively to match how Justin actually works and iterates

This tabletop layer sits above and informs the production backbone.

---

## MVP success criteria

The MVP is successful if it demonstrates all of the following:

1. **Morning flow works**
   - user opens ACI
   - sees day, priorities, handoffs, and approvals
   - switches persona and acts

2. **Command to visible execution works**
   - user issues a command
   - user can click Execute
   - user can inspect Operations View and see where work is flowing

3. **Skipjack behavior exists**
   - system does not proceed blindly on stale or unresolved state
   - uncertainty is surfaced explicitly
   - checkpoints pause or escalate when needed

4. **Cross-device coherence exists**
   - desktop, tablet, phone, and terminal preserve the same system logic and object model

5. **Content Lab is integrated**
   - user can create, park, move, and push content through staged flow

6. **Code Lab is integrated**
   - user can start product / build work from the same portal and drive it through the same governed backbone

7. **Unified production backbone exists**
   - content and code differ at intake and artifacts, but not in governance, QA, assembly, approvals, visibility, or iteration rhythm

8. **RoboStack integration points exist**
   - user can direct or trigger remote development actions through the architecture

9. **Tabletop walkthrough is supported**
   - architect provides a click-through mock that can be iterated before deeper buildout

---

## Architectural posture for the MVP

This system is **not** a normal dashboard.

It is a:
- human-governed agentic operating environment
- tower-based coordination system
- unified content/code manufacturing backbone
- portable command interface backed by RoboStack execution resources

The system must remain doctrine-first and visibility-first.

---

## Initial deliverables expected from the architect

1. System architecture overview
2. Layer map for ACI / GCOL / UXL
3. Tower coordination model
4. HGC³AE² mapping into control-plane requirements
5. Skipjack operational rules mapped into checkpoints and visibility
6. Ubiquitous interface model with RoboStack integration points
7. Unified Content Lab / Code Lab production backbone
8. Tabletop-ready mock for the first ACI morning flow walkthrough

---

## First tabletop target

The first tabletop should begin with the **morning command deck** and answer:
- What does the user see first?
- What are the main boxes or panels?
- What happens when the user clicks one?
- What happens when the user issues a command?
- What appears in the operations layer?
- How do Content Lab and Code Lab branch and rejoin?

The architect should start with a simplified mock, not a finished implementation.
