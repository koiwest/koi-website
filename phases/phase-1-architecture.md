# Phase 1 - Architecture

[INPUT]: 依赖 PRODUCT_BRIEF、当前 repo、技术栈、用户指定的 first slice。
[OUTPUT]: 产出 SLICE_PLAN：目录、数据结构、provider 边界、mock 数据、验证命令。
[POS]: phases 的架构阶段，把产品猜测变成可施工切片。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Goal

Turn product inference into a sequence of small runnable builds.

## Repo Entry

Before edits:

1. Read root `CLAUDE.md` if present.
2. Inspect framework files: `package.json`, app/router directories, DB config, env examples.
3. If architecture docs are absent and files will be created, seed/update `CLAUDE.md`.
4. Prefer the existing stack. Only introduce a stack when starting from an empty repo.

## Architecture Rules

- Put external data behind provider interfaces.
- Store raw input beside processed output.
- Keep scoring/judgment separate from fetching.
- Keep UI read-only until the data path exists.
- Use local JSON/fixtures for the first pass.
- Make database optional for the first slice unless persistence is required by the demo.

## SLICE_PLAN Template

```markdown
# SLICE_PLAN

## Stack
- Framework:
- Data layer:
- Test/dev commands:

## Boundaries
| Layer | Responsibility | First implementation |
|---|---|---|
| Provider | fetch source data | MockProvider |
| Normalize | stable raw shape | local function |
| Judgment | classify/score/curate | deterministic fixture or LLM call |
| Persistence | store brief/raw data | JSON or SQLite |
| UI | inspect brief | route/page |

## Data Structures

## Files To Add
| File | Purpose |
|---|---|

## Slices
| Slice | Goal | Verification | Needs user? |
|---|---|---|---|
```

## Sub-Agent Option

When authorized, use sub-agents for sidecar research only:

- Architecture Scout: inspect repo structure and propose minimal files.
- Domain Modeler: derive data model from PRODUCT_BRIEF.

Main agent owns final architecture. Do not let sub-agents create overlapping edits unless write scopes are disjoint.

## Stop Condition

Continue to Phase 2 unless the user asked for plan-only or the next slice requires a credential.
