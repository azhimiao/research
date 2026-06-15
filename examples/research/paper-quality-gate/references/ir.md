paper-quality-gate

---

# 0. Compilation Target

```yaml
host: any
invocation: auto
output_profile: hybrid
```

---

# 1. Intent（意图）

Theme: research

## Goal
Audit review artifacts for credibility before delivery: citation integrity, verification rates, synthesis quality, methodology transparency. Produces blocking or warning quality-report.json per refs/quality-rubric.md.

## Context
Run after review-synthesis or before any survey delivery. Triggers: "quality check", "verify citations", "论文把关", "audit the review", "is this survey trustworthy".

## Constraints
- 时间：full gate before delivery; strict mode may require author sign-off
- 精度：blocking on fabrication in all gates; thresholds vary by quality_gate level
- 工具限制：auditor role — fix minor issues or return fix list, never silently pass failures

---

# 2. Inputs（输入定义）

## Required Inputs
## Required
- assets/survey-report.md
- assets/evidence-matrix.json
- literature/included-sources.json OR literature/papers/*.md
- literature/params.yaml

## Optional
- literature/search-protocol.md
- literature/screening-log.json

---

# 3. Outputs（输出定义）

**Profile:** hybrid

1. `assets/quality-report.json` — score, checks, blockers, warnings, author_signoff
2. `literature/quality-log.md` — human-readable audit trail and fix instructions

---

# 5. Execution Plan（执行流程）

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

---

# 6. Decision Logic（决策系统）

```
IF fabrication detected → blocker always; remove offending claims or mark UNVERIFIED
IF verification_rate below min_verified_ratio → blocker (strict) or warning (standard/lenient)
IF claim lacks verified source → fail CLAIM_CITATION check
IF recency below RECENCY threshold → warning or blocker per quality_gate
IF evidence-matrix schema invalid → blocker until fixed
IF search-protocol missing AND depth=deep → warning methodology_transparency
IF author_signoff required and false → block delivery
IF all pass → set passed=true; append Quality Gate Summary section to survey-report if missing
```

---

# 7. Tool / API Binding（工具绑定）

| Portable ID | Use | Constraints |
|-------------|-----|-------------|
| file_read | | |
| file_write | | |
| web_fetch | | |
| ask_user | | |

---

# 10. Failure Modes（失败模式）

## F1: fabricated-citation
- Signal: any invented metadata
- Recovery: blocker; fail gate
- Severity: block

## F2: gate-bypass
- Signal: delivery without quality-report
- Recovery: forbidden
- Severity: block

## F3: matrix-drift
- Signal: report claims not in matrix
- Recovery: sync or downgrade
- Severity: block

## F4: false-pass
- Signal: score inflated despite blockers
- Recovery: re-run rubric
- Severity: block

## F5: signoff-skipped
- Signal: strict without author sign-off
- Recovery: block
- Severity: block

---

# 12. Skill Graph Dependencies（依赖）

```yaml
depends_on:
  - skill-core
  - review-synthesis
provides:
  - survey-quality-gate
  - citation-audit
```

---

# 13. Versioning（版本系统）

```yaml
version: "2.0.0"
status: stable
```
