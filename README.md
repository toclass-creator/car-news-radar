# Car Signal Board

24 小时车圈资讯雷达。用于聚合新能源、智能驾驶、车企竞争、补能、电池、供应链和购车风险相关信号。

线上页面：

- `https://toclass-creator.github.io/car-news-radar/`

## 定位

这个仓库是从 `ai-news-radar` 复制出的车圈版本。

它的目标不是做泛汽车门户，而是给小红书车圈内容生产提供上游情报池：

- 看今天车圈发生了什么。
- 看哪些品牌、技术路线、购车风险在反复出现。
- 给晚九双稿生产提供更稳定的候选素材。

## 默认覆盖

内置垂类源：

- 盖世汽车-新车
- 盖世汽车-行业
- 盖世汽车-车企
- 盖世汽车-销量
- 盖世汽车-新技术
- 盖世汽车-新能源
- 盖世汽车-智能网联
- 盖世汽车-供应链
- 盖世汽车-上市公司
- CnEVPost China EV
- CnEVPost Self Driving
- CnEVPost Battery
- CarNewsChina EV

同时保留少量公开聚合源：

- TopHub：只保留汽车垂类来源或明确车圈关键词
- NewsNow：作为轻量补充，经过车圈关键词过滤

已从默认抓取中移除 `TechURLs`、`Buzzing`、`Info Flow`、`BestBlogs` 等泛资讯源，避免把 AI 工具、泛财经、地缘政治和通用供应链新闻带进车圈视图。

可选 OPML：

- `feeds/follow.opml`
- GitHub Actions Secret：`FOLLOW_OPML_B64`

## 本地运行

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python scripts/update_news.py --output-dir data --window-hours 24 --archive-days 21 --rss-opml feeds/follow.opml
python -m http.server 8080
```

打开：

```text
http://localhost:8080
```

## GitHub Pages

进入仓库：

`Settings` -> `Pages`

选择：

- Source: `Deploy from a branch`
- Branch: `main`
- Folder: `/root`

保存后页面地址是：

```text
https://toclass-creator.github.io/car-news-radar/
```

## GitHub Actions

工作流：

```text
.github/workflows/update-news.yml
```

默认每 30 分钟更新一次 `data/*.json` 并提交。

## 数据输出

- `data/latest-24h.json`：车圈精选视图
- `data/latest-24h-all.json`：全量视图
- `data/archive.json`：归档缓存
- `data/source-status.json`：源健康状态
- `data/title-zh-cache.json`：英文标题翻译缓存

每条内容会输出 `topic_tags`，用于前台筛选和下游小红书选题：

- 新能源
- 智能驾驶
- 补能电池
- 销量市场
- 价格战
- 召回安全
- 车企竞争
- 供应链
- 购车决策
- 新车上市

## 后续接 automation-2

晚九双稿可以优先读取：

- `data/latest-24h.json`
- 后续可新增 `data/xhs-candidates-24h.json`

建议流程：

```text
car-news-radar 全天聚合 -> automation-2 晚九筛选 -> Obsidian 终稿 -> 小红书发布
```
