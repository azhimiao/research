---
title: 科研调查
tags: [research, literature, survey, review, quality-gate, prisma]
status: stable
---

# Theme: 科研调查

面向**可信文献调查与综述生成**的 skill 套件，方法论对齐 PRISMA 2020、Cochrane Handbook、GRADE（简化）等公开标准。

## 设计原则

- **默认**：调查 → 综述 → 质量把关（非端到端）
- **可选**：假设候选、路线图（不自动实验/写全文）
- **可信**：引用验证、筛选日志、quality-report 阻断交付

## Skills

| 源文件 | 角色 |
|--------|------|
| `research-survey.skill.md` | **入口编排**（推荐安装） |
| `literature-survey.skill.md` | 检索、筛选、验证 |
| `review-synthesis.skill.md` | 叙事综述、证据矩阵 |
| `paper-quality-gate.skill.md` | 可信度审计 |

## 权威参考（编译进各 skill `references/refs/`）

- `authority-standards.md` — PRISMA / Cochrane / PICO / GRADE
- `search-protocol.md` — 可复现检索
- `citation-verification.md` — 零编造验证
- `quality-rubric.md` — 评分与阻断规则
- `synthesis-methods.md` — 主题综合
- `params-template.md` — 可调参数

## 编译与测试

```bash
python skill-core/skill.py batch build research --test
python skill-core/skill.py install skill-forge/built/research/research-survey --host cursor --scope global
```

## 典型流程

```
author 选 mode → params.yaml
  → literature-survey（检索+验证）
  → review-synthesis（综述+矩阵）
  → paper-quality-gate（审计）
  → 交付 survey-report + quality-report
```
