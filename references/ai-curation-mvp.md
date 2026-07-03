# AI Curation MVP

[INPUT]: 依赖用户偏好、AI 信息源、Twitter/X 或替代 provider、LLM API、可运行 Web 项目。
[OUTPUT]: 提供 qunliao.vip/AIFeed 类 AI 内容策展系统的 MVP 数据模型、流程、页面要求和切片顺序。
[POS]: references 的垂直场景文件，专门服务 AI 信息策展/News Brief 产品复刻。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Product Goal

Build an AI information curation dashboard, not an AI news summary site.

The system should answer:

- What is worth consuming?
- Why is it worth my time?
- How should I consume it?
- Which source proves it?
- How does it match my taste?

Primary example:

```text
"This Karpathy 3.5 hour video is worth watching completely."
"For this Sam Altman talk, watch 20:21-31:52 for product roadmap; skim the rest."
"This tweet is ordinary second-hand news; skip it."
```

## MVP Slices

1. Mock Twitter posts.
2. Normalize posts into durable raw and processed records.
3. Classify content type and tags.
4. Score curation value against the user's taste.
5. Cluster posts into topic cards.
6. Generate one hourly brief.
7. Render `/news-briefs` with filters and expandable cards.
8. Add `SourceProvider` and `MockProvider`.
9. Add `XProvider` only after key/token is available.

## Recommended Stack

Use the existing repo stack when present. If starting clean:

- Next.js + React + TypeScript
- TailwindCSS
- SQLite for MVP; Postgres for production
- Drizzle or Prisma
- cron/background worker
- OpenAI-compatible LLM API
- provider interface for source ingestion

## Data Model

Use this as the first schema. Keep raw data.

```ts
type Source = {
  id: string
  platform: "x" | "rss" | "manual" | "apify"
  handle: string
  displayName: string
  category: "research" | "founder" | "product" | "open-source" | "investor" | "chinese-ai"
  priority: number
  language: "en" | "zh" | "mixed"
  enabled: boolean
}

type RawPost = {
  id: string
  platform: "x"
  postId: string
  authorName: string
  authorHandle: string
  authorProfileUrl: string
  postUrl: string
  text: string
  createdAt: string
  fetchedAt: string
  metrics: { likes?: number; reposts?: number; replies?: number; views?: number }
  mediaUrls: string[]
  externalUrls: string[]
  quotedPost?: unknown
  threadId?: string
  rawJson: unknown
}

type ProcessedPost = {
  id: string
  rawPostId: string
  contentType: ContentType
  language: "zh" | "en" | "other"
  extractedLinks: string[]
  summaryForIndexing: string
  importanceScore: number
  noveltyScore: number
  relevanceScore: number
  credibilityScore: number
  actionabilityScore: number
  finalScore: number
  recommendedAction: "watch_full" | "watch_segment" | "read_full" | "skim" | "save" | "skip"
  reason: string
  tags: string[]
}

type BriefCluster = {
  id: string
  title: string
  emoji: string
  tags: string[]
  takeaway: string
  whyItMatters: string
  recommendedAction: string
  score: number
  items: BriefItem[]
}

type BriefItem = {
  rawPostId: string
  sourceLabel: string
  sourceUrl: string
  curationNote: string
  recommendedAction: string
}
```

`ContentType`:

```text
model_release | product_launch | research_paper | open_source_project |
funding_company | technical_deep_dive | opinion_debate | video_podcast |
tutorial | benchmark | meme_low_value
```

## Provider Interface

Keep ingestion replaceable.

```ts
export interface SourceProvider {
  name: string
  fetchSince(input: {
    since: Date
    until: Date
    sources: Source[]
    query?: string
  }): Promise<RawPost[]>
}
```

Providers to support over time:

- `MockProvider`: local JSON fixtures
- `XApiProvider`: X API v2 Recent Search or Filtered Stream
- `RssProvider`: RSS feeds for blogs/arXiv/project updates
- `ApifyProvider`: third-party crawler when authorized
- `ManualUrlProvider`: user-provided tweet/post/video URLs

## Seed Sources

Start with config, not hardcoded code.

```yaml
english:
  - karpathy
  - sama
  - ylecun
  - AndrewYNg
  - darioamodei
  - gdb
  - demishassabis
  - fchollet
  - jeremyphoward
  - simonw
  - nearcyan
  - sholto
  - aidan_mclau
  - levelsio
  - rauchg
  - amasad
  - swyx
  - latentspacepod
  - lmsysorg
  - OpenAI
  - AnthropicAI
  - GoogleDeepMind
  - MistralAI
  - perplexity_ai
  - cursor_ai
  - huggingface
chinese:
  - dotey
  - op7418
  - Gorden_Sun
  - shao__meng
  - thinkingjimmy
  - vista8
  - lxfater
```

The system should later expand sources from:

- repeated retweets by seed accounts
- frequently cited authors
- repeated high-scoring source authors
- user saves/bookmarks

## User Taste

Default taste profile:

```yaml
prefer:
  - AI products
  - AI agents
  - builder/founder/hacker content
  - open-source tools
  - AI consumer products
  - agent workflow
  - model capability changes
  - Spark Lab / founder community relevance
avoid:
  - vague macro narratives
  - second-hand marketing accounts
  - Chinese summaries without source
  - generic "AI is changing everything" posts
reward:
  - first-hand source
  - author-tested evidence
  - engineering detail
  - product judgment
  - trend inflection
```

## `/news-briefs` UI

Information dashboard, not media homepage.

Top bar:

- Source: Twitter
- fetched count
- time range
- tag filters
- final score sort
- "only worth full consumption" toggle

Cluster card:

- emoji + title
- collapse/expand
- tags
- one sentence takeaway
- why it matters
- bullet items with source author and original link
- importance and recommendation
- score and reason visible

Design:

- small clear typography
- white cards
- light gray separators
- dense scanable layout
- source links obvious
- no oversized marketing hero

## Pipeline

```text
Fetch -> Normalize -> Classify -> Score -> Cluster -> Brief -> Render
```

### Normalize

- dedupe by platform + post id
- merge threads when evidence exists
- detect quote tweet
- expand or preserve shortened links
- extract GitHub/YouTube/arXiv/blog/product links
- detect language
- preserve raw text and raw JSON

### Score

Scores 0-100:

- importance
- novelty
- relevance_to_me
- credibility
- actionability
- final

`final_score` should not be likes-only. Use metrics as weak signal, not truth.

### Cluster

Group by topic and user value. A good cluster can have one very strong source or multiple reinforcing sources.

Cluster output:

- title
- emoji
- tags
- one_sentence_takeaway
- why_it_matters
- items
- recommended_action

## First Demo Acceptance

The mock MVP is acceptable when:

1. `/news-briefs` opens.
2. It shows one hourly Twitter AI brief.
3. It renders at least 4 clusters from mock posts.
4. Every item keeps original text and source URL.
5. Filters work.
6. Expand/collapse works.
7. Scores and reasons are visible.
8. Pipeline can run locally from mock JSON.
9. Provider interface exists.
10. Real X API can be added without rewriting UI or curation logic.
