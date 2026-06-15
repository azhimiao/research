# MD 兼容视图 — `eval-review-synthesis.yaml`

> **权威源文件**：同目录 `eval-review-synthesis.yaml`（本文件由 `skill-core` 自动生成，请勿手改；改源文件后重新 `batch build` 或运行 compat 同步。）

| 项 | 值 |
|----|-----|
| 类型 | `yaml` |
| 路径 | `refs/eval-review-synthesis.yaml` |
| 用途 | Skill 编译测试断言（eval）；CI `batch build --test` 读取 YAML 源文件。 |

## 内容

```yaml
tests:
  - id: T1
    description: synthesis outputs
    assert:
      contains: ["survey-report", "evidence-matrix", "comparison-table", "gaps"]
      sections: ["Quick Start", "Workflow", "Failure Modes"]

  - id: T2
    description: synthesis methods
    assert:
      contains: ["thematic", "Open debates", "TENTATIVE", "certainty"]

  - id: T3
    description: no orphan claims
    assert:
      contains: ["matrix source", "hypotheses", "verified", "TENTATIVE"]

  - id: T4
    description: citation style
    assert:
      contains: ["citation_style", "APA", "Source Index"]
```
