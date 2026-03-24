# Foreman Handoff: Content Lab WP01 → WP02

**From:** Taskmaster  
**To:** Foreman (The Foreman's Build Stack)  
**Date:** 2026-03-24  
**Sprint:** Content Lab WP02 — Citation Remediation + Refinement  
**Type:** Subsprint handoff  
**Blocking:** Submission readiness  

---

## Context

Content Lab WP01 is complete. The HGC³AE² white paper has been formatted with:
- Cover page, copyright page, TOC
- 3 structured tables (Failure Modes, Framework Map, Skipjack Controls)
- 4 figures (Architecture, Misalignment Flow, Skipjack Loop, Trust Calibration Curve)
- Full body text with Georgia typography, branded headers/footers
- Chicago-formatted bibliography with hanging indents
- Deliverables: DOCX + PDF committed to `content-ops/white-paper/sprint/wp01-output/`

**What's NOT done:** The citation apparatus is the single biggest blocker. The handover packet from the iteration lab flagged this explicitly — WP-011 through WP-024 are all citation QA issues that must be resolved before the paper is submission-ready.

---

## Foreman Tasking

Analyze the following and produce an executable task breakdown for WP02:

### Input Artifacts (all in `content-ops` repo)
1. **Citation Audit Addendum** — `white-paper/Mitigating...Citation Audit Addendum.md`
   - 14 logged issues (WP-011 through WP-024)
   - Covers: placeholder removal, misattribution repair, Chicago normalization, footnote sync, claim-to-source validation
   
2. **Issue Log** — `white-paper/Mitigating...HGC3AE2 Framework.md`
   - WP-001 through WP-010 (core revision issues)
   - WP-005 (bibliography), WP-006 (footnote density), WP-010 (submission pass) overlap with citation audit

3. **Sprint Handover Packet** — `white-paper/...Sprint Build Handover.md`
   - Notes dependency on citation remediation completion

4. **WP01 Output** — `white-paper/sprint/wp01-output/`
   - Current formatted DOCX and PDF
   - Figures directory with SVG/PNG sources

### Original Manuscript (DOCX)
- `white-paper/Mitigating...Original Concept.docx`
- 42 body paragraphs, ~3,900 words, 140 footnotes, 44 bibliography entries

---

## Expected Foreman Output

1. **Task decomposition** — Break WP-011 through WP-024 into atomic, executable tasks
2. **Dependency graph** — Which tasks block which (remediation order matters)
3. **Effort estimate** — T-shirt sizing per task
4. **Subsprint board** — WP-Sprint-02 board definition
5. **GitHub issue seed** — Ready to plant as issues on content-ops
6. **Tool requirements** — What the Foreman needs (web search for source verification, DOI lookups, etc.)

---

## Constraints

- Chicago Manual of Style 17th edition is the citation standard
- Every source must be independently verifiable (DOI, stable URL, or publisher page)
- Footnotes and bibliography must be fully synchronized after remediation
- The master source table (WP-022) is a hard requirement — it becomes the audit trail
- No submission until WP-024 (final citation pass) is marked complete

---

## Remediation Order (from citation audit)
1. Remove placeholders and pseudo-citations (WP-011)
2. Repair misattributions and wrong years (WP-012, WP-021)
3. Convert topic-level refs to exact citations (WP-013)
4. Verify institutional/regulatory sources (WP-016)
5. Complete book chapter/handbook citations (WP-017)
6. Normalize all to full Chicago (WP-014, WP-015)
7. Map every footnote to verified entry (WP-018)
8. Verify claim-to-source alignment (WP-019)
9. Tag citation function types (WP-020)
10. Build master source table (WP-022)
11. Regenerate short-form notes (WP-023)
12. Final submission-safe bibliography pass (WP-024)

---

## Handoff Status
- [x] WP01 formatting sprint complete
- [x] DOCX + PDF committed to content-ops
- [x] Figures committed (SVG + PNG)
- [x] Citation audit addendum reviewed
- [ ] Foreman analysis pending
- [ ] WP02 task decomposition pending
- [ ] GitHub issues pending
