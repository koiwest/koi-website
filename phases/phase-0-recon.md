# Phase 0 - Recon

[INPUT]: 依赖参考 URL、用户目标、截图、源码/网络证据、可用浏览器或搜索工具。
[OUTPUT]: 产出 PRODUCT_BRIEF：核心循环、功能地图、证据等级、首个可运行切片。
[POS]: phases 的入口阶段，被 SKILL.md 在任何产品复刻任务开始时调用。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Goal

Infer what the product does before deciding what to build.

Do not start from layout. Start from:

- user outcome
- core loop
- durable objects
- source inputs
- judgment/ranking logic
- hidden jobs
- feedback loops

## Required Reads

- `references/reconstruction-doctrine.md`
- `references/subagent-protocol.md` if sub-agent work is authorized by the user and tools are available
- `references/ai-curation-mvp.md` when the product is a news brief / AI feed / curation dashboard

## Workflow

1. Capture the user's exact desired first slice.
2. Inspect the target URL or repo when available.
3. List visible entities, actions, filters, periods, routes, cards, and source labels.
4. Search public source only when appropriate.
5. Treat network/API findings as `SOURCE`; UI hints as `PARTIAL`; implementation choices as `GUESS`.
6. If sub-agent tools are available and the user asked for sub-agent/parallel work, dispatch:
   - Product Archaeologist: UI/routes/product loop
   - Data Detective: APIs/providers/schema/raw data
   - UX Cartographer: information architecture and interaction states
7. Merge findings into one product brief. Do not let sub-agents decide architecture independently.

## PRODUCT_BRIEF Template

```markdown
# PRODUCT_BRIEF

## User Outcome

## Reference Product
- URL:
- Observed surface:
- Unknowns:

## Core Loop
1.
2.
3.

## Product Objects
| Object | Meaning | Evidence | Confidence |
|---|---|---|---|

## Feature Map
| Feature | Evidence | Confidence | Core object | First slice | Needs user/API? |
|---|---|---:|---|---|---|

## First Slice
- Name:
- Why first:
- Mock data:
- Acceptance:

## Guesses To Validate
```

## Stop Condition

Stop after Phase 0 only if the user requested recon-only or if the next step requires credentials/legal authorization. Otherwise continue to Phase 1.
