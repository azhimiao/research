# MD 兼容视图 — `eval-research-survey.yaml`

> **权威源文件**：同目录 `eval-research-survey.yaml`（本文件由 `skill-core` 自动生成，请勿手改；改源文件后重新 `batch build` 或运行 compat 同步。）

| 项 | 值 |
|----|-----|
| 类型 | `yaml` |
| 路径 | `refs/eval-research-survey.yaml` |
| 用途 | Skill 编译测试断言（eval）；CI `batch build --test` 读取 YAML 源文件。 |

## 内容

```yaml
tests:
  - id: T1
    description: orchestrator three-phase pipeline
    assert:
      contains: ["literature-survey", "review-synthesis", "paper-quality-gate", "quality-report"]
      sections: ["Quick Start", "Workflow", "Failure Modes"]

  - id: T2
    description: mode selection not forced e2e
    assert:
      contains: ["mode", "survey", "end_to_end", "ASK", "roadmap_only"]

  - id: T3
    description: authoritative params
    assert:
      contains: ["params.yaml", "min_verified_ratio", "prisma", "quality_gate"]

  - id: T4
    description: quality gate blocking
    assert:
      contains: ["quality-report", "fabricated", "author_signoff", "Quality Gate"]
```
