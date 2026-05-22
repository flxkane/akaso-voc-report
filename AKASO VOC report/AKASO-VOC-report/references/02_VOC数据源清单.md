# VOC 数据源清单

> 这是一份持续维护的活文档，记录 VOC 监控平台所有已接入和计划接入的数据源。
>
> **主用采集工具：** Bright Data MCP（Pro 模式，有使用次数上限）
> **备选体系：** 当 Bright Data 配额耗尽或不可用时，每个数据源都必须有**至少 1 个免费替代方案**（详见下方「免费备选工具体系」与各数据源详情卡片）
> **当前阶段：** 一次性报告产出（目标：演进为周期性自动监控平台）

---

## 目标产品配置（每次执行前必须填写）

> ⚠️ **重要：** 本清单中所有出现「AKASO 360」「EK7000」「V50 Pro」等具体型号的位置**仅为示例**，不代表当前报告对象。每次执行 VOC 采集前，必须填写下表，并将所有 `{产品型号}` 占位符替换为实际目标 SKU。

### 当前报告对象（执行前由用户指定）

| 字段 | 当前值 | 说明 |
|-----|--------|------|
| **{产品型号}** | _待用户指定_ | 本期 VOC 的核心目标 SKU（如 AKASO EK7000 / Brave 8 / V50X / Brave 7 LE 等） |
| **{产品系列}** | _待用户指定_ | 如 入门 4K / 旗舰防抖 / 360 全景 |
| **{价位段}** | _待用户指定_ | 如 $59-89（入门） / $199-249（中端） |
| **{核心销售市场}** | _待用户指定_ | 如 US / UK / DE / JP（决定采集站点） |
| **{对比竞品列表}** | _待用户指定_ | 如 GoPro Hero 13、Insta360 X4、DJI Osmo Action 5 |
| **{报告周期}** | _待用户指定_ | 如 2026 Q2 |
| **{ASIN 列表}** | _见附录_ | 在文档末附录补全实际 ASIN |

### 填写示例（仅供参考）

```yaml
产品型号: AKASO EK7000
产品系列: 入门 4K 运动相机
价位段:   $59-89
市场:     US, UK, DE
竞品:     GoPro Hero 13 (B0DBPTYM81), Insta360 X4, DJI Osmo Action 5 Pro
报告周期: 2026 Q2
```

填写完成后，下文凡是出现 `{产品型号}` 的位置统一以实际 SKU 替换。

---

## 采集工具备选体系（双层架构）

> **核心原则：每个数据源必须有「主用 + 备选」两套工具。** Bright Data 是主用工具但有配额；当配额耗尽、Token 失效或返回异常时，按"免费备选"顺序切换。

### 工具分层定义

| 层级 | 工具类型 | 特征 | 适用场景 |
|-----|---------|------|---------|
| **L1 主用** | Bright Data MCP（Pro）| 速度快、反爬强、有配额限制 | 日常采集 |
| **L2 免费 API** | 各平台官方免费 API / RSS | 免费但有速率限制 | Bright Data 不可用时的首选 |
| **L3 开源库** | 社区开源 scraper（PRAW / yt-dlp / google-play-scraper 等）| 完全免费，需自部署 Python/Node | API 不开放或数据不全时 |
| **L4 LLM 友好读取** | `r.jina.ai` 前缀 / WebFetch 工具 | 免费、无需鉴权、返回 Markdown | 静态页面快速抓取 |
| **L5 浏览器自动化** | Playwright / Puppeteer / Claude in Chrome | 最通用但慢 | 强反爬 / JS 渲染必须时 |

### 各平台免费备选工具速查表

