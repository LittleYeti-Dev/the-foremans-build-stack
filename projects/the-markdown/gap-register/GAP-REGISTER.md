# The Markdown — Gap Register

**Produced:** March 20, 2026 — Phase 0 Architecture Audit
**Source:** 3 PDFs (handover, rev3 unified, agentics org rev2), 3 GitHub repos + wikis, 3 project boards, live site audit

## Summary

25 gaps identified across 5 categories. 6 are framework-level gaps that were invisible until the Agentics operating model was correctly understood.

---

## Framework Gaps (Agentics Layer)

| ID | Gap | Impact | Priority |
|----|-----|--------|----------|
| FW-01 | **Agent Handoff Protocol** — No formal way to pass context/artifacts between AI instances | Orchestrator bottleneck; Justin manually bridges every handoff | HIGH |
| FW-02 | **Context Packages** — No standard boot context for spinning up a new agent session | Each new session starts cold; knowledge is lost between sessions | HIGH |
| FW-03 | **Cross-Agent Traceability** — No audit trail of which agent produced what | Can't attribute work, debug decisions, or learn from patterns | MEDIUM |
| FW-04 | **Agent Role Registry** — No catalog of active agents, platforms, roles, repo access | Tribal knowledge only; doesn't scale to client work | HIGH |
| FW-05 | **Agent Definition Templates** — Described in Agentics doc but not implemented | Can't onboard new agents repeatably | MEDIUM |
| FW-06 | **Orchestrator Workload Visibility** — No dashboard showing what each agent is working on | Justin has to hold it all in his head | LOW |

## Signal Layer Gaps

| ID | Gap | Impact | Priority |
|----|-----|--------|----------|
| SIG-01 | **Signal Schema** — No formal data model; WP posts with ad-hoc meta fields | Can't build reliable downstream systems without a stable schema | HIGH |
| SIG-02 | **Intent Classification** — Every signal should carry intent tags (narrative_craft, innovation, etc.) | Meaning layer can't function without structured intent | MEDIUM |
| SIG-03 | **Sports Analytics** — No feeds, data sources, or scoring model | 7th domain defined but completely empty | LOW |
| SIG-04 | **Manual Signal Input** — No UI for human-submitted signals (non-RSS) | Limits signal sources to automated feeds only | LOW |

## Meaning Layer Gaps

| ID | Gap | Impact | Priority |
|----|-----|--------|----------|
| MNG-01 | **Domain Weighting Engine** — Hardcoded multipliers in prompt text, not configurable | Can't tune scoring without editing code | MEDIUM |
| MNG-02 | **Pattern Agent** — No narrative/analytical pattern extraction | Key differentiator from vision docs; completely absent | MEDIUM |
| MNG-03 | **Pattern Library** — No reusable pattern database linked to signals/outputs | Patterns can't be applied to future content without a library | LOW |
| MNG-04 | **Intent Agent** — No domain + intent classification agent | Classification happens implicitly through feed tags only | MEDIUM |

## Output Layer Gaps

| ID | Gap | Impact | Priority |
|----|-----|--------|----------|
| OUT-01 | **Content Lab** — No orchestration workspace for drafting/iteration/publishing | Biggest missing product feature; where Content Ops would live | HIGH |
| OUT-02 | **Content Agent** — No AI draft generation for articles, social posts | Manual content creation only | MEDIUM |
| OUT-03 | **Distribution Agent** — No automated multi-channel publishing (X, LinkedIn, YouTube, Medium) | Manual share buttons exist but no automation | MEDIUM |
| OUT-04 | **Feedback Agent** — No engagement signal extraction loop back to signal layer | Open-loop system; no learning from audience response | LOW |
| OUT-05 | **Book Pipeline** — No long-form content workflow (RFI-009) | Future capability, not blocking current work | LOW |
| OUT-06 | **Archive Page** — /the-markdown-archive/ returns 404 | Broken public-facing URL | HIGH (quick fix) |

## Infrastructure Gaps

| ID | Gap | Impact | Priority |
|----|-----|--------|----------|
| INF-01 | **API Abstraction** — No unified API; everything is wp_options + Code Snippets | Can't decouple from WordPress without an API layer | HIGH |
| INF-02 | **K8s Manifests** — 64 files committed, none applied to cluster | Robo Stack can't host self-hosted LLMs or services until K3s is live | HIGH |
| INF-03 | **Container Images** — Nothing built or pushed to GHCR | Prerequisite for K8s deployment | HIGH |
| INF-04 | **GitHub Secrets** — 5 critical secrets not configured (ANTHROPIC_API_KEY, KUBECONFIG_B64, GHCR_PAT, etc.) | Blocks CI/CD and automated deployments | HIGH |
| INF-05 | **Monitoring** — Prometheus/Grafana/Loki YAML exists but not running | No operational visibility into infrastructure | MEDIUM |
| INF-06 | **Traceability** — No Signal → Output lineage tracking system | Can't prove provenance of content | MEDIUM |
| INF-07 | **System Dashboard** — No operational visibility into pipeline health | Flying blind on system performance | MEDIUM |
| INF-08 | **Security** — 7 open tasks + 33 MITRE ATT&CK items unmitigated | Risk exposure on both WordPress and Robo Stack | MEDIUM |
