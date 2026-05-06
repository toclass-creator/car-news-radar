# Source Coverage Plan

This project should help auto/EV content operators scan high-signal car updates
without forcing them to follow noisy timelines or manage many source choices.

## Product Model

Use a two-layer model:

1. **Signal layer**: the default web UI. It should show curated auto/EV/smart-driving
   updates, search, site filtering, and a simple car-focused / all toggle.
2. **Advanced layer**: maintainer and power-user workflows. It includes OPML,
   source health data, GitHub Actions secrets, and custom fetchers.

Do not expose every source-management decision in the first screen. Too many
choices make the tool harder for new users to understand.
The first screen may show coverage and health as read-only status signals:
source health, car signal density, vertical RSS coverage, aggregator breadth,
and private extension readiness.

## V2 Coverage Claim

The project covers the common public paths for car news collection:

- vertical auto RSS/Atom feeds
- OPML collections for personal source lists
- public static pages with timestamps
- public hot lists after strict car-topic filtering
- optional secret-backed adapters for sources that need user-owned credentials

It does not promise reliable default coverage for private inboxes, cookies,
WeChat accounts, or raw social timelines. Those belong in the advanced layer or
private forks because they need credentials, bridges, or ongoing maintenance.

## Supported Source Types

| Source type | Current support | Recommended path | Notes |
| --- | --- | --- | --- |
| Auto RSS / Atom | Supported through built-in feeds and OPML | Keep stable vertical feeds built in; add private feeds to `feeds/follow.opml` locally, or `FOLLOW_OPML_B64` in GitHub Actions | Best default for personal customization. |
| Auto vertical updates | Built in for selected high-signal sources | Keep Gasgoo, CnEVPost, and CarNewsChina EV feeds as first-class sources | These should not depend only on aggregator coverage. |
| OPML collections | Supported | Export from RSS reader, copy from `feeds/follow.example.opml`, keep private file out of git | Good for cross-device and multi-agent workflows. |
| Public JSON APIs | Supported by custom Python fetchers | Add a `fetch_*` function in `scripts/update_news.py` and register it in the task list | Use only stable APIs with timestamps. |
| Public static pages | Supported by custom Python fetchers | Parse with `requests` + BeautifulSoup and normalize titles/URLs/times | Avoid fragile selectors when possible. |
| Newsletters | Partially supported | Prefer public archive RSS or stable archive pages | Use only if the source is car-focused. |
| X / Twitter | Supported only through curated central feeds or opt-in API adapters | Prefer generated feeds that already use official X API; keep direct X API optional and secret-backed | Following broad accounts often imports noise; public bridge routes can be unstable. |
| WeChat public accounts | Not recommended as a default | Use stable third-party RSS only if the maintainer accepts breakage risk | Login/copyright/bridge stability can be poor. |
| Telegram / Bilibili / Zhihu / podcasts | Skipped by default when feeds are unreliable | Add only as opt-in OPML entries | These can be noisy or bridge-dependent. |

## Source Selection Rules

Add a source only when it passes most of these checks:

- Publishes auto, EV, smart-driving, battery, charging, automaker, or car-buying updates with low noise.
- Has a stable URL, feed, API, or page structure.
- Provides usable timestamps or enough ordering information.
- Does not require private cookies, login sessions, browser automation, or secrets.
- Can be fetched politely by GitHub Actions without heavy rate limits.
- Adds coverage not already represented by stronger sources.

## Built-In Vertical Nodes

The public site should directly track these high-signal vertical sources:

- 盖世汽车新能源 RSS
- 盖世汽车智能网联 RSS
- 盖世汽车供应链 RSS
- CnEVPost China EV / Self Driving / Battery RSS
- CarNewsChina EV RSS

Aggregator sites may surface some of these updates, but they are not guaranteed
to be complete or timely. Keep vertical feeds as the stable baseline, then let
the aggregator layer add breadth.

## Built-In Aggregators

- **TopHub**: retained for broad Chinese hot-list monitoring after strict
  car-topic filtering.
- **NewsNow**: retained as a light broad-news supplement after strict car-topic
  filtering.

## Disabled Default Sources

- Broad global aggregators such as Buzzing, TechURLs, Iris, and BestBlogs are
  kept as historical fetchers in code but not registered as default car sources.
  In tests they import too much finance, geopolitics, generic supply-chain, and
  AI-tool noise for a car-focused public board.

## Personal Source Workflow

For a private custom setup:

1. Copy `feeds/follow.example.opml` to `feeds/follow.opml`.
2. Add official RSS/Atom feeds to `feeds/follow.opml`.
3. Run:

   ```bash
   python scripts/update_news.py --output-dir data --window-hours 24 --rss-opml feeds/follow.opml
   ```

4. Check `data/source-status.json` for `failed_feeds`, `zero_item_feeds`,
   `skipped_feeds`, and `replaced_feeds`.
5. Keep `feeds/follow.opml` private. For GitHub Actions, store its base64
   content in the `FOLLOW_OPML_B64` secret.

## Adding A Built-In Source

Use this only for sources that should benefit every public visitor:

1. Add a `fetch_<source_name>(session, now)` function in `scripts/update_news.py`.
2. Return `RawItem` objects with `site_id`, `site_name`, `source`, `title`, `url`,
   `published_at`, and `meta`.
3. Register the fetcher in the built-in task list.
4. Normalize URLs and dates using existing helpers.
5. Update this document if the source changes coverage.
6. Run the fastest relevant checks:

   ```bash
   python -m py_compile scripts/update_news.py
   pytest -q
   ```

## Deployment

The public deployment should remain GitHub Pages + GitHub Actions:

- GitHub Actions updates `data/*.json`.
- GitHub Pages serves `index.html` and `assets/*`.
- Private OPML input belongs in `FOLLOW_OPML_B64`, not in the repository.

This keeps the public version easy to fork while still letting each maintainer
bring their own private source list.