| 平台 | L2 免费 API | L3 开源库 | L4 LLM 读取 | L5 浏览器自动化 |
|-----|------------|----------|-------------|---------------|
| **Amazon** | ❌ 无官方公开 API | `scrapy-amazon`、`amazon-paapi`（需账号） | `https://r.jina.ai/<URL>` | Playwright + 代理池 |
| **Reddit** | ✅ **官方 API（推荐）** — 100 req/min OAuth；URL 后缀 `.json` 免登录 | `PRAW`（Python 官方）| `r.jina.ai/<reddit URL>` | Playwright |
| **YouTube** | ✅ **Data API v3** — 每天 10000 单位配额（免费） | `yt-dlp --write-comments`（开源）| `r.jina.ai/<youtube URL>` 仅页面文本 | Playwright |
| **Instagram** | ❌ Basic Display API 仅限自有账号 | `instaloader`（限速严重）| `r.jina.ai` 通常被拦 | **首选**：Claude in Chrome |
| **TikTok** | ⚠️ Research API 需学术资质 | `TikTokApi`（不稳定）、`tiktok-scraper` | r.jina.ai 部分可行 | **首选**：Claude in Chrome |
| **X（Twitter）** | ⚠️ 免费层 1500 推/月仅自己 | `twscrape`（需账号池）、Nitter 实例 | `nitter.net/<user>` 可读 | Playwright |
| **Apple App Store** | ✅ **iTunes RSS** — `itunes.apple.com/rss/customerreviews/...`（免费官方）| `app-store-scraper`（npm，开源）| `r.jina.ai` 部分页面可读 | Playwright |
| **Google Play** | ⚠️ Play Console 仅限自有 App | `google-play-scraper`（npm，开源，**推荐**）| `r.jina.ai/<play URL>` | Playwright |
| **专业评测站** | 通常无 API | requests + BeautifulSoup | `r.jina.ai/<URL>`（最优）| Playwright |
| **Google 搜索** | ⚠️ Custom Search API 100/天免费 | `googlesearch-python`、`duckduckgo-search` | — | Playwright |

### 免费备选切换决策树

```
                  Bright Data 可用？
                       │
              ┌────────┴────────┐
              是                否
              │                 │
            主用                有 L2 官方免费 API？
                                 │
                        ┌────────┴────────┐
                       是                  否
                       │                   │
                  L2 API               有 L3 开源库？
                                          │
                                  ┌───────┴───────┐
                                 是                否
                                 │                 │
                              L3 开源库         L4 r.jina.ai 可读？
                                                   │
                                          ┌────────┴────────┐
                                         是                  否
                                         │                   │
                                    L4 WebFetch        L5 浏览器自动化
```

---

## Bright Data MCP 工具可用状态

> 以下为 Bright Data MCP 各工具的可用状态，供采集时直接参考。

### ✅ 已验证可用工具

| 工具名称 | 用途 | 返回格式 | 备注 |
|---------|------|---------|------|
| `search_engine` | 搜索目标 URL、发现数据源 | 结构化 JSON（标题/链接/摘要）| 支持 Google / Bing / Yandex，支持地区参数 `geo_location` |
| `web_data_reddit_posts` | 抓取 Reddit 帖子及评论 | 结构化 JSON | 返回帖子正文、所有评论、点赞数、关联帖子，质量高 |
| `web_data_youtube_comments` | 抓取 YouTube 视频评论区 | 结构化 JSON | 返回评论内容、点赞数、用户名、发布时间，支持 `num_of_comments` 参数 |
| `scrape_as_markdown` | 通用网页抓取（含反爬绕过）| Markdown 原文 | 可绕过 Amazon、Instagram 等强反爬站点 |
| `scrape_as_html` | 通用网页抓取（返回原始 HTML）| HTML 原文 | 适合需要精确解析 DOM 结构的场景 |
| `scrape_batch` | 批量抓取多个 URL | Markdown 列表 | 一次最多 10 个 URL |
| `search_engine_batch` | 批量搜索 | JSON 列表 | 一次最多 10 条查询 |

### ❌ 未激活（付费数据集，当前不使用）

| 工具名称 | 原因 | 替代方案 |
|---------|------|---------|
| `web_data_amazon_product_reviews` | 账户未激活（`HTTP 400`）| 主用：`scrape_as_markdown`；备选：见 S1 免费工具栏 |
| `web_data_apple_app_store` | 未激活 | 主用：`search_engine`+`scrape_as_markdown`；备选：**iTunes RSS（免费官方）** |
| `web_data_google_play_store` | 未激活 | 主用：`search_engine`+`scrape_as_markdown`；备选：**google-play-scraper（开源）** |

---

## 数据源状态说明

