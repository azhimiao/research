---
name: research-survey
description: >-
  Orchestrate credible research surveys: author chooses scope (survey-only vs optional
  hypothesis/roadmap), runs literature-survey → review-synthesis → paper-quality-gate with
  adjustable parameters. Default is not end-to-end automation. Use when author asks for
  literature review, 文献调研, 综述, gap analysis, or research investigation. Entry point for the
  research theme. Triggers: "survey the literature", "research review", "systematic review"
  (lightweight; full PRISMA when prisma=true).
metadata:
  version: "2.0.0"
  status: stable
  protocol: skill-protocol-v2
compatibility: no auto experiments or full paper writing unless author explicitly confirms roadmap execution separately
---

# Research Survey

## Quick Start

1. READ refs/authority-standards.md; ASK author mode (A survey / B +hypothesis / C roadmap-only end_to_end) OR accept defaults
2. WRITE `literature/params.yaml` (min_verified_ratio, quality_gate, prisma); IF strict → set author_signoff_required=true
3. IF mode=end_to_end → ASK: "仅生成路线图，不自动实验或写全文"; IF declined → downgrade mode
4. RUN literature-survey workflow (see literature-survey skill): search, screen, verify, catalog
5. RUN review-synthesis workflow: narrative, matrix, gaps, optional hypotheses

## Workflow

### Step 1
READ refs/authority-standards.md; ASK author mode (A survey / B +hypothesis / C roadmap-only end_to_end) OR accept defaults

### Step 2
WRITE `literature/params.yaml` (min_verified_ratio, quality_gate, prisma); IF strict → set author_signoff_required=true

### Step 3
IF mode=end_to_end → ASK: "仅生成路线图，不自动实验或写全文"; IF declined → downgrade mode

### Step 4
RUN literature-survey workflow (see literature-survey skill): search, screen, verify, catalog

### Step 5
RUN review-synthesis workflow: narrative, matrix, gaps, optional hypotheses

### Step 6
RUN paper-quality-gate workflow: audit, score, WRITE quality-report.json; reject fabricated citations

### Step 7
IF gate blockers → FIX loop (max 2) OR return fix list to author

### Step 8
IF mode=end_to_end AND gate passed → GENERATE assets/roadmap.md (experiments, writing milestones, ethics/data checklist); ASK before any downstream automation

### Step 9
DELIVER summary: params used, Quality Gate score, quality-report path, verified source count, limitations, sign-off status

### Decision logic

```
IF author未选 mode → default survey; log in params.yaml
IF literature phase fails min_sources → do not proceed to synthesis without author consent
IF quality_gate fails strict → no delivery until resolved
IF author only wants phase 1 → stop after literature-survey; skip synthesis
IF author only wants audit → run paper-quality-gate on existing assets
IF contradiction between phases → quality-gate wins; synthesis must revise
```

## Outputs

Profile: `hybrid`

Return artifacts plus a narrative summary.

## Tools

| ID | Use | Constraints |
|----|-----|-------------|
| ask_user |  |  |
| file_read |  |  |
| web_search |  |  |
| memory_read |  |  |

## Failure Modes

| ID | Signal | Recovery |
|----|--------|----------|
| F1 | unverifiable or fabricated DOI/title/author | gate blocker; rollback claim |
| F2 | delivery without quality-report | forbidden |
| F3 | auto experiment or full paper without consent | stop; keep survey artifacts only |
| F4 | below min_sources | honest report; lower confidence globally |
| F5 | author wanted full FARS pipeline | clarify roadmap_only; offer staged manual steps |

## Dependencies

- `skill-core`
- `literature-survey`
- `review-synthesis`
- `paper-quality-gate`

## Additional Resources

- [IR source](references/ir.md)
