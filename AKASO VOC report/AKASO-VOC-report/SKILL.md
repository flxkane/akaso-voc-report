---
name: AKASO-VOC-report
description: 生成 AKASO（及其他消费硬件）产品的 VOC 用户洞察报告。当用户要做产品口碑/用户之声调研、差评痛点分析、竞品感知分析、预期落差分析，或提到「VOC 报告」「用户洞察报告」「调研报告」「AKASO 报告」「运动相机口碑分析」时使用。工作流覆盖：询问目标产品型号 → 连接 Bright Data MCP 并小范围试爬验证（失败则切免费备选）→ 多源采集真实评论 → 按 D0–D15 维度分析 → 按 Editorial Intelligence 设计规范+样板生成报告（自动检测并叠加 canvas-design 设计技能，缺失时引导启用并回退到规范产出，默认 HTML）。
---

# AKASO-VOC-report · VOC 用户洞察报告工作流

把"全网产品评论"加工成一份**给 CEO 和各部门看的、数据驱动的 VOC 洞察报告**。报告风格固定为 **Editorial Intelligence**（灵感：The Economist / Financial Times 数据新闻），数据和图表为主、文字为辅。

## 交付物

- 默认：一份多页 **HTML** 报告（浏览器可直接打开，可 `Ctrl+P` 导出 PDF）。
- 若用户明确要求其他格式（PDF / PPT / 海报等），按其要求输出。

## 本 skill 附带的参考文件（务必先读）

| 文件 | 作用 | 何时读 |
|------|------|--------|
| `references/01_VOC分析维度框架.md` | 分析什么——D0–D15 维度、指标算法、严重性分级、优先级公式 | 分析阶段 |
| `references/02_VOC数据源清单.md` | 数据哪来——数据源、Bright Data 工具、**免费备选体系**、采集执行指引 | 采集阶段 |
| `references/05_BrightData_MCP_设置指南.md` | 怎么连——Bright Data MCP **保姆级设置**（控制台 / Token / URL / 桌面端连接 / 免费额度） | 连接阶段（Step 1） |
| `references/03_设计哲学.md` | 风格立意——Editorial Intelligence 的视觉哲学 | 生成阶段 |
| `references/04_VOC报告设计规范.md` | 怎么呈现——颜色/字体/字号/组件/图表/检查清单（**精确到 px/hex**） | 生成阶段（最重要） |
| `references/样板_AKASO_EK7000_VOC报告.html` | **黄金视觉样板**——任何不确定处以此文件为准 | 生成阶段 |

---

## ⚠️ 三条铁律（任何时候不得违反）

1. **连接 MCP 后，先小范围试爬验证，确认能正常返回数据，再开始正式采集。绝不一上来就大批量爬取。**
2. **报告默认输出 HTML**；只有用户明确要求才换格式。
3. **报告里不出现 `Dxx` 维度编号**（用「CHAPTER NN · 模块名」+ 模块名称交叉引用，详见设计规范 §6.4）；状态用几何符号 ◆●○，不用 emoji。

---

## 工作流

### Step 0 · 确认目标产品型号

- **如果用户已经说了产品型号**（如「调研 AKASO EK7000」），直接采用，**不要再问**。
- 如果没说，问一句：「本次要调研的产品型号是什么？（例：AKASO EK7000 / Brave 7 / V50 Pro）」
- 拿到型号后，按 `references/02` 的「目标产品配置」记录：目标 SKU、市场（默认 us）、竞品对标 SKU。
- 之后所有采集、分析、报告中的 `{产品型号}` 占位符都替换为该型号。

### Step 1 · 连接 Bright Data MCP（主用采集工具）

1. **先检查是否已连接**：查看当前可用工具里是否有 `mcp__…__search_engine`、`mcp__…__scrape_as_markdown`、`mcp__…__web_data_reddit_posts` 等（名字含 `search_engine` / `scrape_as_markdown` 的 MCP 工具）。
   - 若已存在 → 直接进入 Step 2。
