# GPT Handoff: Car News Radar

Use this document to hand the project to a fresh GPT/Codex/Claude session for
verification.

## Local Project

- Local path: `/Users/alienmacmini/Downloads/ai-news-radar-master`
- Repository: `https://github.com/toclass-creator/car-news-radar`
- Public site: `https://toclass-creator.github.io/car-news-radar/`

## What This Project Is

Car News Radar / Car Signal Board is a static car-news signal page:

- readers open the hosted page and scan 24h auto, EV, smart-driving, charging,
  battery, supply-chain, automaker, and car-buying updates
- maintainers can fork it and add private OPML/RSS via GitHub Secrets
- content production can use it as an upstream source pool before selecting
  XiaoHongShu topics

## Current Source Model

Default public sources:

- built-in vertical RSS feeds from Gasgoo, CnEVPost, and CarNewsChina EV
- OPML RSS when configured locally or through `FOLLOW_OPML_B64`
- TopHub and NewsNow as limited broad supplements after strict car-topic filtering

Each output item includes `topic_tags` for downstream topic selection. Current
tags include EV, smart driving, charging/battery, sales/market, price war,
recall/safety, automaker competition, supply chain, purchase decision, and new
car launch themes.

Disabled as default:

- Buzzing, TechURLs, Iris, BestBlogs, Follow Builders, AI Breakfast, Zeli, and
  other AI-oriented or broad aggregators
- X, email inboxes, WeChat, cookies, and login-bound bridges

## Validation Checklist

1. Reader path: Can a normal user understand that the page is for car signals,
   not AI news?
2. Fork path: Can a GitHub user understand how to enable Pages/Actions and
   optionally add OPML through `FOLLOW_OPML_B64`?
3. Source quality: Does the default feed avoid generic finance, politics,
   generic supply-chain, crypto, and AI-tool noise?
4. Safety: Are private OPML files, tokens, cookies, inboxes, and API keys kept
   out of the repository?
5. Maintenance: Are validation commands and source-intake rules clear enough for
   a future agent to add or reject new car sources?

## Suggested Prompt For New GPT

```text
你现在接手本地项目：
/Users/alienmacmini/Downloads/ai-news-radar-master

请先阅读：
README.md
AGENTS.md
docs/SOURCE_COVERAGE.md
docs/V2_PRODUCT_BRIEF.md

任务：
1. 判断这个汽车资讯聚合项目是否已经达到可公开发布状态。
2. 分别从普通读者、fork 用户、内容生产用户三个角度验收 README 和项目结构。
3. 检查是否有隐私/密钥/OPML 泄漏风险。
4. 检查信息源覆盖是否诚实：哪些是公共默认源，哪些只是高级/私有路径。
5. 如果发现问题，请按严重程度列出具体文件和建议修复方式。

不要直接重构。先做验收报告。
```
