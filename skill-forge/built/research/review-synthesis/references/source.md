---
name: review-synthesis
profile: hybrid
status: stable
version: "2.0.0"
invocation: auto
host: any
---

# Goal
Produce authoritative narrative review and structured evidence from verified literature artifacts: thematic synthesis, consensus/debate, gaps, optional hypotheses. Requires literature-survey outputs or equivalent.

# Context
Author has paper notes and wants 综述, related work section, landscape report, or evidence matrix. Phase 2 of research-survey. Triggers: "write the review", "synthesize findings", "写综述", "related work draft", "evidence matrix".

# Constraints
- 时间：synthesis after literature phase; one draft + revision pass
- 精度：every claim traceable to verified source or TENTATIVE/UNVERIFIED
- 工具限制：do not add new papers without re-running verification discipline

# Inputs
## Required
- literature/papers/*.md OR literature/included-sources.json
- literature/research-question.md

## Optional
- literature/params.yaml — sections, mode, output_lang, citation_style
- literature/survey.md — cluster seed from phase 1

# Outputs
**Profile:** hybrid

1. `assets/survey-report.md` — full narrative review (outline: refs/survey-standards.md)
2. `assets/evidence-matrix.json` — claims with certainty (schema: refs/evidence-matrix.schema.json)
3. `literature/comparison-table.md` — cross-study comparison (standard+ depth)
4. `literature/gaps.md` — structured gaps with evidence links
5. `assets/hypotheses.json` — only if mode=survey+hypothesis or end_to_end

# Steps
1. READ refs/synthesis-methods.md and refs/authority-standards.md (GRADE simplified)
2. VALIDATE inputs exist; IF missing literature phase → RUN literature-survey or ASK author
3. READ all paper notes; REJECT unverified sources for factual claims (may cite as "identified but unverified")
4. BUILD thematic clusters per synthesis-methods; WRITE comparison-table for standard+ depth
5. DRAFT consensus and Open debates sections with dual-sided citations where conflict exists
6. EXTRACT gaps → literature/gaps.md; link to claim ids
7. COMPOSE survey-report.md sections per params.sections and output_lang
8. BUILD evidence-matrix.json: each claim has certainty, confidence, verified sources
9. IF mode includes hypothesis → GENERATE 3-5 hypotheses with validation_plan in hypotheses.json
10. SELF-CHECK: no sentence-level fact without matrix entry or TENTATIVE tag

# Decision
IF zero verified papers → STOP; return to literature-survey
IF confidence < min_confidence from params → mark claim TENTATIVE in report and matrix
IF contradiction → Open debates subsection; cite both sides
IF author wants related-work-only → trim to landscape + sources sections
IF new paper suggested during synthesis → ADD to literature phase with verification, not inline invent
IF citation_style specified → format Source Index accordingly (APA/IEEE/GB7714)

# Tools
- file_read — literature/*, refs/*
- file_write — assets/*, literature/gaps.md, comparison-table.md
- web_fetch — only to confirm quote/page, not discovery
- ask_user — section scope, hypothesis mode, citation style

# Failures
F1: orphan-claim | factual sentence without matrix source | fix or mark TENTATIVE
F2: laundry-list | paper-by-paper summary without themes | restructure per synthesis-methods
F3: false-consensus | single-source stated as field consensus | downgrade certainty
F4: missing-debate | known conflict ignored | add Open debates section
F5: style-mismatch | citations not per author style | reformat Source Index

# Deps
depends_on:
  - skill-core
  - literature-survey
provides:
  - narrative-review
  - evidence-matrix
  - gap-analysis

# Version
version: "2.0.0"
status: stable
