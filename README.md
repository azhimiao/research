# Research Skills

面向**可信文献调查与综述**的 Cursor Agent Skills 套件，方法论对齐 PRISMA 2020、Cochrane Handbook、GRADE（简化）。默认流程：调查 → 综述 → 质量把关（非端到端自动化）。

| Skill | 作用 |
|-------|------|
| **research-survey** | 入口（推荐）— 编排三阶段 + 参数可调 |
| literature-survey | 检索、筛选、引用验证 |
| review-synthesis | 叙事综述、证据矩阵 |
| paper-quality-gate | 可信度审计与阻断交付 |

## 快速开始

```bash
git clone https://github.com/azhimiao/research.git
cd research
pip install -r requirements.txt

# 安装入口 skill（Cursor 全局）
python skill-core/skill.py install examples/research/research-survey --host cursor --scope global
```

Windows 可用 `./skill install examples/research/research-survey --host cursor --scope global`。

## 从源码编译

```bash
python skill-core/skill.py batch build research --test
python skill-core/skill.py install skill-forge/built/research/research-survey --host cursor --scope global
```

主题说明见 `skill-forge/themes/research/THEME.md`。权威参考在 `skill-forge/themes/research/refs/`（`authority-standards.md`、`citation-verification.md`、`quality-rubric.md` 等）。

## 典型流程

```
author 选 mode → params.yaml
  → literature-survey（检索 + 验证）
  → review-synthesis（综述 + 矩阵）
  → paper-quality-gate（审计）
  → 交付 survey-report + quality-report
```

## 仓库结构

| 路径 | 说明 |
|------|------|
| `examples/research/` | 可安装的 4 个 skill |
| `skill-forge/themes/research/` | 主题源（`.skill.md`、`refs/`） |
| `skill-forge/built/research/` | 编译输出 |
| `skill-core/` | 编译、测试、安装 CLI |

完整工具链见 [azhimiao/skillforskill](https://github.com/azhimiao/skillforskill)。
