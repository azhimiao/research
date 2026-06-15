# MD 兼容视图 — `eval-paper-quality-gate.yaml`

> **权威源文件**：同目录 `eval-paper-quality-gate.yaml`（本文件由 `skill-core` 自动生成，请勿手改；改源文件后重新 `batch build` 或运行 compat 同步。）

| 项 | 值 |
|----|-----|
| 类型 | `yaml` |
| 路径 | `refs/eval-paper-quality-gate.yaml` |
| 用途 | Skill 编译测试断言（eval）；CI `batch build --test` 读取 YAML 源文件。 |

## 内容

```yaml
tests:
  - id: T1
    description: quality audit outputs
    assert:
      contains: ["quality-report", "quality-log", "verification_rate"]
      sections: ["Quick Start", "Workflow", "Failure Modes"]

  - id: T2
    description: rubric and blocking
    assert:
      contains: ["quality-rubric", "blocker", "fabrication", "strict"]

  - id: T3
    description: mandatory checklist
    assert:
      contains: ["Quality Gate Checklist", "CLAIM_CITATION", "RECENCY", "blocker"]

  - id: T4
    description: author sign-off
    assert:
      contains: ["author_signoff", "sign-off", "passed"]
```
