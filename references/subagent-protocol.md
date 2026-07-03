# Sub-Agent Protocol

[INPUT]: 依赖用户授权、可用 sub-agent 工具、PRODUCT_BRIEF 或目标 URL、当前 repo。
[OUTPUT]: 提供网站复刻过程中的子代理分工、并行规则、合并规则和禁止事项。
[POS]: references 的协作调度文件，被 Phase 0、Phase 1 和复杂实现阶段读取。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Principle

Sub-agents are for independent pressure, not for outsourcing responsibility.

The main agent owns:

- final architecture
- file edits integration
- product judgment
- user checkpoint
- verification report

Sub-agents supply bounded evidence or disjoint implementation.

## When To Use

Use sub-agents only when:

- the user explicitly asked for sub-agent / parallel work, or approved it
- sub-agent tools are available
- tasks are independent
- the main agent can continue useful work while they run

Do not use sub-agents when:

- the immediate next step is blocked on their answer
- the task is tiny
- write scopes overlap
- credentials or legal authorization are unresolved

## Phase 0 Research Agents

Run in parallel when authorized:

### Agent A - Product Archaeologist

Scope:

- visible UI
- routes
- user actions
- page objects
- product loop

Output:

```markdown
| Observation | Evidence | Confidence | Product implication |
|---|---|---|---|
```

### Agent B - Data Detective

Scope:

- network/API hints
- source labels
- provider assumptions
- durable data objects
- raw vs processed data

Output:

```markdown
| Data object | Evidence | Confidence | Suggested schema |
|---|---|---|---|
```

### Agent C - UX Cartographer

Scope:

- information architecture
- filters
- cards/tables/forms
- interaction states
- empty/loading/error states

Output:

```markdown
| View | State | User decision supported | Missing behavior |
|---|---|---|---|
```

## Phase 1 Planning Agents

Use only for non-overlapping planning:

- Architecture Scout: existing repo structure and minimal files.
- Domain Modeler: objects and relationships.
- Verification Planner: tests and runnable checks.

## Phase 2+ Worker Agents

Use workers only when write scopes are disjoint:

- Worker 1 owns data/pipeline files.
- Worker 2 owns UI route/components.
- Worker 3 owns tests/fixtures.

Tell every worker:

- other agents may edit the codebase
- do not revert unrelated changes
- list changed files
- keep the assigned write scope
- leave verification command output

## Merge Rule

Main agent merges by evidence, not by vote.

If agents conflict:

1. prefer `SOURCE` over `PARTIAL`
2. prefer `PARTIAL` over `GUESS`
3. preserve unresolved conflict in the checkpoint
4. ask user only if the conflict changes product direction

## User-Facing Progress

When sub-agents are running, tell the user:

```markdown
I split this into:
- Product Archaeologist:
- Data Detective:
- UX Cartographer:

I am continuing locally on:
```

Do not wait idly unless blocked.
