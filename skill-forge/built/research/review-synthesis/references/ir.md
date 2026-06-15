review-synthesis

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
Produce authoritative narrative review and structured evidence from verified literature artifacts: thematic synthesis, consensus/debate, gaps, optional hypotheses. Requires literature-survey outputs or equivalent.

## Context
Author has paper notes and wants 综述, related work section, landscape report, or evidence matrix. Phase 2 of research-survey. Triggers: "write the review", "synthesize findings", "写综述", "related work draft", "evidence matrix".

## Constraints
- 时间：synthesis after literature phase; one draft + revision pass
- 精度：every claim traceable to verified source or TENTATIVE/UNVERIFIED
- 工具限制：do not add new papers without re-running verification discipline

---

# 2. Inputs（输入定义）

## Required Inputs
## Required
- literature/papers/*.md OR literature/included-sources.json
- literature/research-question.md

## Optional
- literature/params.yaml — sections, mode, output_lang, citation_style
- literature/survey.md — cluster seed from phase 1

---

# 3. Outputs（输出定义）

**Profile:** hybrid

1. `assets/survey-report.md` — full narrative review (outline: refs/survey-standards.md)
2. `assets/evidence-matrix.json` — claims with certainty (schema: refs/evidence-matrix.schema.json)
3. `literature/comparison-table.md` — cross-study comparison (standard+ depth)
4. `literature/gaps.md` — structured gaps with evidence links
5. `assets/hypotheses.json` — only if mode=survey+hypothesis or end_to_end

---

# 5. Execution Plan（执行流程）

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

---

# 6. Decision Logic（决策系统）

```
IF zero verified papers → STOP; return to literature-survey
IF confidence < min_confidence from params → mark claim TENTATIVE in report and matrix
IF contradiction → Open debates subsection; cite both sides
IF author wants related-work-only → trim to landscape + sources sections
IF new paper suggested during synthesis → ADD to literature phase with verification, not inline invent
IF citation_style specified → format Source Index accordingly (APA/IEEE/GB7714)
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

## F1: orphan-claim
- Signal: factual sentence without matrix source
- Recovery: fix or mark TENTATIVE
- Severity: block

## F2: laundry-list
- Signal: paper-by-paper summary without themes
- Recovery: restructure per synthesis-methods
- Severity: block

## F3: false-consensus
- Signal: single-source stated as field consensus
- Recovery: downgrade certainty
- Severity: block

## F4: missing-debate
- Signal: known conflict ignored
- Recovery: add Open debates section
- Severity: block

## F5: style-mismatch
- Signal: citations not per author style
- Recovery: reformat Source Index
- Severity: block

---

# 12. Skill Graph Dependencies（依赖）

```yaml
depends_on:
  - skill-core
  - literature-survey
provides:
  - narrative-review
  - evidence-matrix
  - gap-analysis
```

---

# 13. Versioning（版本系统）

```yaml
version: "2.0.0"
status: stable
```
