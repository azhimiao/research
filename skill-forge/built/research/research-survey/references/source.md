---
name: research-survey
profile: hybrid
status: stable
version: "2.0.0"
invocation: auto
host: any
---

# Goal
Orchestrate credible research surveys: author chooses scope (survey-only vs optional hypothesis/roadmap), runs literature-survey → review-synthesis → paper-quality-gate with adjustable parameters. Default is not end-to-end automation.

# Context
Author asks for literature review, 文献调研, 综述, gap analysis, or research investigation. Entry point for the research theme. Triggers: "survey the literature", "research review", "systematic review" (lightweight; full PRISMA when prisma=true).

# Constraints
- 时间：default single-session survey deliverable; multi-phase ok
- 成本：open-access first; paywalls documented
- 精度：paper-quality-gate must pass before final delivery
- 工具限制：no auto experiments or full paper writing unless author explicitly confirms roadmap execution separately

# Inputs
## Required
- research_question: string
- domain: string

## Parameters (refs/params-template.md)
- mode: survey | survey+hypothesis | end_to_end — default: survey
- framework, depth, time_range, min_sources, min_verified_ratio, min_confidence
- quality_gate: strict | standard | lenient — default: standard
- prisma, sections, output_lang, citation_style

# Outputs
**Profile:** hybrid

Always (after gate passes):
1. `assets/survey-report.md`
2. `assets/evidence-matrix.json`
3. `assets/quality-report.json`

Conditional:
4. `assets/hypotheses.json` — mode survey+hypothesis or end_to_end
5. `assets/roadmap.md` — mode end_to_end only (planning; no auto execution)

Intermediate (audit trail):
- `literature/*` — full workspace from phase 1–3

# Steps
1. READ refs/authority-standards.md; ASK author mode (A survey / B +hypothesis / C roadmap-only end_to_end) OR accept defaults
2. WRITE `literature/params.yaml` (min_verified_ratio, quality_gate, prisma); IF strict → set author_signoff_required=true
3. IF mode=end_to_end → ASK: "仅生成路线图，不自动实验或写全文"; IF declined → downgrade mode
4. RUN literature-survey workflow (see literature-survey skill): search, screen, verify, catalog
5. RUN review-synthesis workflow: narrative, matrix, gaps, optional hypotheses
6. RUN paper-quality-gate workflow: audit, score, WRITE quality-report.json; reject fabricated citations
7. IF gate blockers → FIX loop (max 2) OR return fix list to author
8. IF mode=end_to_end AND gate passed → GENERATE assets/roadmap.md (experiments, writing milestones, ethics/data checklist); ASK before any downstream automation
9. DELIVER summary: params used, Quality Gate score, quality-report path, verified source count, limitations, sign-off status

# Decision
IF author未选 mode → default survey; log in params.yaml
IF literature phase fails min_sources → do not proceed to synthesis without author consent
IF quality_gate fails strict → no delivery until resolved
IF author only wants phase 1 → stop after literature-survey; skip synthesis
IF author only wants audit → run paper-quality-gate on existing assets
IF contradiction between phases → quality-gate wins; synthesis must revise

# Tools
- ask_user — mode, parameters, sign-off, roadmap confirmation
- file_read / file_write — orchestrate literature/* and assets/*
- web_search / web_fetch — delegated to literature-survey phase
- memory_read — prior literature/ in project

# Failures
F1: fabricated-citation | unverifiable or fabricated DOI/title/author | gate blocker; rollback claim
F2: skipped-gate | delivery without quality-report | forbidden
F3: mode-overreach | auto experiment or full paper without consent | stop; keep survey artifacts only
F4: insufficient-sources | below min_sources | honest report; lower confidence globally
F5: e2e-misunderstood | author wanted full FARS pipeline | clarify roadmap_only; offer staged manual steps

# Deps
depends_on:
  - skill-core
  - literature-survey
  - review-synthesis
  - paper-quality-gate
provides:
  - research-survey-orchestrator
  - literature-survey
  - review-generation
  - survey-quality-gate

# Version
version: "2.0.0"
status: stable
