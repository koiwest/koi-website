# Incremental Reconstruction Workflow

[INPUT]: 依赖目标网站 URL、当前 repo、用户目标、可用 API 凭证、项目 CLAUDE.md。
[OUTPUT]: 提供从侦察、计划、切片、实现、验证到继续协作的执行流程。
[POS]: references 的施工流程文件，被 SKILL.md 在任何实际实现前读取。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Phase 0: Enter The Repo

Before editing:

1. Run `pwd`, `rg --files`, and inspect package/config files.
2. Read root `CLAUDE.md` if present.
3. If no architecture docs exist and files will be created, seed root `CLAUDE.md`.
4. Identify framework, package manager, test command, lint command, dev server command.

Output:

```markdown
## Repo Read
- Stack:
- Existing routes:
- Data layer:
- Test/dev commands:
- Docs status:
```

## Phase 1: Product Recon

If a URL exists:

1. Browse or fetch the page.
2. Record visible routes, labels, filters, objects, and actions.
3. Search public repo/source if appropriate.
4. Inspect network only when available and allowed.
5. Separate product function from visual treatment.

Output:

```markdown
## Product Map
- User outcome:
- Core loop:
- Durable objects:
- External sources:
- Hidden jobs:
- Confidence table:
```

## Phase 2: Slice Backlog

Create vertical slices. Each slice must be independently useful.

Recommended order:

1. Mock data and schema.
2. Read-only UI for one real route.
3. Local processing pipeline.
4. Persistence.
5. Provider interface.
6. Real provider.
7. Feedback/taste controls.
8. Design polish.
9. Deployment and audit.

Output:

```markdown
| Slice | Goal | Files | Verification | Needs user? |
|---|---|---|---|---|
```

## Phase 3: Implement One Slice

For each slice:

1. Edit only the files required by this slice.
2. Keep provider boundaries explicit.
3. Preserve raw inputs.
4. Add minimal tests or runnable scripts when risk is non-trivial.
5. Update L3/L2/L1 docs if files, modules, exports, or architecture changed.

Avoid:

- building future slices speculatively
- adding auth before the product loop exists
- styling all pages before data is real
- mixing provider credentials into code

## Phase 4: Verify

Run the cheapest reliable checks:

- typecheck/lint/test if configured
- local script for pipeline
- dev server and browser check for UI
- database migration dry run if available
- fixture snapshot for curation output

If a check cannot run, say why.

## Phase 5: Checkpoint

After each slice, report:

```markdown
## Slice Result
- Built:
- Verified:
- Evidence:
- Guesses:
- Bad smells:
- Next slice:
```

Ask the user to choose only when multiple product directions are genuinely different. Otherwise continue with the next obvious slice.

## Phase 6: Replace Mock With Real Provider

Provider replacement is allowed only after:

- the mock provider works
- schema stores raw source
- pipeline can process fixture data
- UI can display results
- credentials are available or the user authorizes a third-party route

Provider implementation must include:

- backoff for rate limits
- cursor/window tracking
- raw response storage
- dedupe by source id
- error logging
- disabled provider fallback

## Phase 7: Design Last

Use design extraction only after product behavior is proven.

Design work should preserve:

- information density
- source traceability
- scanability
- filters
- score visibility
- action recommendations

If the original visual design conflicts with the product goal, choose product clarity.
