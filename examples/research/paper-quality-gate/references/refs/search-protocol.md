# 检索与筛选协议

## 1. 检索前（mandatory）

写入 `literature/search-protocol.md`：

```markdown
# Search Protocol

## Research question (structured)
- Framework: PICO | SPIDER | PEO
- ...

## Databases & sources
| Source | Rationale | Access |
|--------|-----------|--------|
| Semantic Scholar | 广覆盖 CS/跨学科 | API / web |
| OpenAlex | 开放书目前景 | API / web |
| Crossref | DOI 元数据验证 | API |
| arXiv / PubMed / ACM DL | 按 domain 选用 | web |

## Query blocks (document all)
| Block ID | Query string | Source | Date run | Hits |
|----------|--------------|--------|----------|------|

## Inclusion criteria
- 语言、时间、研究类型、与 question 相关性阈值

## Exclusion criteria
- 重复、离题、无法验证元数据、纯社媒

## Screening stages
1. Title/abstract screen
2. Full-text (or abstract) eligibility
3. Final included set
```

## 2. 查询构造（Cochrane 适配）

- 拆概念块：Population / Intervention / Outcome 等
- 每块 2–4 同义词，块内 OR，块间 AND
- 记录**完整检索式**与运行日期
- `depth` 决定轮数：quick=2, standard=3, deep=5+ 迭代

## 3. 去重

- 同一 DOI / arXiv ID → 保留一条
- 标题相似度 >90% 且同作者 → 人工标记 duplicate
- 写入 `literature/screening-log.json`

## 4. PRISMA 计数（prisma=true 或 standard+ gate）

```json
{
  "identified": 0,
  "deduplicated": 0,
  "screened_title_abstract": 0,
  "excluded_title_abstract": 0,
  "sought_retrieval": 0,
  "not_retrieved": 0,
  "assessed_eligibility": 0,
  "excluded_fulltext": 0,
  "included": 0,
  "reasons_excluded": [{"reason": "", "count": 0}]
}
```

## 5. 纳入文献最低标准

| depth | min_sources | 至少 peer-reviewed 占比 |
|-------|-------------|-------------------------|
| quick | 5 | 40% |
| standard | 8 | 50% |
| deep | 15 | 60% |

未达 min_sources：**不得**夸大覆盖度；在综述 Limitations 中明确说明。
