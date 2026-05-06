# Car News Radar Product Brief

## Problem Statement

Car-content operators need a high-signal page for auto, EV, smart-driving, and
automaker competition updates without manually visiting many vertical sites,
RSS readers, hot lists, and social feeds every day.

## Demand Evidence

- The account direction is more vertical and focused on car-related content.
- XiaoHongShu topic production needs a raw material pool before judgment,
  candidate selection, and final writing.
- Broad AI/news aggregators produce too much off-topic noise for car-content use.

## Status Quo

Useful signals are split across vertical auto media, company announcements,
financial/news aggregators, RSS readers, and platform hot lists. Broad
aggregators can look rich but often import politics, crypto, generic finance,
and generic supply-chain stories that do not help car-topic selection.

## Narrowest Useful Wedge

Keep the default page as a simple 24h car signal board:

- vertical auto RSS as the baseline
- OPML for private/custom source expansion
- a small hot-list layer after strict car-topic filtering
- visible source health so bad feeds are easy to spot

## Approaches Considered

### A: Reuse The AI Radar Source Mix

Fast, but it keeps too many AI and generic tech/news sources. The result is not
reliable enough for car-topic production.

### B: Convert To A Vertical Car Source Model

Use stable auto RSS first, then add broad hot-list sources only when the topic
filter is strict enough. This is the current approach.

### C: Build A Full Social/Platform Crawler

Useful later for XiaoHongShu, Dongchedi, Weibo, and automaker account tracking,
but it adds login, anti-bot, and maintenance cost. Keep this as a later private
adapter path.

## Recommended Approach

Use Approach B now. Treat the public site as the upstream car-news material pool.
Use platform-specific crawling and publishing workflows outside this repo until
they are stable enough to productize.

## Success Criteria

- First viewport clearly says this is a car-news signal board.
- Default data is mostly auto/EV/smart-driving/automaker content.
- Private OPML and credentials stay out of git.
- Future agents can route a new source into public default vs private/advanced.