| 状态标签 | 含义 |
|---------|------|
| ✅ 已接入 | 工具已验证可用，可直接调用 |
| ❌ 未激活 | 付费工具未开通，当前用替代方案 |
| ⚠️ 待排查 | 工具存在但返回异常，有替代方案 |
| 📋 计划中 | 已列入采集计划，尚未实施 |

---

## 第一类：电商平台评论

### S1 亚马逊产品评论

**数据价值：** VOC 核心数据源，买家经过购买验证，反馈最真实，结构最规范
**采集状态：** ✅ 已接入（使用替代工具）
**优先级：** P0

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `scrape_as_markdown`（Bright Data，已验证）|
| **L2 免费 API** | ❌ Amazon 无官方公开评论 API |
| **L3 开源库备选** | `scrapy-amazon-reviews`（Python，需配代理池避免封 IP） / `amazon-paapi`（需 Affiliate 账号，只返回摘要不返回评论全文） |
| **L4 LLM 读取备选** | **`https://r.jina.ai/https://www.amazon.com/dp/[ASIN]`** —— 免费、无需鉴权、返回 Markdown，**推荐作为 Bright Data 配额耗尽时的首选** |
| **L5 浏览器自动化** | Playwright + 住宅代理 + 随机化 UA（最稳定但最慢） |
| 可获取字段 | 评分（1-5星）、评论正文、评论日期、Verified Purchase 标签、Helpful 票数、评论者地区 |
| 支持站点 | 美国 / 英国 / 德国 / 法国 / 日本 / 加拿大 / 澳大利亚 等 |
| 采集范围 | `{产品型号}` 全系 SKU + `{对比竞品列表}` 对应 SKU |
| 备选切换信号 | Bright Data 返回 429/配额警告 → 切 L4 `r.jina.ai` |
| VOC 对应维度 | D1 D2 D3 D4 D7 D8 D9 D10 D12 |

**备注：** 实测 `r.jina.ai` 对 Amazon 商品页返回质量良好，免费版有速率限制（约每分钟 20 次）。采集时建议同步拉取 `{产品型号}` + 竞品 SKU。ASIN 列表见附录。

---

### S2 亚马逊产品 Q&A

**数据价值：** 反映购买前的信息缺口和决策障碍，是预期落差分析（D7）的原始材料
**采集状态：** ✅ 已接入
**优先级：** P1

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `scrape_as_markdown`（Bright Data，按产品页 Q&A 区抓取）|
| **L2 免费 API** | ❌ 无 |
| **L4 LLM 读取备选** | `https://r.jina.ai/https://www.amazon.com/ask/questions/asin/[ASIN]` |
| **L5 浏览器自动化** | Playwright（Q&A 区往往需要滚动加载，建议执行 `scroll-to-bottom`）|
| 可获取字段 | 问题内容、官方/买家回答、问题日期、Helpful 票数 |
| 采集范围 | `{产品型号}` 全系 SKU |
| 备选切换信号 | 同 S1 |
| VOC 对应维度 | D7 D4 D5 |

---

## 第二类：社交媒体

### S3 Reddit

**数据价值：** 深度讨论质量高，用户会详细描述使用场景和问题复现步骤，变通行为（D6）信息最丰富
**采集状态：** ✅ 已接入
**优先级：** P0

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `web_data_reddit_posts`（Bright Data）|
| **L2 免费 API** | ✅ **Reddit 官方 API（推荐备选）** — OAuth 后 100 req/min，免费且稳定；或直接 `https://www.reddit.com/r/<sub>/.json` 免登录访问 |
| **L3 开源库备选** | `PRAW`（Python Reddit API Wrapper，官方推荐）|
| **L4 LLM 读取备选** | `https://r.jina.ai/https://www.reddit.com/r/ActionCameras/...` |
| **L5 浏览器自动化** | Playwright（最后兜底） |
| 可获取字段 | 帖子标题、正文、评论列表、点赞数、子版块、发布时间 |
| 重点子版块 | r/ActionCameras / r/GoPro / r/Cameras / r/videography / r/360cameras |
| 关键词 | `"{产品型号}"` / `"{产品系列}"` / `"action camera"` |
| 备选切换信号 | Bright Data 配额耗尽 → 直接切 L2 官方 API（最稳） |
| VOC 对应维度 | D2 D4 D5 D6 D9 D12 |

