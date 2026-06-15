# 运行参数模板

复制为 `literature/params.yaml` 并在会话开始时与 author 确认。

```yaml
version: "2.0"

# survey | survey+hypothesis | end_to_end
mode: survey

framework: PICO  # PICO | SPIDER | PEO | custom

depth: standard  # quick | standard | deep
time_range: "2020-2026"
output_lang: zh

min_sources: 8
min_verified_ratio: 0.90  # strict: 1.0, standard: 0.90, lenient: 0.75
min_confidence: 0.6

quality_gate: standard  # strict | standard | lenient
prisma: false

sections:
  - summary
  - research_question
  - search_strategy
  - landscape
  - consensus
  - gaps
  - limitations
  - sources

# end_to_end 仅输出路线图，不自动实验/写全文
roadmap_only: true

citation_style: apa  # apa | ieee | gb7714 | author-specified

author_signoff_required: false  # strict gate 自动 true
```

## mode 说明

| mode | 阶段 |
|------|------|
| survey | literature-survey → review-synthesis → paper-quality-gate |
| survey+hypothesis | 上述 + hypotheses.json |
| end_to_end | 上述 + roadmap.md（须 author 确认） |
