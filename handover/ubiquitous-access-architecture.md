# HANDOVER: Ubiquitous Access Architecture (YKS Ecosystem)

**From:** Taskmaster / Iteration Lab  
**To:** Architect (Foreman Build Stack)  
**Date:** 2026-03-25  
**Classification:** Architecture + Sprint Execution  
**Phase:** Implementation (Post-Eval Gate Approval)

---

## 1. OBJECTIVE

Establish a **persistent, secure, cloud-based development environment** that enables:

- Full YKS ecosystem access from **any device (iPad, iPhone, browser, laptop)**
- Persistent sessions across device switching
- Execution of development, DevOps, and infrastructure workflows without laptop dependency

---

## 2. ARCHITECTURE DECISION (APPROVED)

### Model: **Triple-Agent Architecture**

| Agent | Role |
|------|------|
| Cowork | Planning, connectors, orchestration |
| Claude Code (Local) | High-performance local execution |
| Claude Code (Cloud) | Persistent ubiquitous dev environment |

---

## 3. IMPLEMENTATION STRATEGY

### 3.1 Core Platform (Sprint 1 Target)

**Host:** AWS EC2 (existing Robo Stack environment or dedicated instance)

**Components:**
- code-server (browser IDE)
- Claude Code CLI (installed locally on EC2)
- Full toolchain:
  - git
  - terraform
  - kubectl
  - docker
  - wrangler
- tmux (session persistence)

---

### 3.2 Secure Access Layer

**Primary:**
- Cloudflare Tunnel (outbound-only connection)
- Cloudflare Access (Zero Trust + MFA)

**Access Path:**
```
Browser → Cloudflare Access → Tunnel → code-server (EC2)
```

---

### 3.3 Mobile Support

**Tier 1 (Browser):**
- code-server via Safari/Chrome

**Tier 2 (Terminal fallback):**
- Blink Shell (preferred)
- Termius (secondary)

---

## 4. EXPLICIT NON-GOALS (SPRINT 1)

- ❌ Do NOT deploy inside K3s yet
- ❌ Do NOT over-engineer orchestration
- ❌ Do NOT migrate all secrets without review
- ❌ Do NOT expose public ports

---

## 5. SECURITY REQUIREMENTS

- Zero-trust access only (Cloudflare Access)
- MFA enforced
- Least-privilege credentials
- Separate cloud credentials from laptop credentials
- Audit logging enabled

---

## 6. SUCCESS CRITERIA

Yeti must be able to:

- [ ] Access dev environment from iPad browser
- [ ] Edit code, commit, push
- [ ] Run Claude Code from browser terminal
- [ ] Execute `terraform plan` without laptop
- [ ] Maintain session across disconnects (tmux)
- [ ] Switch devices without losing state

---

## 7. ROBO STACK ALIGNMENT

Create new Epic:

**Epic:** Ubiquitous Cloud Dev Environment

Stories:
1. EC2 dev environment provisioning
2. code-server install + config
3. Claude Code CLI install
4. Toolchain install (git/terraform/kubectl/docker)
5. tmux session management
6. Cloudflare Tunnel setup
7. Cloudflare Access policy + MFA
8. Credential migration (secure)
9. Mobile validation (iPad/iPhone)
10. Logging + observability
11. Documentation + SOP

---

## 8. DELIVERABLES

- Running cloud dev environment
- Secure access endpoint (e.g., dev.nonsequitur.dev)
- Verified mobile workflow
- Documentation in repo:
  - `/docs/architecture/ubiquitous-access.md`
  - `/docs/sop/cloud-dev-environment.md`

---

## 9. HANDOFF NOTES

- This is **Sprint 1 build-critical infrastructure**
- Prioritize **working system over perfect system**
- Keep architecture **simple, inspectable, and debuggable**
- All changes must be:
  - committed
  - pushed
  - documented
  - linked to issues

---

## 10. NEXT PHASE (NOT IN THIS SPRINT)

- K3s integration (optional)
- Multi-node dev environments
- Agent runtime integration (Alistair Prime cloud execution)
- Advanced observability dashboards

---