**备注：** Reddit 是所有平台里**免费备选最强**的——L2 官方 API 完全可以替代 Bright Data，建议作为长期主备双轨。

---

### S4 YouTube 视频评论

**数据价值：** 评测视频下的评论质量极高，用户会直接对比竞品、提出具体改进意见
**采集状态：** ✅ 已接入
**优先级：** P1

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `web_data_youtube_comments`（Bright Data）|
| **L2 免费 API** | ✅ **YouTube Data API v3** — 每天 10000 单位配额；读取评论约 1 单位/请求（即每天可读约 1 万次）|
| **L3 开源库备选** | `yt-dlp --write-comments`（完全免费，无配额限制，速率自调）|
| **L4 LLM 读取备选** | `r.jina.ai/<youtube URL>` 仅能读到页面元信息和顶部评论 |
| **L5 浏览器自动化** | Playwright（需展开评论区）|
| 可获取字段 | 评论正文、点赞数、回复数、发布时间 |
| 采集范围 | ① `{产品型号}` 官方视频评论区  ② 第三方评测视频（搜索 `"{产品型号} review"`）③ 竞品评测视频评论区 |
| 备选切换信号 | Bright Data 配额耗尽 → 优先切 L3 `yt-dlp`（完全免费且数据更全） |
| VOC 对应维度 | D2 D3 D6 D7 D12 |

**备注：** `yt-dlp` 是 YouTube 数据采集的**事实标准开源工具**，应作为长期备选。`{对比竞品列表}` 评测视频的评论区是隐藏金矿。

---

### S5 Instagram

**数据价值：** 视觉内容为主，适合分析使用场景（D5）和用户画像（D10），评论相对简短
**采集状态：** ✅ 已接入
**优先级：** P2

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `web_data_instagram_posts` / `web_data_instagram_comments`（Bright Data）|
| **L2 免费 API** | ❌ Instagram Basic Display API 仅限读取自有账号 |
| **L3 开源库备选** | `instaloader`（开源 Python，**速率限制严重**，约每小时 200 次后被封） |
| **L4 LLM 读取备选** | `r.jina.ai` 通常被 Instagram 反爬拦截 |
| **L5 浏览器自动化** | **首选**：Claude in Chrome 手动登录后按需采集（最稳定）|
| 可获取字段 | 帖子描述、评论、点赞数、标签、发布时间 |
| 采集范围 | `#{产品型号}` `#{产品系列}` `#actionsports` `#actioncamera` 相关帖子 |
| 备选切换信号 | Bright Data 配额耗尽 → 切 L5 Claude in Chrome（Instagram 免费方案普遍不稳定）|
| VOC 对应维度 | D5 D10 |

**备注：** Instagram 反爬最严格，免费备选普遍不稳定，建议把 Instagram 列入"低优先级、低频率"采集对象。

---

### S6 TikTok

**数据价值：** 真实使用场景展示，评论区有大量用户对产品的直接反馈
**采集状态：** ✅ 已接入
**优先级：** P1

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `web_data_tiktok_posts` / `web_data_tiktok_comments`（Bright Data）|
| **L2 免费 API** | ⚠️ TikTok Research API 仅限学术资质 |
| **L3 开源库备选** | `TikTokApi`（Python，社区维护，常因协议变化失效）、`tiktok-scraper`（Node）|
| **L4 LLM 读取备选** | r.jina.ai 对 TikTok 帖子可读，但评论不完整 |
| **L5 浏览器自动化** | **首选**：Claude in Chrome（最稳定）|
| 可获取字段 | 视频描述、评论、点赞数、分享数、标签 |
| 采集范围 | `#{产品型号}` `#actionsports` `#actioncamera` 相关视频评论 |
| 备选切换信号 | Bright Data 配额耗尽 → L3 开源库（先尝试，不稳就切 L5）|
| VOC 对应维度 | D1 D5 D10 |

---

### S7 X（Twitter）

