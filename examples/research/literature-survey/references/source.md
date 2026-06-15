---
name: literature-survey
profile: hybrid
status: stable
version: "2.0.0"
invocation: auto
host: any
---

# Goal
Conduct traceable, standards-aligned literature investigation: structured research question, reproducible search, screening log, verified paper notes. Does not write final review prose (see review-synthesis).

# Context
Author needs literature search, paper catalog, screening, or inputs for a survey. Use standalone or as phase 1 of research-survey. Triggers: "find papers", "文献检索", "build reading list", "systematic search", "related work sources".

# Constraints
- 时间：complete search+screen in session; document everything for audit
- 精度：follow refs/search-protocol.md and refs/citation-verification.md
- 工具限制：read-only on external repos; no fabricated metadata

# Inputs
## Required
- research_question: string
- domain: string

## Parameters (from params.yaml or defaults)
- framework: enum — PICO | SPIDER | PEO — default: infer from question
- depth: enum — quick | standard | deep — default: standard
- time_range: string — default: last 5 years emphasis
- min_sources: number — default: 8
- min_verified_ratio: number — default: 0.90
- prisma: boolean — default: false
- output_lang: string — default: match author

# Outputs
**Profile:** hybrid

1. `literature/research-question.md` — structured question (PICO/SPIDER/PEO)
2. `literature/search-protocol.md` — databases, queries, dates, hits
3. `literature/screening-log.json` — PRISMA-style counts + exclusions
4. `literature/papers/{slug}.md` — one note per included paper (template: refs/paper-note-template.md)
5. `literature/survey.md` — raw thematic clusters (facts only, not narrative review)
6. `literature/included-sources.json` — machine-readable included set with verification status

# Steps
1. READ refs/authority-standards.md; APPLY question framework (PICO/SPIDER/PEO)
2. IF research_question vague → ASK author; WRITE `literature/research-question.md`
3. LOAD or CREATE `literature/params.yaml` from refs/params-template.md
4. WRITE `literature/search-protocol.md` BEFORE searching (protocol-first discipline)
5. SEARCH multi-source per refs/search-protocol.md: Semantic Scholar, OpenAlex, Crossref, domain DBs (arXiv, PubMed, ACM); RUN query rounds per depth
6. DEDUPE by DOI/arXiv ID; LOG in screening-log.json
7. SCREEN title/abstract against inclusion/exclusion; RECORD reasons for exclusions
8. FOR each candidate: VERIFY metadata per refs/citation-verification.md; WRITE paper note
9. INCLUDE only verified or partial (not mismatch); IF verified count < min_sources → EXPAND search or ASK author
10. CLUSTER included papers in `literature/survey.md` (themes, no synthesis prose)
11. EXPORT `literature/included-sources.json`; REPORT verified ratio and gaps

# Decision
IF no framework fits → ASK author; default PICO for intervention questions, SPIDER for qualitative
IF prisma=true → mandatory screening-log.json with all PRISMA 2020 count fields
IF verification status=mismatch → EXCLUDE from included set; log in screening-log
IF paywalled → note in paper file; status partial; do not invent abstract text
IF duplicate DOI → keep highest-quality venue version
IF author provides bib/local PDFs → INTEGRATE and still run verification

# Tools
- web_search — discover papers
- web_fetch — abstracts, landing pages, DOI resolution
- file_read — bib, params.yaml, refs/*
- file_write — literature/*
- ask_user — scope, framework, inclusion criteria
- memory_read — prior literature/ workspace

# Failures
F1: fabricated-citation | invented DOI/title/author | exclude; mark UNVERIFIED; never write to included-sources
F2: protocol-after-search | search without documented protocol | stop; write protocol first
F3: insufficient-verified | verified count below min_sources | expand search or report honestly
F4: screening-gap | no exclusion reasons logged | backfill screening-log.json
F5: secondary-only | claim supported only by blog/news | exclude from included set

# Deps
depends_on:
  - skill-core
provides:
  - literature-search
  - paper-catalog
  - screening-log

# Version
version: "2.0.0"
status: stable
