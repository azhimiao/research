---
name: paper-quality-gate
description: >-
  Audit review artifacts for credibility before delivery: citation integrity, verification
  rates, synthesis quality, methodology transparency. Produces blocking or warning
  quality-report.json per refs/quality-rubric.md. Use when run after review-synthesis or
  before any survey delivery. Triggers: "quality check", "verify citations", "论文把关", "audit
  the review", "is this survey trustworthy".
metadata:
  version: "2.0.0"
  status: stable
  protocol: skill-protocol-v2
compatibility: auditor role — fix minor issues or return fix list, never silently pass failures
---

# Paper Quality Gate

## Quick Start

1. READ refs/quality-rubric.md, refs/citation-verification.md, params quality_gate
2. RUN fabrication scan: zero invented DOI/PMID/arXiv/author/title in report and matrix
3. COMPUTE verification_rate from included-sources vs verified status
4. AUDIT claim_citation: every non-TENTATIVE claim has ≥1 verified source in matrix
5. CHECK sections_coverage against params.sections

## Workflow

### Step 1
READ refs/quality-rubric.md, refs/citation-verification.md, params quality_gate

### Step 2
RUN fabrication scan: zero invented DOI/PMID/arXiv/author/title in report and matrix

### Step 3
COMPUTE verification_rate from included-sources vs verified status

### Step 4
AUDIT claim_citation: every non-TENTATIVE claim has ≥1 verified source in matrix

### Step 5
CHECK sections_coverage against params.sections

### Step 6
CHECK recency: share of sources within time_range vs gate threshold

### Step 7
IF prisma=true → validate screening-log.json completeness

### Step 8
SCORE synthesis_quality: thematic structure, debates, limitations present

### Step 9
APPLY weights from quality-rubric; RUN Quality Gate Checklist (FABRICATION, VERIFICATION, CLAIM_CITATION, RECENCY); SET passed/blockers/warnings

### Step 10
IF strict AND blockers → STOP delivery; WRITE fix list in quality-log.md

### Step 11
IF standard AND warnings → ASK author acknowledgment before delivery

### Step 12
IF strict → REQUIRE author_signoff.completed=true in quality-report.json

### Step 13
WRITE quality-report.json and quality-log.md; SUMMARIZE in 5 lines for author

### Decision logic

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

## Outputs

Profile: `hybrid`

Return artifacts plus a narrative summary.

## Tools

| ID | Use | Constraints |
|----|-----|-------------|
| file_read |  |  |
| file_write |  |  |
| web_fetch |  |  |
| ask_user |  |  |

## Failure Modes

| ID | Signal | Recovery |
|----|--------|----------|
| F1 | any invented metadata | blocker; fail gate |
| F2 | delivery without quality-report | forbidden |
| F3 | report claims not in matrix | sync or downgrade |
| F4 | score inflated despite blockers | re-run rubric |
| F5 | strict without author sign-off | block |

## Dependencies

- `skill-core`
- `review-synthesis`

## Additional Resources

- [IR source](references/ir.md)