**数据价值：** 实时反馈，适合捕捉产品发布节点或负面事件爆发时的用户情绪
**采集状态：** ✅ 已接入
**优先级：** P2

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `web_data_x_posts`（Bright Data）|
| **L2 免费 API** | ⚠️ X 免费层每月 1500 推文且仅限自己账号 |
| **L3 开源库备选** | `twscrape`（需自备账号池）|
| **L4 LLM 读取备选** | **Nitter 实例**（如 `nitter.net/<user>`）—— 第三方免登录前端，可被 `r.jina.ai` 读取 |
| **L5 浏览器自动化** | Playwright |
| 可获取字段 | 推文内容、转发数、点赞数、回复数、发布时间 |
| 关键词 | `"{产品型号}"` `"@{品牌官方账号}"` |
| 备选切换信号 | Bright Data 配额耗尽 → L4 Nitter + r.jina.ai（最经济） |
| VOC 对应维度 | D1 D7 |

**备注：** X 上相机产品的深度讨论较少，优先级低于 Reddit / YouTube。Nitter 实例稳定性波动，需备多个域名 fallback。

---

## 第三类：应用商店

### S8 Apple App Store 评论

**数据价值：** 直接反映配套 App 的体验问题，是 App 团队（D8 归因）最关键的数据源
**采集状态：** ⚠️ 待排查（替代方案丰富）
**优先级：** P0

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `web_data_apple_app_store` 未激活；当前 `search_engine`+`scrape_as_markdown` 替代 |
| **L2 免费 API** | ✅ **iTunes RSS（官方免费，强烈推荐）** —— `https://itunes.apple.com/<region>/rss/customerreviews/page=1/id=<APP_ID>/sortby=mostrecent/json`，返回结构化 JSON，无需鉴权 |
| **L3 开源库备选** | `app-store-scraper`（npm，开源，封装了 RSS 接口）|
| **L4 LLM 读取备选** | `r.jina.ai/<App Store URL>` 可读到部分评论 |
| **L5 浏览器自动化** | Playwright |
| 可获取字段 | 评分、评论正文、版本号、评论日期、开发者回复 |
| 采集范围 | `{产品型号}` 配套 App（iOS）+ 竞品 App（GoPro Quik / DJI Mimo / Insta360）|
| 备选切换信号 | Bright Data 不可用 → **直接切 L2 iTunes RSS（最佳备选）** |
| VOC 对应维度 | D2 D6 D8 D9 |

**备注：** **iTunes RSS 是最被低估的免费数据源**——官方提供、结构化、无配额，应作为 App 评论的长期主用工具。开发者回复字段可反推团队对问题的认知程度。

---

### S9 Google Play Store 评论

**数据价值：** 同 S8，覆盖 Android 用户，与 iOS 评论对比可发现平台差异问题
**采集状态：** ⚠️ 待排查（替代方案丰富）
**优先级：** P0

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `web_data_google_play_store` 未激活；当前 `search_engine`+`scrape_as_markdown` 替代 |
| **L2 免费 API** | ⚠️ Play Console API 仅限自有 App，不可用作竞品采集 |
| **L3 开源库备选** | ✅ **`google-play-scraper`（npm，开源，强烈推荐）** —— 维护活跃、数据完整、无需鉴权，可获取评论全文 + 设备型号 + Android 版本 |
| **L4 LLM 读取备选** | `r.jina.ai/<Play Store URL>` |
| **L5 浏览器自动化** | Playwright |
| 可获取字段 | 评分、评论正文、设备型号、Android 版本、开发者回复 |
| 采集范围 | `{产品型号}` 配套 App（Android）+ 竞品 App |
| 备选切换信号 | Bright Data 不可用 → **直接切 L3 `google-play-scraper`（最佳备选）** |
| VOC 对应维度 | D2 D6 D8 D9 |

**备注：** `google-play-scraper` 是 Google Play 数据采集的事实标准库，应作为长期主备双轨方案。

---

## 第四类：专业评测平台

### S10 专业评测网站评论区

**数据价值：** 技术背景较强的评论者，能精准指出硬件和固件层面的问题
**采集状态：** 📋 计划中
**优先级：** P1

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `scrape_as_markdown`（Bright Data）|
| **L2 免费 API** | 通常无 |
| **L3 开源库备选** | `requests + BeautifulSoup` / `Scrapy`（评测站普遍无反爬）|
| **L4 LLM 读取备选** | ✅ **`https://r.jina.ai/<评测站 URL>`（推荐，免费、Markdown 输出）** |
| **L5 浏览器自动化** | Playwright（仅对个别 JS 渲染站点使用）|
| 目标站点 | 360rumors.com / TechRadar / Camera Labs / Digital Camera World / Trusted Reviews / The Verge |
| 可获取字段 | 评测正文、读者评论、评分、发布日期 |
| 备选切换信号 | Bright Data 不可用 → **直接切 L4 `r.jina.ai`** |
| VOC 对应维度 | D2 D4 D7 D12 |

