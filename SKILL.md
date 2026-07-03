---
name: koi-website
description: Website product reconstruction and incremental builder. Use when the user asks to 复刻网站, clone/rebuild/reproduce a website, infer how a product works, build a functional copy from a reference URL, implement a website feature-by-feature, run sub-agent/product research for a web product, create a qunliao.vip/AIFeed-like AI curation/news brief system, or build a mock-first MVP that later connects real providers/APIs.
---

# koi-website

Build the product behind a website, not just the screenshot.

Core rule: **function first, design last**. Infer the product, prove the data path, implement one runnable slice, ask for correction, then continue.

## Routing

Default route: run all phases until the first meaningful checkpoint, then continue slice by slice.

Flags:

- `/koi-website`: full reconstruction workflow
- `/koi-website --recon <url>`: Phase 0 only, product archaeology and feature map
- `/koi-website --plan`: Phase 1 only, architecture and slice plan
- `/koi-website --slice <name>`: Phase 2 only, implement one mock-first vertical slice
- `/koi-website --provider <name>`: Phase 3 only, replace a mock provider with a real provider
- `/koi-website --polish`: Phase 4 only, design, QA, audit, and ship readiness
- `/koi-website --resume <brief-or-log>`: continue from an existing product brief or slice log

## Execution Files

Read and execute phase files when entering that phase:

- `phases/phase-0-recon.md`: infer the product from URL, repo, screenshots, network, docs, and user demand.
- `phases/phase-1-architecture.md`: turn recon into architecture, schema, provider boundaries, and vertical slices.
- `phases/phase-2-mock-slice.md`: build one runnable mock-first feature slice.
- `phases/phase-3-provider.md`: connect real APIs, cron jobs, queues, crawlers, or third-party providers.
- `phases/phase-4-polish-ship.md`: add visual fidelity, QA, audit, docs, and deployment readiness.

Reference files are loaded only when needed:

- `references/reconstruction-doctrine.md`: philosophy, evidence levels, strengths, limits.
- `references/subagent-protocol.md`: how to split research/building across sub-agents when authorized and available.
- `references/ai-curation-mvp.md`: qunliao.vip/AIFeed/Twitter AI curation product spec.
- `references/curation-judgment-protocol.md`: classifier/scorer/curator roles and output contracts.
- `references/validation.md`: completion gates and bad smell audit.

## Core Workflow

```
Phase 0: Recon          -> PRODUCT_BRIEF
Phase 1: Architecture   -> SLICE_PLAN
Phase 2: Mock Slice     -> RUNNABLE_MVP_SLICE
Phase 3: Real Provider  -> LIVE_DATA_PATH
Phase 4: Polish + Ship  -> VALIDATED_PRODUCT
```

Each phase must end with:

```markdown
## Phase Result
- Built / learned:
- Evidence:
- Guesses:
- Files changed:
- Verified:
- Next decision:
```

## Hard Rules

- Preserve raw source data and source URLs. Never replace originals with AI output.
- Use `SOURCE` / `PARTIAL` / `GUESS` labels for product inferences.
- Isolate guesses behind interfaces so they can be replaced.
- Do not hardcode crawler/API logic into UI or business logic.
- Use mock data before credentials or paid providers.
- Split AI judgment into roles such as classifier, scorer, curator, and source expander.
- Do not make a visual clone before the product loop works.
- Ask the user only for credentials, legal authorization, paid APIs, destructive operations, or taste decisions that change scoring.
- If sub-agent tools are available and the user asked for parallel/sub-agent work, use `references/subagent-protocol.md`; otherwise perform the same roles sequentially.

## Taste Check

Bad taste:

- one giant "clone the site" pass
- screenshot-first UI with no data model
- `fetchTwitter()` inside React components
- AI brief with no source links
- all content reduced to generic summary
- no checkpoint after speculative inference

Good taste:

- staged phase files
- evidence table before implementation
- vertical slice before full product
- raw data stored beside processed data
- provider interface before provider implementation
- mock-first demo, real provider second
- user correction loop after each slice

[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md
