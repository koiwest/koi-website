# Phase 3 - Provider

[INPUT]: 依赖可运行 mock slice、provider interface、API 凭证、用户授权、rate limit 边界。
[OUTPUT]: 产出 LIVE_DATA_PATH：真实数据 provider、调度、错误处理、raw 数据保留。
[POS]: phases 的外部数据接入阶段，替换 mock 但不重写业务层。
[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md

## Goal

Replace mock input with real input without changing UI or curation logic.

## Provider Order

1. Official API.
2. RSS or public feeds.
3. Authorized third-party provider such as Apify.
4. Manual URL import.
5. Browser crawling only when authorized and compliant.

## Required Provider Behavior

- accepts `since` and `until`
- respects rate limits
- backoffs on 429
- stores raw JSON
- dedupes by platform + source id
- preserves source URL
- logs partial failure
- can be disabled without breaking the UI

## X/Twitter Provider Notes

Use X API v2 Recent Search or Filtered Stream when credentials exist.

If no key exists:

- keep `MockProvider`
- implement env/config placeholders
- document exact variables needed
- do not attempt abusive scraping

## Cron / Job Rules

- The hourly job should call provider -> normalize -> process -> persist -> brief.
- Jobs must be idempotent for a time window.
- Store `period_start` and `period_end`.
- Never train on fetched content.

## Stop Condition

Checkpoint after one real fetch window or after credential blocker is explicit.