**备注：** 专业评测站普遍无反爬保护，**`r.jina.ai` 是性价比最高的备选**。

---

### S11 摄影 / 运动垂类论坛

**数据价值：** 重度用户聚集地，反映极限场景下的真实表现
**采集状态：** 📋 计划中
**优先级：** P1

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `search_engine`（定向搜索）→ `scrape_as_markdown`（Bright Data）|
| **L2 免费 API** | Reddit 子版块部分覆盖（见 S3 L2）|
| **L3 开源库备选** | `requests + BeautifulSoup`（绝大多数论坛无反爬）|
| **L4 LLM 读取备选** | ✅ **`r.jina.ai/<论坛帖子 URL>`** |
| **L5 浏览器自动化** | Playwright（仅必要时）|
| 目标社区 | DPReview 论坛 / Fred Miranda / Reddit r/diving、r/cycling、r/skiing、r/surfing |
| 搜索策略 | `"{产品型号} forum"` / `"{产品型号} [场景词] review"`，场景词：diving / cycling / skiing / cruise / motorcycle / surfing / hiking |
| 可获取字段 | 帖子标题、正文、用户评论、场景描述、具体问题复现步骤 |
| 备选切换信号 | Bright Data 不可用 → r.jina.ai（论坛文本简单，r.jina.ai 几乎全适用）|
| VOC 对应维度 | D2 D4 D5 D6 D8 |

---

### S12 TikTok / YouTube 搜索词分析

**数据价值：** 用户主动搜索的关键词揭示未被满足的信息需求
**采集状态：** 📋 计划中
**优先级：** P1

| 项目 | 内容 |
|-----|------|
| **L1 主用工具** | `search_engine` → `web_data_youtube_comments` / `web_data_tiktok_comments`（Bright Data）|
| **L2 免费 API** | YouTube Data API v3（见 S4 L2）|
| **L3 开源库备选** | `yt-dlp`（YouTube）/ `TikTokApi`（TikTok）|
| **L4 LLM 读取备选** | r.jina.ai 仅适合 YouTube 元信息 |
| **L5 浏览器自动化** | Claude in Chrome（TikTok 首选）|
| 搜索词策略 | ① 问题类：`"{产品型号} problem / issue / fix / error / not working"`  ② 场景类：`"{产品型号} [场景词] test / review"` |
| 重点场景词 | diving、underwater、motorcycle、skiing、cycling、cruise、vlog、travel |
| 备选切换信号 | YouTube 切 yt-dlp；TikTok 切 Claude in Chrome |
| VOC 对应维度 | D2 D4 D5 D6 D13 |

---

## 采集执行指引（含备选方案）

> 直接按以下步骤执行采集。每步标注"主用 → 备选"两条路径。

**Step 1 — 搜索目标 URL**
```
主用：search_engine（Bright Data） query="{产品型号} amazon review" geo="us"
备选：duckduckgo-search（Python 库，免费）或 Google Custom Search API（100/天免费）
目的：找到 {产品型号} 的亚马逊链接
```

**Step 2 — 抓取亚马逊评论页**
```
主用：scrape_as_markdown url="https://www.amazon.com/dp/[ASIN]#customerReviews"
备选 L4：直接 GET https://r.jina.ai/https://www.amazon.com/dp/[ASIN]
备选 L5：Playwright + 住宅代理
返回：评分分布 + 完整评论全文
```

**Step 3 — 抓取 Reddit 讨论**
```
主用：search_engine → web_data_reddit_posts
备选 L2：https://www.reddit.com/r/<sub>/search.json?q={产品型号}（免登录免费 API）
备选 L3：PRAW（Python）
返回：帖子正文 + 全部评论 + 关联帖子列表
```

