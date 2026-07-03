# Phase 2 - Mock Slice

[INPUT]: 依赖 SLICE_PLAN、mock data、当前 repo、first slice 验收标准。
[OUTPUT]: 产出 RUNNABLE_MVP_SLICE：一个本地可运行、可验证的垂直功能切片。
[POS]: phases 的第一施工阶段，先用 mock 跑通产品核心。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Goal

Build one vertical slice crossing data, logic, and UI. Do not build the whole product.

For the qunliao/AIFeed case, the first slice is:

```text
mock Twitter AI posts -> normalize -> classify/score/cluster -> generated brief -> /news-briefs UI
```

## Build Order

1. Add mock fixture data.
2. Add domain types/schema.
3. Add provider interface and `MockProvider`.
4. Add processing pipeline.
5. Add page/API route that reads generated output.
6. Add filters/expand-collapse only if required by acceptance.
7. Run local verification.
8. Update CLAUDE.md and module maps for new files.

## Rules

- Keep original source text and URLs visible.
- Do not call external APIs.
- Do not require user credentials.
- Keep scoring explainable.
- Prefer deterministic mock output if no LLM key is configured.
- If LLM is used, store the prompt contract and raw response for debugging.

## Phase Result Template

```markdown
## Phase 2 Result
- Built:
- Route/command:
- Mock data:
- Verified:
- Guesses:
- Bad smells:
- Next slice:
```

## Stop Condition

Stop and checkpoint after the first slice works. User correction here is valuable; continuing blindly multiplies wrong guesses.
