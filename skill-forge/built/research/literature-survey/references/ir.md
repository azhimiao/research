literature-survey

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
Conduct traceable, standards-aligned literature investigation: structured research question, reproducible search, screening log, verified paper notes. Does not write final review prose (see review-synthesis).

## Context
Author needs literature search, paper catalog, screening, or inputs for a survey. Use standalone or as phase 1 of research-survey. Triggers: "find papers", "文献检索", "build reading list", "systematic search", "related work sources".

## Constraints
- 时间：complete search+screen in session; document everything for audit
- 精度：follow refs/search-protocol.md and refs/citation-verification.md
- 工具限制：read-only on external repos; no fabricated metadata

---

# 2. Inputs（输入定义）

## Required Inputs
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

---

# 3. Outputs（输出定义）

**Profile:** hybrid

1. `literature/research-question.md` — structured question (PICO/SPIDER/PEO)
2. `literature/search-protocol.md` — databases, queries, dates, hits
3. `literature/screening-log.json` — PRISMA-style counts + exclusions
4. `literature/papers/{slug}.md` — one note per included paper (template: refs/paper-note-template.md)
5. `literature/survey.md` — raw thematic clusters (facts only, not narrative review)
6. `literature/included-sources.json` — machine-readable included set with verification status

---

# 5. Execution Plan（执行流程）

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

---

# 6. Decision Logic（决策系统）

```
IF no framework fits → ASK author; default PICO for intervention questions, SPIDER for qualitative
IF prisma=true → mandatory screening-log.json with all PRISMA 2020 count fields
IF verification status=mismatch → EXCLUDE from included set; log in screening-log
IF paywalled → note in paper file; status partial; do not invent abstract text
IF duplicate DOI → keep highest-quality venue version
IF author provides bib/local PDFs → INTEGRATE and still run verification
```

---

# 7. Tool / API Binding（工具绑定）

| Portable ID | Use | Constraints |
|-------------|-----|-------------|
| web_search | | |
| web_fetch | | |
| file_read | | |
| file_write | | |
| ask_user | | |
| memory_read | | |

---

# 10. Failure Modes（失败模式）

## F1: fabricated-citation
- Signal: invented DOI/title/author
- Recovery: exclude; mark UNVERIFIED; never write to included-sources
- Severity: block

## F2: protocol-after-search
- Signal: search without documented protocol
- Recovery: stop; write protocol first
- Severity: block

## F3: insufficient-verified
- Signal: verified count below min_sources
- Recovery: expand search or report honestly
- Severity: block

## F4: screening-gap
- Signal: no exclusion reasons logged
- Recovery: backfill screening-log.json
- Severity: block

## F5: secondary-only
- Signal: claim supported only by blog/news
- Recovery: exclude from included set
- Severity: block

---

# 12. Skill Graph Dependencies（依赖）

```yaml
depends_on:
  - skill-core
provides:
  - literature-search
  - paper-catalog
  - screening-log
```

---

# 13. Versioning（版本系统）

```yaml
version: "2.0.0"
status: stable
```
