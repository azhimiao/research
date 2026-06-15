# MD 兼容视图 — `eval-literature-survey.yaml`

> **权威源文件**：同目录 `eval-literature-survey.yaml`（本文件由 `skill-core` 自动生成，请勿手改；改源文件后重新 `batch build` 或运行 compat 同步。）

| 项 | 值 |
|----|-----|
| 类型 | `yaml` |
| 路径 | `refs/eval-literature-survey.yaml` |
| 用途 | Skill 编译测试断言（eval）；CI `batch build --test` 读取 YAML 源文件。 |

## 内容

```yaml
tests:
  - id: T1
    description: search and screening workflow
    assert:
      contains: ["search-protocol", "screening-log", "included-sources"]
      sections: ["Quick Start", "Workflow", "Failure Modes"]

  - id: T2
    description: citation verification
    assert:
      contains: ["citation-verification", "Crossref", "UNVERIFIED", "VERIFY"]

  - id: T3
    description: PRISMA and frameworks
    assert:
      contains: ["PICO", "SPIDER", "prisma", "protocol-first"]

  - id: T4
    description: standards reference
    assert:
      contains: ["authority-standards", "min_sources", "verified"]
```
