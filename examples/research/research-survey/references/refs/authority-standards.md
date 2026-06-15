# 权威方法论依据

本主题 skill 对齐以下公开标准与手册（非医疗场景下做**适配**，不声称等同系统评价）：

| 标准 | 用途 | 参考 |
|------|------|------|
| **PRISMA 2020** | 检索、筛选、纳入流程的可追溯报告 | [prisma-statement.org](https://www.prisma-statement.org/) |
| **Cochrane Handbook** | 检索策略、重复剔除、偏倚意识 | [training.cochrane.org/handbook](https://training.cochrane.org/handbook) |
| **PICO / SPIDER / PEO** | 研究问题结构化 | Cochrane / Joanna Briggs Institute |
| **GRADE（简化）** | 证据确定性分级 | [gradepro.org](https://www.gradepro.org/) |
| **AMSTAR 2** | 评估已有系统综述质量（secondary review 时） | BMC Med 2017 |
| **CARS / 学术综述写作** | 叙事综述组织与批判性综合 | 各大学 writing center 共识 |

## 本工具链的定位

- **默认**：结构化**文献调查 + 叙事综述 + 质量把关**（scoping / narrative review 级别）
- **prisma=true**：输出 PRISMA 2020 式筛选表与 flow counts，仍**不替代**人工系统评价
- **禁止**：编造引用、未验证即写进正文、把 agent 输出当作可投稿终稿

## 问题框架（必选其一）

| 类型 | 框架 | 字段 |
|------|------|------|
| 干预/效果 | PICO | Population, Intervention, Comparison, Outcome |
| 定性/经验 | SPIDER | Sample, Phenomenon, Design, Evaluation, Research type |
| 政策/现象 | PEO | Population, Exposure, Outcome |

写入 `literature/research-question.md`，并在 `params.yaml` 中记录 `framework`.

## 证据确定性（GRADE 简化）

| 级别 | 含义 | 默认处理 |
|------|------|----------|
| **High** | 多篇一致 + 方法可靠 | 可写进综述结论段 |
| **Moderate** | 有支持但样本/方法有限 | 正文可用，注明限制 |
| **Low** | 单源或方法弱 | 标 TENTATIVE |
| **Very low** | 间接、冲突或未验证 | 仅讨论段 + UNVERIFIED |

每条 claim 在 `evidence-matrix.json` 中必须有 `certainty` 字段。

## 来源层级（优先顺序）

1. 同行评议期刊 / 会议（含 DOI）
2. 预印本（arXiv、bioRxiv 等，须标注 preprint）
3. 官方技术报告 / 标准文档
4. 学位论文（可检索条目）
5. 二次摘要（仅作线索，**不得**作为唯一依据）

博客、论坛、无作者页面：**不得**单独支撑 factual claim。
