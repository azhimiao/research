---
name: review-synthesis
description: >-
  Produce authoritative narrative review and structured evidence from verified literature
  artifacts: thematic synthesis, consensus/debate, gaps, optional hypotheses. Requires
  literature-survey outputs or equivalent. Use when author has paper notes and wants 综述,
  related work section, landscape report, or evidence matrix. Phase 2 of research-survey.
  Triggers: "write the review", "synthesize findings", "写综述", "related work draft",
  "evidence matrix".
metadata:
  version: "2.0.0"
  status: stable
  protocol: skill-protocol-v2
compatibility: do not add new papers without re-running verification discipline
---

# Review Synthesis

## Quick Start

1. READ refs/synthesis-methods.md and refs/authority-standards.md (GRADE simplified)
2. VALIDATE inputs exist; IF missing literature phase → RUN literature-survey or ASK author
3. READ all paper notes; REJECT unverified sources for factual claims (may cite as "identified but unverified")
4. BUILD thematic clusters per synthesis-methods; WRITE comparison-table for standard+ depth
5. DRAFT consensus and Open debates sections with dual-sided citations where conflict exists

## Workflow

### Step 1
READ refs/synthesis-methods.md and refs/authority-standards.md (GRADE simplified)

### Step 2
VALIDATE inputs exist; IF missing literature phase → RUN literature-survey or ASK author

### Step 3
READ all paper notes; REJECT unverified sources for factual claims (may cite as "identified but unverified")

### Step 4
BUILD thematic clusters per synthesis-methods; WRITE comparison-table for standard+ depth

### Step 5
DRAFT consensus and Open debates sections with dual-sided citations where conflict exists

### Step 6
EXTRACT gaps → literature/gaps.md; link to claim ids

### Step 7
COMPOSE survey-report.md sections per params.sections and output_lang

### Step 8
BUILD evidence-matrix.json: each claim has certainty, confidence, verified sources

### Step 9
IF mode includes hypothesis → GENERATE 3-5 hypotheses with validation_plan in hypotheses.json

### Step 10
SELF-CHECK: no sentence-level fact without matrix entry or TENTATIVE tag

### Decision logic

```
IF zero verified papers → STOP; return to literature-survey
IF confidence < min_confidence from params → mark claim TENTATIVE in report and matrix
IF contradiction → Open debates subsection; cite both sides
IF author wants related-work-only → trim to landscape + sources sections
IF new paper suggested during synthesis → ADD to literature phase with verification, not inline invent
IF citation_style specified → format Source Index accordingly (APA/IEEE/GB7714)
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
| F1 | factual sentence without matrix source | fix or mark TENTATIVE |
| F2 | paper-by-paper summary without themes | restructure per synthesis-methods |
| F3 | single-source stated as field consensus | downgrade certainty |
| F4 | known conflict ignored | add Open debates section |
| F5 | citations not per author style | reformat Source Index |

## Dependencies

- `skill-core`
- `literature-survey`

## Additional Resources

- [IR source](references/ir.md)
