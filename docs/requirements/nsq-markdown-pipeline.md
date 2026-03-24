# NON SEQUITUR – MARKDOWN PIPELINE SYSTEM
## Requirements Document (Foreman Handover)
**Project:** Non Sequitur Web (Markdown System)  
**Phase:** Signal → Wrap → Analysis → Iteration → Implementation  
**Owner:** Justin Kuiper (Yeti)  
**Artifact ID:** NSQ-MD-REQ-001  

---

## 1. Purpose (Intent)
Design and implement a **content pipeline system** that transforms raw signals (RSS, ideas, external inputs) into structured, multi-output content (tweets, blogs, vlogs, white papers).

The **Markdown page** is the front-end visualization of the pipeline.  
The **Content Lab** is the back-end execution engine.

This system must:
- operationalize curiosity
- enable real-time content evolution
- provide a clear, navigable user experience
- integrate with the broader Non Sequitur ecosystem

---

## 2. Core Concept
> “It does not follow… or does it?”

The page should demonstrate that ideas may appear disconnected at first, but through structured processing they form coherent meaning.

---

## 3. System Architecture Overview

### 3.1 Dual-System Model
| Layer | Function |
|---|---|
| Markdown Page (Frontend) | Visual pipeline and reader experience |
| Content Lab (Backend) | Workflow engine and decision system |

---

## 4. Pipeline Model (Mandatory)
Each content item must move through the following stages.

### 4.1 Signal
**Function:** detect an interesting thread.  
**Source examples:** RSS, tweet, article, observation.

**Subsections:**
- Detection: what caught attention
- Initial commentary
- Early framing

**Output:** raw or lightly annotated input

### 4.2 Vector
**Function:** define and validate the idea.

**Subsections:**
- Definition (**new concepts live here**)
- Validation (is it real, useful, worth carrying forward?)
- Direction (where is it going?)

**Outputs:**
- tweet / short insight
- concept definition artifact
- early public commentary where appropriate

### 4.3 Wrapper
**Function:** cross-domain synthesis.

**Mandatory requirement:** each item in Wrapper should connect to at least one additional domain, such as:
- philosophy
- theology
- engineering
- cyber
- culture

**Subsections:**
- Analogy building
- Cross-domain mapping
- Concept strengthening

**Outputs:**
- diagrams (including Venn-style models)
- teasers / fragments
- medium-form blog seeds

### 4.4 Wave Drop
**Function:** final synthesis and application.

**Subsections:**
- Synthesis
- Application
- Deployment

**Outputs:**
- white papers
- blogs
- vlogs
- products
- speaking content

---

## 5. Visual Experience Requirements

### 5.1 Primary Flow
The page must support:
- **left-to-right wave flow** as the primary reading model
- optional vertical fallback where needed

### 5.2 Wave Visualization
- A continuous waveform should visually connect stages.
- Each idea becomes a wave thread.
- The wave should visibly progress through the pipeline.

### 5.3 Color / Arc System
- Each subject should have a distinct visual arc.
- Gradient progression should help readers follow a thread from start to finish.
- Subject matter should be easy to track visually.

### 5.4 State Visualization
| State | Visual cue |
|---|---|
| Active | Bright / animated |
| In-progress | Solid gradient |
| Paused | Fading wave |
| Archived | Removed from main visual emphasis |

---

## 6. Content State Management

### 6.1 Active Flow
- Appears on the main Markdown page
- Shows full pipeline progression

### 6.2 Paused / Stale
If an item is defined but not actively advanced:
- the wave should visually taper or fade
- the item should clearly appear as inactive / held

### 6.3 Archive System
The archive should:
- be visually lighter than the main Markdown page
- be date-sorted
- preserve old signals without cluttering active flow

This should function as a straightforward Markdown archive rather than a visually dominant experience.

---

## 7. User Interaction Model (Creator Side)
At each stage, the system should support prompts or decision gates for the creator.

### Decision Gates
- Advance
- Hold
- Archive
- Publish (partial)
- Expand into a series
- Keep private

---

## 8. Content Output Mapping
| Stage | Typical output |
|---|---|
| Signal | Reference / citation / captured source |
| Vector | Tweet / short insight / concept definition |
| Wrapper | Diagram / teaser / blog seed |
| Wave Drop | White paper / vlog / full blog / productized output |

---

## 9. Cross-Linking Requirements
The Markdown page must support links to:
- related blog posts
- YouTube videos / vlogs
- books
- external references

Each wave thread should function as a navigable knowledge chain through the broader NSQ ecosystem.

---

## 10. Comment System Requirements

### 10.1 Modes
- Open comments should be possible
- Moderated / curated display should be supported

### 10.2 Control Model
The owner should be able to:
- highlight quality comments
- suppress noise
- disable comments per thread if needed

### 10.3 Future Option
- trusted contributor / tiered commenting model

---

## 11. Content Lab (Backend) Requirements

### 11.1 Ingestion
The backend should support:
- RSS feeds across multiple domains
- manual entry
- social ingestion

### 11.2 Pipeline Tracking
Each item should be a tracked object with:
- stage
- status
- date
- tags

### 11.3 Platform Direction
Candidate backend model: **GitLab or Git-style content ops workflow**.

The system should support:
- Kanban-style movement
- tagging
- phase tracking
- handoff discipline

---

## 12. Kanban Model Alignment
The pipeline should be translatable into a Kanban-style board:

| Kanban column | Pipeline stage |
|---|---|
| Backlog | Signal |
| In Progress | Vector |
| Review | Wrapper |
| Done / Published | Wave Drop |

---

## 13. Guiding Theme (Global Requirement)
> **Curiosity is the engine.**

Curiosity should be legible across the full website and especially within this system as:
- Signal (spark)
- Vector (direction)
- Wrapper (connection)
- Wave Drop (understanding)

---

## 14. Header Requirement
The top of the page must prominently display:

> **“It does not follow… or does it?”**

---

## 15. Extensibility Requirements
The system should support:
- future automation / AI-assisted movement through stages
- additional stages if needed later
- multiple concurrent threads
- integration with the larger NSQ / Agentics ecosystem

---

## 16. Non-Functional Requirements
- fast load times
- mobile-friendly experience
- clean UX despite conceptual depth
- scalable to high content volume

---

## 17. Risks / Watch Items
- over-complex UI that becomes hard to follow
- comment noise degrading signal quality
- pipeline friction that feels bureaucratic rather than fluid
- visual overload from wave effects or color systems

---

## 18. Deliverables for Foreman
Immediate analysis tasks:
1. Define data model for pipeline objects
2. Design UI wireframe for a wave + Kanban hybrid experience
3. Recommend backend platform / workflow approach
4. Define tagging and phase schema
5. Prototype the Markdown page layout for the NSQ website refactor

---

## 19. Iteration Lab Tagging
- **Signal:** concept captured from live ideation
- **Wrap:** structured into a system model
- **Analysis:** pipeline, UX, and operating breakdown
- **Iteration:** refined with visual and operational constraints
- **Implementation:** ready for Foreman analysis and build planning

---

## 20. Final Note to Foreman
This is not just a blog page.

It is:
- a **content operating system**
- a **thinking pipeline made visible**
- a **curiosity engine rendered in public**

Primary use case: **NSQ website refactor** and the formalization of the Markdown page as the front door to the wider content ecosystem.
