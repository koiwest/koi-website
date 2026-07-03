# Curation Judgment Protocol

[INPUT]: 依赖 raw posts、用户品味、已归一化链接、目标产品的策展定义。
[OUTPUT]: 提供 Classifier、Scorer、Curator、Source Expansion 四个判断角色的输入输出契约。
[POS]: references 的 AI 判断层文件，被 AI 策展和类似 ranking/curation 产品读取。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Principle

This is not a prompt pack. It is a judgment pipeline.

The website is built step by step by separating roles:

```text
RawPost -> Classifier -> Scorer -> Curator -> Brief
                              |
                              v
                      Source Expander
```

Each role can be implemented by LLM, rules, human review, or a future model. Keep the IO contract stable.

## Global Rules

- Preserve `raw_post_id` and `source_url`.
- Do not invent sources, links, authors, timestamps, metrics, or video timestamps.
- Say `unknown` when evidence is missing.
- Optimize for consumption decision, not summary elegance.
- Keep reasons short enough to display in UI.

## Classifier Role

Purpose: convert raw/normalized posts into typed items.

Input:

```json
{
  "taste_profile": {},
  "posts": []
}
```

Output:

```json
{
  "items": [
    {
      "raw_post_id": "...",
      "language": "zh|en|other",
      "content_type": "model_release|product_launch|research_paper|open_source_project|funding_company|technical_deep_dive|opinion_debate|video_podcast|tutorial|benchmark|meme_low_value",
      "tags": ["..."],
      "is_low_value": false,
      "low_value_reason": "unknown|...",
      "extracted_links": ["..."],
      "summary_for_indexing": "one factual sentence"
    }
  ]
}
```

## Scorer Role

Purpose: score whether an item deserves the user's attention.

Rubric:

- `importance_score`: broad significance in AI/product/engineering
- `novelty_score`: new or under-discussed signal
- `relevance_score`: fit to user taste
- `credibility_score`: first-hand source, trusted author, concrete evidence
- `actionability_score`: can the user use/save/watch/build from it

Output:

```json
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

## Curator Role

Purpose: cluster high-signal items into a brief.

Rules:

- Prefer fewer stronger clusters.
- One excellent source can form a cluster.
- Preserve dissent or debate if useful.
- Mark skip-worthy items only if they explain what was filtered.
- Titles should be specific, not generic.

Output:

```json
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

## Source Expander Role

Purpose: recommend new sources from repeated high-value evidence.

Output:

```json
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

## Video Segment Hook

If a post points to a video/podcast and transcript is unavailable:

```json
{
  "time_guidance": "unknown",
  "needs_transcript": true,
  "transcript_provider_hint": "youtube_transcript|manual|future_provider"
}
```

Never invent timestamps without transcript evidence.
