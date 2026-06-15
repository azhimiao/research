# 引用验证协议（零编造）

## 原则

**No citation without verification.** 每条进入 `Source Index` 的文献必须完成验证步骤并记录 provenance。

## 验证流程（每条文献）

1. **Collect** — 从检索结果提取 title, authors, year, venue, doi_or_url
2. **Resolve** — 按优先级：
   - 有 DOI → Crossref API 或 `https://doi.org/{doi}` HEAD/GET
   - 无 DOI → Semantic Scholar / OpenAlex 标题检索
   - 预印本 → arXiv / bioRxiv 摘要页
3. **Match** — 标题 normalized 相似度 ≥0.85 且年份 ±1；否则标 `MISMATCH`
4. **Record** — 写入 `literature/papers/{slug}.md` 的 `Verification` 段：

```markdown
## Verification
- status: verified | partial | unverified | mismatch
- verified_at: ISO8601
- resolver: crossref | semantic_scholar | openalex | manual
- canonical_url:
- notes:
```

5. **Block** — `quality_gate=strict` 时：`unverified` / `mismatch` **不得**进入 factual claims

## 禁止行为

- 编造 DOI、PMID、arXiv ID
- 把「记得有一篇」写成完整引用
- 从二次博客复制引用而不回查原文
- 猜测 abstract 或实验结果

## UNVERIFIED 使用规范

当仅知标题或无法打开全文：

```markdown
[UNVERIFIED: Title et al., ~YYYY — could not confirm metadata]
```

不得将 UNVERIFIED 条目计入 `min_sources` 的 verified 计数。

## quality-report 检查项

| check_id | 规则 |
|----------|------|
| fabrication | 零编造 ID |
| verification_rate | verified / total ≥ 阈值（strict 100%, standard 90%） |
| claim_citation | 每条 claim ≥1 verified source 或 TENTATIVE |
| doi_live | 抽样 DOI 可解析 |
