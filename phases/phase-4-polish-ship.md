# Phase 4 - Polish And Ship

[INPUT]: 依赖可运行核心功能、真实或 mock 数据、用户反馈、验收清单。
[OUTPUT]: 产出 VALIDATED_PRODUCT：设计还原、QA、审计、部署准备和下一轮优化清单。
[POS]: phases 的收尾阶段，在功能闭环成立后处理视觉和上线风险。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Goal

Make the product usable, faithful, and shippable after the data loop works.

## Design Order

1. Information density.
2. Source traceability.
3. Filtering and sorting clarity.
4. Card hierarchy.
5. Visual fidelity to reference.
6. Animation and polish.

Do not add a marketing hero to an operational dashboard.

## QA

Run the checks in `references/validation.md`.

For `/news-briefs`, verify:

- source, count, and period visible
- tag filters work
- score sort works
- "only worth full consumption" works
- cards collapse and expand
- source links open original URLs
- reasons and recommendations visible

## Audit

Check:

- provider credentials not committed
- raw data and processed data separated
- no original brand residue unless intentionally referenced
- no hidden tracker copied from reference site
- no external links without purpose
- docs updated

## Final Report

```markdown
## Ship Result
- Working product:
- Verified:
- Remaining guesses:
- Known limitations:
- Next high-leverage improvement:
```
