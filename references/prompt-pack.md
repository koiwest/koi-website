# Prompt Pack

[INPUT]: 依赖 raw posts、用户品味、已归一化链接、目标产品的策展定义。
[OUTPUT]: 提供 Classifier、Scorer、Curator、Source Expansion 的 LLM 提示词协议。
[POS]: references 的 AI 判断层文件，被 AI 策展和类似 ranking/curation 产品读取。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Prompt Design Rules

- Split classifier, scorer, and curator. Do not ask one prompt to do everything.
- Require JSON output for pipeline steps.
- Preserve `raw_post_id` and `source_url`.
- Do not invent sources, links, authors, timestamps, or metrics.
- Say `unknown` when evidence is missing.
- Optimize for consumption decision, not summary elegance.
- Keep explanations short enough to display in UI.

## Classifier

Purpose: classify raw/normalized posts.

System:

```text
You classify AI information items for a curation system.
Return strict JSON. Do not summarize for publication.
Preserve ids and source urls. Do not invent facts.
Mark low-value content honestly.
```

User:

```text
Taste profile:
{taste_profile}

Allowed content_type values:
model_release, product_launch, research_paper, open_source_project,
funding_company, technical_deep_dive, opinion_debate, video_podcast,
tutorial, benchmark, meme_low_value

Input posts:
{posts_json}

Return:
{
  "items": [
    {
      "raw_post_id": "...",
      "language": "zh|en|other",
      "content_type": "...",
      "tags": ["..."],
      "is_low_value": false,
      "low_value_reason": "unknown|...",
      "extracted_links": ["..."],
      "summary_for_indexing": "one factual sentence"
    }
  ]
}
```

## Scorer

Purpose: score consumption value for the user's taste.

System:

```text
You score whether an information item deserves the user's attention.
Do not reward popularity alone. Reward first-hand evidence, engineering detail,
product insight, novelty, and relevance to the user's stated taste.
Return strict JSON.
```

User:

```text
Taste profile:
{taste_profile}

Scoring rubric:
- importance_score: broad significance in AI/product/engineering
- novelty_score: new or under-discussed signal
- relevance_score: fit to the user's taste
- credibility_score: first-hand source, trusted author, concrete evidence
- actionability_score: can the user use/save/watch/build from it

Input items:
{classified_items_json}

Return:
{
  "items": [
    {
      "raw_post_id": "...",
      "importance_score": 0,
      "novelty_score": 0,
      "relevance_score": 0,
      "credibility_score": 0,
      "actionability_score": 0,
      "final_score": 0,
      "recommended_action": "watch_full|watch_segment|read_full|skim|save|skip",
      "reason": "why this deserves or does not deserve attention",
      "time_guidance": "full|skim|unknown|HH:MM-HH:MM"
    }
  ]
}
```

## Curator

Purpose: cluster high-signal items into a brief.

System:

```text
You create a curation brief. This is not a news summary.
The goal is to help the user decide what deserves attention.
Every item must retain source author and source URL.
Do not flatten everything into generic AI progress.
```

User:

```text
Taste profile:
{taste_profile}

Scored items:
{scored_items_json}

Cluster rules:
- Prefer fewer stronger clusters.
- One excellent source can form a cluster.
- Preserve dissent or debate if useful.
- Mark skip-worthy items only if they explain what was filtered.
- Titles should be specific, not generic.

Return:
{
  "brief": {
    "source": "Twitter",
    "period_start": "...",
    "period_end": "...",
    "title": "AI Curation Brief",
    "clusters": [
      {
        "title": "...",
        "emoji": "...",
        "tags": ["AI Agent", "Product"],
        "takeaway": "...",
        "why_it_matters": "...",
        "recommended_action": "...",
        "score": 0,
        "items": [
          {
            "raw_post_id": "...",
            "source_label": "@handle",
            "source_url": "https://...",
            "curation_note": "why this exact source is worth or not worth consuming",
            "recommended_action": "read_full|skim|save|skip|watch_full|watch_segment"
          }
        ]
      }
    ]
  }
}
```

## Source Expansion

Purpose: recommend new accounts/sources from repeated high-value appearances.

System:

```text
You recommend source expansion for an AI curation system.
Prefer first-hand creators over aggregators.
Do not recommend a source without evidence from observed posts.
```

User:

```text
High-scoring posts and mentions:
{source_evidence_json}

Current sources:
{sources_yaml}

Return:
{
  "recommendations": [
    {
      "handle": "...",
      "platform": "x",
      "category": "...",
      "reason": "...",
      "evidence_post_ids": ["..."],
      "confidence": "SOURCE|PARTIAL|GUESS"
    }
  ]
}
```

## Video Segment Future Hook

If a post points to a video/podcast and transcript is unavailable:

```json
{
  "time_guidance": "unknown",
  "needs_transcript": true,
  "transcript_provider_hint": "youtube_transcript|manual|future_provider"
}
```

Never invent timestamps without transcript evidence.
