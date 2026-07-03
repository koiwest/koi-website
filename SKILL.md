---
name: koi-website
description: Website product reconstruction skill for turning a reference website or product idea into a functional clone through evidence-first analysis, inferred feature mapping, incremental implementation slices, provider abstractions, mock-first demos, user checkpoints, and final UI polish. Use when the user asks to 复刻网站, clone/rebuild/reproduce a website, infer how a product works, build a functional copy from a reference URL, implement a website feature-by-feature, create an AI curation/news brief system, or build a qunliao.vip/AIFeed-like product.
---

# koi-website

Build the product behind a website, not just the screenshot.

The core rule: **function first, design last**. Infer the product, prove the data path, implement one runnable slice, ask for correction, then continue. A wrong early guess is acceptable; a large unverified rewrite is not.

## First Move

1. Read the nearest project `CLAUDE.md` before editing a repo. If missing, create it when the first architecture change happens.
2. If the request includes a reference URL, inspect it as evidence before proposing implementation.
3. If the user gives a target feature, start with that feature instead of the whole site.
4. Produce a small plan with:
   - what is known
   - what is guessed
   - what must be verified
   - the first runnable slice
   - files likely to change
5. Stop for user input only when credentials, legal authorization, paid API access, or product preference is required. Otherwise implement the first slice.

## What To Read

- Read `references/reconstruction-doctrine.md` when analyzing a reference product or deciding what "clone" means.
- Read `references/workflow.md` before starting implementation in a repo.
- Read `references/ai-curation-mvp.md` for qunliao.vip/AIFeed/Twitter AI brief/news curation systems.
- Read `references/prompt-pack.md` when designing classifier/scorer/curator LLM prompts.
- Read `references/validation.md` before reporting completion or moving to the next slice.

## Reconstruction Loop

For every product:

1. **Frame**: Name the user outcome, the business object, and the smallest valuable behavior.
2. **Recon**: Gather source evidence: UI, routes, network, public APIs, copy, screenshots, docs, repo search, and visible data model.
3. **Infer**: Build a feature map with confidence labels:
   - `SOURCE`: directly observed in source, network, docs, or running app
   - `PARTIAL`: supported by surface evidence but not fully verified
   - `GUESS`: plausible implementation choice
4. **Slice**: Choose one vertical slice that crosses UI, data, business logic, and persistence.
5. **Mock**: Use local JSON or seed data before real providers.
6. **Build**: Implement the slice in the existing stack when possible.
7. **Verify**: Run the app, test the path, and compare against acceptance criteria.
8. **Checkpoint**: Tell the user what is working, what was guessed, and what decision is next.
9. **Repeat**: Replace mocks with real providers, expand the feature set, then polish design.

## Product Decomposition

Do not start from visual components. Start from product objects:

- actor: who uses or produces the data
- source: where data enters
- object: durable domain entity
- action: what changes state
- judgment: scoring/ranking/classification rules
- view: how users inspect and decide
- feedback: user corrections that improve future runs

For a reference website, create this table before coding:

```markdown
| Feature | Evidence | Confidence | Core object | First slice | Needs user/API? |
|---|---|---:|---|---|---|
```

## Incremental Build Contract

Each step must end with a runnable state or a clearly named blocker. Prefer these slices:

1. project skeleton and architecture docs
2. mock data and schema
3. read-only page
4. local pipeline or job
5. persistence
6. provider abstraction
7. real provider
8. user feedback controls
9. visual polish
10. deployment/audit

For every slice, report:

```markdown
## Slice Result
- Built:
- Verified:
- Guesses:
- Blockers:
- Next slice:
```

## Hard Rules

- Preserve raw source data and source URLs. Never replace originals with AI output.
- Do not hardcode crawler/API logic into UI or business logic. Use provider interfaces.
- Do not treat AI summary as product value unless the user asked for summary. For curation products, optimize for "what deserves attention".
- Do not promise full clones of login, payment, permissions, recommendation systems, or proprietary APIs. Build demonstrable substitutes unless authorized.
- Do not use scraped content for model training. Use it only for retrieval, ranking, classification, curation, and display.
- Use official APIs first. If missing credentials, build mocks and the provider interface.
- If rate limited, back off and record the limit; do not bypass by abuse.
- Keep design polish after the functional loop is proven.

## Taste Check

Bad taste:
- one huge "clone the site" pass
- screenshot-first UI with no data model
- crawler code inside React components
- AI output without source links
- all content summarized into generic news
- no user checkpoint after a speculative inference

Good taste:
- vertical slice with real data shape
- raw source preserved
- provider interface before provider implementation
- classifier, scorer, curator split
- mock-first then credentials
- brief cards grounded in source links and recommended actions

[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md