**Step 4 — 抓取 YouTube 评论**
```
主用：search_engine → web_data_youtube_comments num_of_comments="50"
备选 L2：YouTube Data API v3 commentThreads.list
备选 L3：yt-dlp --write-comments --skip-download <video URL>
返回：评论列表（按点赞排序）
```

**Step 5 — 抓取 App Store 评论**
```
主用：search_engine → scrape_as_markdown
备选 L2（推荐）：GET https://itunes.apple.com/<region>/rss/customerreviews/page=1/id=<APP_ID>/sortby=mostrecent/json
备选 L3：app-store-scraper（npm）
返回：评分、评论正文、版本号、开发者回复
```

**Step 6 — 抓取 Google Play 评论**
```
主用：search_engine → scrape_as_markdown
备选 L3（推荐）：google-play-scraper（npm）—— reviews({ appId: 'com.akaso...', num: 500 })
返回：评分、评论正文、设备、Android 版本
```

**Step 7 — 抓取专业评测站**
```
主用：search_engine → scrape_as_markdown
备选 L4（推荐）：GET https://r.jina.ai/<评测站 URL>
返回：评测正文 + 评论区
```

---

## 采集优先级路线图

### 第一批（一次性报告阶段）
优先采集评论量最大、数据质量最高的来源：

| 优先级 | 数据源 | 预期评论量 | 备注 |
|-------|-------|-----------|------|
| P0 | S1 亚马逊评论（{核心销售市场}） | 数千条 | 最核心 VOC 来源 |
| P0 | S8/S9 App Store / Google Play | 数百条 | 常被忽略但价值高，**优先用 iTunes RSS / google-play-scraper** |
| P0 | S3 Reddit | 数百条 | 深度讨论，**官方 API 长期备选** |
| P1 | S4 YouTube 评论 | 数百条 | **yt-dlp 长期备选** |
| P1 | S10 专业评测站 | 数十条 | **r.jina.ai 长期备选** |
| P2 | S2 亚马逊 Q&A | 数十条 | 补充预期落差分析 |

### 第二批（监控平台阶段）

| 优先级 | 数据源 | 采集频率建议 |
|-------|-------|-----------|
| P1 | S6 TikTok | 每周 |
| P1 | S1 亚马逊（多站点） | 每两周 |
| P2 | S5 Instagram | 每月 |
| P2 | S7 X/Twitter | 事件触发 |

---

## 待探索数据源（Backlog）

> 有价值但尚未纳入计划的数据来源：

- **S13 B&H Photo / Adorama 评论**：摄影专业电商，评论者技术水平更高
- **S14 沃尔玛 / 百思买评论**：覆盖更大众化的消费群体
- **S15 内部客服工单**：第一方数据，与外部评论结合后价值极高
- **S16 用户调研问卷**：结构化调研，可补充定量维度
- **S17 Trustpilot / BBB 投诉记录**：情绪最强烈的用户投诉
- **S18 亚马逊退货原因**：后台数据（需平台权限）

---

## 附录：核心 ASIN 清单

> 采集前必须填写，对应「目标产品配置」中的 `{产品型号}` 与 `{对比竞品列表}`

### {产品型号} 主要 SKU（按市场）

| 市场 | SKU 描述 | ASIN |
|-----|---------|------|
| US | {产品型号} 主版本 | 待填入 |
| UK | {产品型号} 主版本 | 待填入 |
| DE | {产品型号} 主版本 | 待填入 |

### 竞品对标 SKU

| 品牌 | 产品 | ASIN（{核心销售市场}）|
|-----|------|-------------|
| {对比竞品 1} | — | 待填入 |
| {对比竞品 2} | — | 待填入 |
| {对比竞品 3} | — | 待填入 |

### App ID 清单（用于 S8/S9 App Store 采集）

| 平台 | App 名称 | ID |
|-----|---------|-----|
| iOS | {产品型号} 配套 App | 待填入（Apple App ID）|
| Android | {产品型号} 配套 App | 待填入（Package Name，如 com.akaso.xxx）|

---

*如需添加新数据源，请在对应分类下新建 `### Sxx 数据源名称` 段落，并填写：数据价值、采集状态、优先级、L1-L5 五层工具、可获取字段、VOC 对应维度。*
