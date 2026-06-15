---
name: literature-survey
description: >-
  Conduct traceable, standards-aligned literature investigation: structured research
  question, reproducible search, screening log, verified paper notes. Does not write final
  review prose (see review-synthesis). Use when author needs literature search, paper
  catalog, screening, or inputs for a survey. Use standalone or as phase 1 of
  research-survey. Triggers: "find papers", "文献检索", "build reading list", "systematic
  search", "related work sources".
metadata:
  version: "2.0.0"
  status: stable
  protocol: skill-protocol-v2
compatibility: read-only on external repos; no fabricated metadata
---

# Literature Survey

## Quick Start

1. READ refs/authority-standards.md; APPLY question framework (PICO/SPIDER/PEO)
2. IF research_question vague → ASK author; WRITE `literature/research-question.md`
3. LOAD or CREATE `literature/params.yaml` from refs/params-template.md
4. WRITE `literature/search-protocol.md` BEFORE searching (protocol-first discipline)
5. SEARCH multi-source per refs/search-protocol.md: Semantic Scholar, OpenAlex, Crossref, domain DBs (arXiv, PubMed, ACM); RUN query rounds per depth

## Workflow

### Step 1
READ refs/authority-standards.md; APPLY question framework (PICO/SPIDER/PEO)

### Step 2
IF research_question vague → ASK author; WRITE `literature/research-question.md`

### Step 3
LOAD or CREATE `literature/params.yaml` from refs/params-template.md

### Step 4
WRITE `literature/search-protocol.md` BEFORE searching (protocol-first discipline)

### Step 5
SEARCH multi-source per refs/search-protocol.md: Semantic Scholar, OpenAlex, Crossref, domain DBs (arXiv, PubMed, ACM); RUN query rounds per depth

### Step 6
DEDUPE by DOI/arXiv ID; LOG in screening-log.json

### Step 7
SCREEN title/abstract against inclusion/exclusion; RECORD reasons for exclusions

### Step 8
FOR each candidate: VERIFY metadata per refs/citation-verification.md; WRITE paper note

### Step 9
INCLUDE only verified or partial (not mismatch); IF verified count < min_sources → EXPAND search or ASK author

### Step 10
CLUSTER included papers in `literature/survey.md` (themes, no synthesis prose)

### Step 11
EXPORT `literature/included-sources.json`; REPORT verified ratio and gaps

### Decision logic

```
IF no framework fits → ASK author; default PICO for intervention questions, SPIDER for qualitative
IF prisma=true → mandatory screening-log.json with all PRISMA 2020 count fields
IF verification status=mismatch → EXCLUDE from included set; log in screening-log
IF paywalled → note in paper file; status partial; do not invent abstract text
IF duplicate DOI → keep highest-quality venue version
IF author provides bib/local PDFs → INTEGRATE and still run verification
```

## Outputs

Profile: `hybrid`

Return artifacts plus a narrative summary.

## Tools

| ID | Use | Constraints |
|----|-----|-------------|
| web_search |  |  |
| web_fetch |  |  |
| file_read |  |  |
| file_write |  |  |
| ask_user |  |  |
| memory_read |  |  |

## Failure Modes

| ID | Signal | Recovery |
|----|--------|----------|
| F1 | invented DOI/title/author | exclude; mark UNVERIFIED; never write to included-sources |
| F2 | search without documented protocol | stop; write protocol first |
| F3 | verified count below min_sources | expand search or report honestly |
| F4 | no exclusion reasons logged | backfill screening-log.json |
| F5 | claim supported only by blog/news | exclude from included set |

## Dependencies

- `skill-core`

## Additional Resources

- [IR source](references/ir.md)
