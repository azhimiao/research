# MD 兼容视图 — `evidence-matrix.schema.json`

> **权威源文件**：同目录 `evidence-matrix.schema.json`（本文件由 `skill-core` 自动生成，请勿手改；改源文件后重新 `batch build` 或运行 compat 同步。）

| 项 | 值 |
|----|-----|
| 类型 | `json` |
| 路径 | `refs/evidence-matrix.schema.json` |
| 用途 | JSON Schema 定义。 |

## 内容

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "EvidenceMatrix",
  "type": "object",
  "required": ["research_question", "generated_at", "claims"],
  "properties": {
    "research_question": { "type": "string" },
    "framework": { "enum": ["PICO", "SPIDER", "PEO", "custom"] },
    "generated_at": { "type": "string", "format": "date-time" },
    "claims": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["id", "claim", "certainty", "confidence", "sources"],
        "properties": {
          "id": { "type": "string" },
          "claim": { "type": "string" },
          "certainty": { "enum": ["high", "moderate", "low", "very_low"] },
          "confidence": { "type": "number", "minimum": 0, "maximum": 1 },
          "sources": {
            "type": "array",
            "items": {
              "type": "object",
              "required": ["title", "verification_status"],
              "properties": {
                "title": { "type": "string" },
                "doi_or_url": { "type": "string" },
                "verification_status": { "enum": ["verified", "partial", "unverified"] }
              }
            }
          },
          "notes": { "type": "string" }
        }
      }
    }
  }
}
```
