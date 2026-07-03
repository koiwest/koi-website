# Validation

[INPUT]: 依赖实现后的 repo、运行命令、验收标准、证据等级和用户目标。
[OUTPUT]: 提供切片完成判断、报告模板、坏味道检查和停止条件。
[POS]: references 的验收闸门文件，被 SKILL.md 在汇报完成前读取。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Completion Rule

A slice is complete only if:

1. It runs or the blocker is explicit.
2. The user-visible behavior is described.
3. Tests/checks were run or skipped with reason.
4. Raw sources remain traceable.
5. Architecture docs are consistent with changed files.
6. Guesses are named.

## Verification Ladder

Use the strongest practical check:

1. Unit/integration tests.
2. Local pipeline command with fixtures.
3. Typecheck/lint.
4. Browser interaction.
5. Screenshot/visual inspection.
6. Static review only, with risk stated.

Do not claim verified behavior from static review.

## Report Template

```markdown
## Slice Result
- Built:
- Verified:
- Files changed:
- Evidence:
- Guesses:
- Bad smells:
- Next slice:
```

For AI curation:

```markdown
## Curation Demo Check
- Raw posts preserved:
- Source URLs visible:
- Classifier output:
- Scorer reasons:
- Cluster count:
- Filters:
- Expand/collapse:
- Provider boundary:
```

## Bad Smell Audit

Check before final response:

- crawler/provider code imported into UI components
- no provider interface
- no mock data path
- no raw data storage
- AI result lacks source URL
- scores lack reasons
- all final scores clustered near 80
- summaries replace curation notes
- visual polish hides missing function
- architecture docs stale
- more than three nested branches in new logic
- one file over 800 lines
- one folder layer over 8 files without subdivision

If found, report it and either fix it or ask whether to optimize.

## When To Stop And Ask

Stop only for:

- missing required API credentials
- paid API or third-party crawler cost
- legal/authorization uncertainty
- ambiguous product decision with real tradeoff
- destructive migration or data deletion
- user taste conflict that changes scoring behavior

Do not stop for:

- choosing SQLite vs local JSON for mock MVP
- writing a provider interface
- creating fixtures
- implementing read-only UI
- adding architecture docs

## Acceptance For `/news-briefs`

The first MVP passes when:

- `/news-briefs` opens locally
- one hourly brief is visible
- top bar shows source, count, and time range
- tag filters work
- "only full consumption" filter works
- each cluster can collapse and expand
- source links open original URLs
- every item displays recommendation and reason
- mock pipeline regenerates the brief
- real provider can be added without changing UI components

## Design Acceptance

Design is accepted only after function acceptance.

It should be:

- dense
- quiet
- scanable
- source-forward
- low ornament
- no marketing hero
- no decorative cards inside cards
- no huge type inside compact dashboards
