# 质量评分量表（quality-report.json）

总分 `score` ∈ [0, 1]，加权如下。**passed** 规则见 `params.yaml` 的 `quality_gate`。

## 维度与权重

| 维度 | 权重 | 说明 |
|------|------|------|
| citation_integrity | 0.30 | 无编造、验证率、DOI 可解析 |
| evidence_coverage | 0.25 | claim 有源、 certainty 标注完整 |
| synthesis_quality | 0.20 | 主题聚类、非流水账、有对比 |
| methodology_transparency | 0.15 | 检索式、筛选日志、参数可追溯 |
| limitations_honesty | 0.10 | 明确缺口、偏倚、未覆盖领域 |

## 各级 gate 通过线

| gate | 最低 score | blockers |
|------|------------|----------|
| strict | 0.85 | 任一 fabrication；verification_rate < 1.0 |
| standard | 0.70 | fabrication；verification_rate < 0.90 |
| lenient | 0.55 | fabrication only |

## checks 数组（必含）

```json
[
  {"id": "fabrication", "passed": true, "weight": 0.30, "notes": ""},
  {"id": "verification_rate", "passed": true, "weight": 0.15, "value": 0.95, "threshold": 0.90},
  {"id": "claim_citation", "passed": true, "weight": 0.25, "notes": ""},
  {"id": "sections_coverage", "passed": true, "weight": 0.10, "notes": ""},
  {"id": "recency", "passed": true, "weight": 0.10, "value": 0.35, "threshold": 0.30},
  {"id": "prisma_log", "passed": true, "weight": 0.10, "notes": "N/A if prisma=false"}
]
```

## 论文/综述交付前人工清单（author sign-off）

- [ ] 研究问题与 scope 与原文一致
- [ ] 无关键文献遗漏（作者补充领域知识）
- [ ] 争议点双方均引用
- [ ] 语言与 venue 风格匹配
- [ ] 若投稿：符合目标期刊 citation style（APA/IEEE/GB/T 7714 等，author 指定）

`quality-report.json` 增加：

```json
"author_signoff": {"required": true, "completed": false, "notes": ""}
```

strict 模式：`author_signoff.completed` 须为 true 才 delivery。
