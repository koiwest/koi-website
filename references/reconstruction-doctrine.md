# Reconstruction Doctrine

[INPUT]: 依赖 design-dna 的结构化抽取、web-clone 的证据纪律、用户目标中的功能复刻需求。
[OUTPUT]: 提供 koi-website 的复刻哲学、证据等级、强项、局限、执行边界。
[POS]: references 的总纲文件，被 SKILL.md 在产品分析和路径选择时读取。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Essence

`design-dna` is strong because it converts taste into schema.

`web-clone` is strong because it converts cloning into evidence.

`koi-website` must go one level deeper: convert a reference website into a sequence of **functional product slices**.

Formula:

```text
Product Clone = Evidence x Feature Map x Runnable Slices x User Correction
```

If any term is missing, the clone becomes theater:

- no evidence -> hallucination
- no feature map -> random pages
- no runnable slices -> plan without product
- no user correction -> wrong guesses compound

## Why This Is Powerful

Most agents clone the visible layer first. That fails for products because the value lives behind the screen:

- source acquisition
- normalization
- ranking
- scheduling
- persistence
- permission boundaries
- user preference
- feedback loops
- operational recovery

This skill forces the agent to infer those invisible systems, mark uncertainty, and build them gradually.

## Evidence Levels

Use these labels everywhere:

| Label | Meaning | Example |
|---|---|---|
| `SOURCE` | Direct evidence from source, network, docs, code, API response, screenshot, or running behavior | observed `/api/briefs` response |
| `PARTIAL` | Surface evidence supports the claim but implementation is not confirmed | UI suggests hourly jobs |
| `GUESS` | Plausible reconstruction choice | using SQLite for MVP |

Rules:

- Untagged claims count as `GUESS`.
- `GUESS` is allowed only if isolated behind an interface.
- User decisions can upgrade or reject guesses.
- Do not hide guesses in confident prose.

## What To Clone

Clone in this order:

1. **Core loop**: the repeated action that creates value.
2. **Data model**: objects that survive between sessions.
3. **Judgment function**: ranking, scoring, matching, filtering, or curation.
4. **Interaction model**: how users inspect, correct, save, or act.
5. **Operations**: cron, providers, retries, logs, audits.
6. **Design system**: final visual language and fidelity.

Design is not unimportant. It is just late. A polished empty surface is bad engineering.

## Typical Feature Map

```markdown
| Feature | Evidence | Confidence | Core object | First slice | Needs user/API? |
|---|---|---:|---|---|---|
| Hourly AI brief | UI period label | PARTIAL | brief | mock one brief from JSON | no |
| Twitter ingestion | source label Twitter | PARTIAL | raw_post | SourceProvider mock | API key later |
| Topic clustering | grouped cards | SOURCE | cluster | deterministic fixtures | no |
| Taste-aware scoring | user requirement | SOURCE | processed_post | scoring prompt + mock LLM | maybe |
```

## Good Reconstruction

Good reconstruction makes every future replacement easy:

- mock provider -> official API provider
- local JSON -> database
- rule scoring -> LLM scoring
- single route -> background job
- static account list -> source expansion
- rough dashboard -> polished UI

If replacing one layer forces rewriting unrelated layers, the design is wrong.

## Limits

Do not promise exactness for:

- private dashboards
- paid APIs
- login-only data
- recommendation algorithms
- proprietary ranking
- social graph internals
- copyrighted media libraries
- full payment/order/permission flows

Build a truthful substitute and mark the boundary.

## Known Bad Smells

- Three or more provider branches in business code.
- `fetchTwitter()` called from a React component.
- AI curation result without `source_url`.
- Scoring prompt that cannot explain why a user should spend time.
- Schema that stores only summary and discards raw text.
- Design polish before a working data pipeline.
- One giant commit that mixes crawler, database, UI, and styling.

When found, tell the user and propose the smallest repair.
