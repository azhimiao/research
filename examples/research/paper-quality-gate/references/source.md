---
name: paper-quality-gate
profile: hybrid
status: stable
version: "2.0.0"
invocation: auto
host: any
---

# Goal
Audit review artifacts for credibility before delivery: citation integrity, verification rates, synthesis quality, methodology transparency. Produces blocking or warning quality-report.json per refs/quality-rubric.md.

# Context
Run after review-synthesis or before any survey delivery. Triggers: "quality check", "verify citations", "论文把关", "audit the review", "is this survey trustworthy".

# Constraints
- 时间：full gate before delivery; strict mode may require author sign-off
- 精度：blocking on fabrication in all gates; thresholds vary by quality_gate level
- 工具限制：auditor role — fix minor issues or return fix list, never silently pass failures

# Inputs
## Required
- assets/survey-report.md
- assets/evidence-matrix.json
- literature/included-sources.json OR literature/papers/*.md
- literature/params.yaml

## Optional
- literature/search-protocol.md
- literature/screening-log.json

# Outputs
**Profile:** hybrid

1. `assets/quality-report.json` — score, checks, blockers, warnings, author_signoff
2. `literature/quality-log.md` — human-readable audit trail and fix instructions

# Steps
1. READ refs/quality-rubric.md, refs/citation-verification.md, params quality_gate
2. RUN fabrication scan: zero invented DOI/PMID/arXiv/author/title in report and matrix
3. COMPUTE verification_rate from included-sources vs verified status
4. AUDIT claim_citation: every non-TENTATIVE claim has ≥1 verified source in matrix
5. CHECK sections_coverage against params.sections
6. CHECK recency: share of sources within time_range vs gate threshold
7. IF prisma=true → validate screening-log.json completeness
8. SCORE synthesis_quality: thematic structure, debates, limitations present
9. APPLY weights from quality-rubric; RUN Quality Gate Checklist (FABRICATION, VERIFICATION, CLAIM_CITATION, RECENCY); SET passed/blockers/warnings
10. IF strict AND blockers → STOP delivery; WRITE fix list in quality-log.md
11. IF standard AND warnings → ASK author acknowledgment before delivery
12. IF strict → REQUIRE author_signoff.completed=true in quality-report.json
13. WRITE quality-report.json and quality-log.md; SUMMARIZE in 5 lines for author

# Decision
IF fabrication detected → blocker always; remove offending claims or mark UNVERIFIED
IF verification_rate below min_verified_ratio → blocker (strict) or warning (standard/lenient)
IF claim lacks verified source → fail CLAIM_CITATION check
IF recency below RECENCY threshold → warning or blocker per quality_gate
IF evidence-matrix schema invalid → blocker until fixed
IF search-protocol missing AND depth=deep → warning methodology_transparency
IF author_signoff required and false → block delivery
IF all pass → set passed=true; append Quality Gate Summary section to survey-report if missing

# Quality Gate Checklist
- FABRICATION: zero invented identifiers
- VERIFICATION: verification_rate meets min_verified_ratio
- CLAIM_CITATION: matrix aligns with report prose
- RECENCY: share of sources within time_range meets gate threshold
- PRISMA_LOG: complete when prisma=true

# Tools
- file_read — all artifacts and refs
- file_write — quality-report.json, quality-log.md, patch survey-report summary
- web_fetch — spot-check DOI resolution (sample ≥3 or all if strict)
- ask_user — acknowledgment, sign-off, fix approval

# Failures
F1: fabricated-citation | any invented metadata | blocker; fail gate
F2: gate-bypass | delivery without quality-report | forbidden
F3: matrix-drift | report claims not in matrix | sync or downgrade
F4: false-pass | score inflated despite blockers | re-run rubric
F5: signoff-skipped | strict without author sign-off | block

# Deps
depends_on:
  - skill-core
  - review-synthesis
provides:
  - survey-quality-gate
  - citation-audit

# Version
version: "2.0.0"
status: stable