2. **若未连接，按 `references/05_BrightData_MCP_设置指南.md` 手把手教对方连接（保姆级，约 5 分钟）。** 先把下面这段关键信息发给对方：

   > Bright Data MCP 是主用采集工具，**每月有免费额度**（约 5,000 次请求，以控制台 Usage 为准），够产出多份报告。
   >
   > 1. 进控制台 **https://brightdata.com/cp** → 左侧边栏 **AI Gateways → MCP → Configure MCP**。
   > 2. 勾选要开通的工具组（**第一个 `Search & Extract` 免费、必勾**；E-commerce / Social Media / App Stores 等付费、按需）→ 点 Continue to Configure。
   > 3. 在页面下方**复制自动生成的连接 URL**（形如 `https://mcp.brightdata.com/sse?token=…&groups=…`，令牌已填好、无需手改）。
   > 4. 把这条 URL 接入 AI 工具：Claude 桌面端 = **Settings / Customize → Connectors → Add custom connector** → 粘贴 URL；其它工具见指南 5B / 5C。重启后回复「**已连接**」。
   >
   > 📖 完整图文步骤见 `references/05_BrightData_MCP_设置指南.md`，含 **多种 AI 工具的接入方式（Claude 桌面端 / Claude Code CLI / Cursor / Windsurf / VS Code 等）**、数据集工具组、免费额度与常见问题排查。对方用什么工具，就照指南里对应的小节走。

   - 若对方不想配置 Bright Data，可直接走免费备选体系（见 Step 2 失败分支），照常出报告。

3. **等待用户回复「已连接」后**，再进入 Step 2 验证。

### Step 2 · 小范围试爬验证（关键，不可跳过）

> 目的：在投入大批量采集前，确认工具链真的能返回有效数据。

用**最小成本**做一次验证（任选其一，建议 1–2 次）：
- `search_engine` 搜索 `"{产品型号} amazon review" geo=us`，确认返回结构化结果（标题/链接）。
- 或 `scrape_as_markdown` 抓取**一个**亚马逊产品页，确认能拿到评分分布 + 评论正文。

判定：
- ✅ **返回正常** → 告诉用户「验证通过，开始正式采集」，进入 Step 3。
- ❌ **返回空 / 报错 / 配额不足 / 无法连接** → **不要重试纠缠**，立即切换到免费备选体系：
  - 按 `references/02` 的「采集工具备选体系（双层架构）」与「免费备选切换决策树」，对每个平台选用 L2 免费 API → L3 开源库 → L4 `r.jina.ai`（WebFetch）→ L5 浏览器自动化。
  - 例：Reddit 用 `.json` 免登录 API；YouTube 用 Data API v3；App Store 用 iTunes RSS；专业评测站用 `r.jina.ai/<URL>`。
  - 切换后同样**先小范围验证再正式采集**。

### Step 3 · 多源采集真实评论

- 严格按 `references/02` 的「采集执行指引（含备选方案）」Step 1–7 执行。
- 按「采集优先级路线图」第一批优先：**Amazon 评论（P0）→ App Store/Google Play（P0）→ Reddit（P0）→ YouTube（P1）→ 专业评测（P1）→ Amazon Q&A（P2）**。
- 同时采集 **{产品型号} 自身 + 竞品对标 SKU**，供竞品感知分析使用。
- **数据纪律**：报告里所有数字必须来自真实采集到的评论；缺第一方数据（退货率/复购率等）时标注「估算」，不得伪造精确值。注意每个标签的样本量 `n`，`n<30` 标灰、`n<10` 不进 P0。

### Step 4 · 按 D0–D15 维度分析

- 读 `references/01_VOC分析维度框架.md`，按维度体系做分析。
- 至少覆盖给 CEO 的必含维度：**业务影响驾驶舱 / 情感分布 / 预期落差 / 问题归因（含红线）/ 下一代机会 / 角色行动清单**；按数据丰富度补充痛点地图、竞品对比、用户画像等。
- 按框架的**优先级公式**给问题排序，按**严重性分级**（红线/严重/痛感/改进）标注，按 **SKU 健康分公式**算分（图表下方必须附公式与本期计算示例）。

### Step 5 · 生成报告（设计规范+样板锁定视觉，canvas-design 为可选增强）

> **视觉一致性的硬保证来自 `references/04_VOC报告设计规范.md` + 黄金样板 HTML，不依赖任何外部技能。** 报告是按规范的 CSS token / 组件 / 图表规则，用纯 HTML+CSS+SVG 一行行写出来的——这正是样板的产出方式。`canvas-design` 只是"设计审美增强"，**有则更好、无也不影响一致性**。

1. **先检测 `canvas-design` 是否可用**（Claude Code 的内置/官方设计技能）：
   - **可用** → 调用 `/canvas-design` 来增强版面与审美决策，叠加在第 2 点的硬规范之上。
   - **不可用** → 按下方「启用 canvas-design」给对方一次引导；**但绝不因此卡住或降级**，直接走第 2 点，按规范+样板手工产出，视觉同样高度一致。**严禁改用其它通用方式草草产出没有设计感的报告。**
2. **无论有没有 canvas-design，都必须严格执行以下硬规范（这才是一致性的真正来源）：**
   - 套用 `references/04` 的颜色（只用色板）、字号比例尺、组件库、图表规范（尤其 §8.3 的 SVG 防错位规则）；设计立意见 `references/03`。
   - 用纯 HTML + CSS + 内联 SVG（无外部库、无 JS），从设计规范「附录 A：CSS 底座」+「附录 B：页面骨架」起步。
   - **任何视觉细节不确定，逐像素对照 `references/样板_AKASO_EK7000_VOC报告.html`**——它是黄金基准，成品必须与它的配色/字体/字号/组件/图表风格一致。
3. 默认产出 **HTML**；仅当用户明确要单页静态海报 / PDF 画册时，才用 canvas-design 直接产出 png/pdf。

#### 启用 canvas-design（仅当检测到缺失时，给对方一次引导，不阻塞流程）

> `canvas-design` 通常是 Claude Code 自带的设计技能。若 `/canvas-design` 不可用，可这样启用：
> 1. 在 Claude Code 输入 `/plugin`，从 marketplace 安装 **anthropic-skills** 插件（内含 canvas-design）；或把 Claude Code 更新到包含该技能的版本。
> 2. 重启 / 重连 Claude Code 后，`/canvas-design` 即生效。
>
> 若对方暂时无法启用，**照常出报告**——走第 2 点的「规范+样板」路径，视觉一致性不受任何影响。

### Step 6 · 交付前自检

逐条核对 `references/04` 的 **§10 发布前检查清单**，重点：
- [ ] 图表无错位/遮挡（图例在 SVG 外、雷达刻度不压轴标签、Kano 气泡在框内、哑铃数字对齐圆点）
- [ ] 页面无大片留白（用 `min-height` 非 `aspect-ratio`，`.spacer` 用 `flex:0 0`）
- [ ] 字号符合比例尺（body≥15.5px、大数字层级拉开）；英文引言非衬线斜体
- [ ] 无 `Dxx`；章节号 = 页码；算分图表附计算说明；严重性附四级定义
- [ ] 每个指标带 n；每页页脚有数据来源
- [ ] 颜色只用色板；状态符号为 ◆●○

### Step 7 · 交付

- 把 HTML 文件交付给用户，并说明：浏览器直接打开查看；导出 PDF 用 `Ctrl+P`（纸张选 A3 或自定义）。
- 用一句话总结本期报告的最大风险与 TOP 行动项（呼应报告第 1 页的 CEO Take-away）。

---

## 一句话流程图

```
问型号 → 连 Bright Data MCP → 小范围试爬验证 ──成功──→ 正式多源采集 → D0–D15 分析 → 设计规范+样板（canvas-design 有则叠加增强）出 HTML → 自检 → 交付
                                    └──失败──→ 切 references/02 免费备选（L2→L3→L4→L5）→（同样先验证）→ 回到正式采集
```
