# VOC 报告设计规范 · Design System

> **基准模板：** `AKASO_EK7000_VOC报告_2026Q2.html`
> **风格名称：** Editorial Intelligence（编辑情报）
> **灵感来源：** *The Economist* 图表语法 · *Financial Times* 数据新闻 · McKinsey/BCG 战略备忘录 · Bloomberg 终端配色
> **适用：** AKASO 全系列 VOC 用户洞察报告（可扩展至其他消费硬件品类与市场）
> **读者：** 公司 CEO + 研发 / 市场 / 软件 / 客服 / 供应链 / 法务各部门负责人
>
> **本规范的唯一目标：** 让任何人（或 AI）照此产出与基准模板**视觉风格完全一致**的新报告。每条规则都精确到 px / hex / 字体栈，附「✅ 必须 / ❌ 禁止」与「⚠️ 关键注意点」。

---

## 0. 30 秒上手

1. 复制 [附录 A：CSS 底座](#附录-acss-底座复制即用) 到新 HTML 的 `<style>`。
2. 复制 [附录 B：页面骨架](#附录-b页面骨架) 作为每一页的起点。
3. 按 [§7 组件库](#7-组件库) 往页面里填数据面板。
4. 按 [§8 图表规范](#8-图表可视化规范-svg) 配可视化。
5. 交付前过一遍 [§10 发布前检查清单](#10-发布前检查清单-checklist)。

**一句话风格定义：** 奶白纸感底 + 限定五色 + 衬线大数字 + 无衬线密集标签 + 强栅格留白 + 数据即图形、文字即脚注。**追求「被相信」，不追求「好看」。**

---

## 1. 设计哲学：Editorial Intelligence

把"严肃新闻编辑部 + 战略咨询报告 + 金融终端"三者熔炼成的视觉范式。它不取悦读者，它**让读者无法回避数据本身**。每一处留白、每一个对齐、每一个数字的字号选择，都应让读者感到「一位资深信息设计师在为我工作」，而不是模板填空。

### 三条铁律（违反即不合格）

| # | 铁律 | 落地标准 |
|---|------|---------|
| 1 | **数据是主角，文字是脚注** | 全页 **图表/数字占 70%，文字占 30%**；禁止大段解释性段落 |
| 2 | **5 秒可读** | 每页只讲一个核心命题，读者靠数字和图形 5 秒内抓住结论 |
| 3 | **被相信，而非好看** | 无渐变、无投影装饰、无图标艺术、无装饰字体；每个色值都来自限定色板 |

### 受众适配

- **CEO**：看第 1 页驾驶舱（金钱 / 退货 / NPS）+ 每页一句裁判式结论 + 红线问题。
- **部门负责人**：看与自己相关的归因、落差、行动清单（带 RICE / 工作量 / 收益）。
- 规范要求：**任何一页都能脱离上下文被单独打印递给某个总监**，他能立刻看懂。

---

## 2. 核心呈现原则

1. **数字优先 + 样本量**：每个指标必须带 `提及率% / 情感极性 / 环比↑↓% / 样本量 n`。`n<30` 标灰「⚠ 低置信度」，`n<10` 不进 P0 排序。
2. **裁判式标题（lead）**：每页正文第一句是一句不超过 ~22 字的结论，`.lead` 样式（21px 衬线半粗）。例：「85% 用户给 4–5 星——但负面集中在电池 / App / SD 卡三处。」
3. **限定色即分类系统**：红 = 危险/差评，琥珀 = 警戒，绿 = 正面/健康，海军蓝 = 品牌/中性强调。读者扫一眼颜色就知道轻重缓急。
4. **FT 图表纪律**：标题居左、副标题写方法、**数据来源永远在右下角**、坐标轴单位明示、刻度数字优先于刻度线。
5. **所有数字右对齐**（便于纵向扫读），**所有标题左对齐**，表格用极细横线而非边框。

---

## 3. 颜色系统

### 3.1 主色板（CSS 变量 = 唯一合法来源）

| 变量 | HEX | 语义 | 用途 |
|------|-----|------|------|
| `--paper` | `#F8F5EE` | 纸张奶白 | 页面底色（纸感而非屏幕白） |
| `--ink` | `#1A1A1A` | 炭灰主文 | 标题、大数字、正文 |
| `--ink-soft` | `#4A4A4A` | 次级文字 | 注释、副文案 |
| `--ink-faint` | `#9A9A9A` | 弱化文字 | 标签、来源、页码 |
| `--rule` | `#D8D2C2` | 分隔线 | 表格横线、卡片描边、虚线 |
| `--navy` | `#1B3A5C` | 品牌主色 | 章节眉、品牌/中性强调、AKASO 自身数据 |
| `--red` | `#C0392B` | 风险信号 | 差评、危险、红线、负向 |
| `--amber` | `#D89614` | 警戒中优先级 | 警戒、痛感、需关注 |
| `--green` | `#2E6B47` | 正向健康 | 好评、健康、正向落差、收益 |
| `--neutral` | `#7F8C8D` | 中性灰 | P2、低优先、待定 |
| `--light-bg` | `#EFEAD8` | 浅底块 | 引言块、示例框、条形槽底 |
| 页面外底色 | `#E8E2D0` | 桌面背景 | `body` 背景（衬托纸张卡片） |

> ❌ **禁止使用色板以外的任何颜色。** 每多一种颜色都是对信任的损耗。

### 3.2 派生色（仅用于热力 / 渐变分级，不可滥用）

| 场景 | 色阶 |
|------|------|
| 严重性堆叠条 | 红线 `#C0392B` → 严重 `#D75A4A` → 痛感 `#D89614` → 改进 `#2E6B47` |
| 热力表格子（浅→深） | `#F8F5EE`(空) · `#FBEEEC` · `#F9E7E4` · `#F4D5D2` · `#E89089` · `#D75A4A` |
| 星级环形（5→1★） | `#2E6B47` · `#7FA98D` · `#D89614` · `#B57A35` · `#C0392B` |
| 引言/原话背景 | `#F1ECD9` |
| 角色色（D15 任务） | 法务`#C0392B` 市场`#D89614` 软件`#1B3A5C` 研发固件`#5D4E37` 研发硬件`#4A6B8A` 客服`#2E6B47` 供应链`#7F8C8D` |

### 3.3 状态符号体系（⚠️ 用几何符号，不用 emoji）

> **规则：** 状态分类一律使用 CSS 几何符号（◆●○），不用 emoji——与「不用艺术符号」的要求一致，且打印 / 跨平台渲染稳定。

| 符号 | CSS 类 | 颜色 | 含义 |
|------|--------|------|------|
| ◆ 菱形 | `.diamond` | red | 红线（安全/法律/合规） |
| ● 实心圆 | `.dot.red` | red | 严重 |
| ● 实心圆 | `.dot.amber` | amber | 痛感 |
| ○ 空心环 | `.ring` | green | 改进 |
| ● 实心圆 | `.dot.green` / `.dot.neutral` | green/灰 | 健康 / 低优先 |

---

## 4. 字体系统

### 4.1 三套字体栈

```css
--serif: 'Source Serif Pro','Source Han Serif SC','Noto Serif SC','Songti SC','SimSun','宋体',serif;
--sans:  'Microsoft YaHei','微软雅黑','PingFang SC','Source Han Sans SC','Noto Sans SC','Segoe UI','Helvetica Neue',sans-serif;
--quote: 'Microsoft YaHei','微软雅黑','PingFang SC','Source Han Sans SC','Segoe UI','Helvetica Neue',sans-serif;
```

### 4.2 用法规则

| 字体 | 用在哪 | 传达 |
|------|--------|------|
| **`--serif` 衬线** | 大标题（page-title）、所有**大数字/KPI**、裁判式 lead、计算示例 | "已被审计的权威与庄严" |
| **`--sans` 无衬线（微软雅黑）** | **正文**、所有图表标签、表格、注释、章节眉 | 密集信息的可读性 |
| **`--quote` 引言体** | 英文用户原话、关键词短语 | 自然引用感 |

### 4.3 ⚠️ 字体关键规则（必须遵守）

1. ❌ **英文引言/关键词不要用「衬线 + 斜体」**——笔画过细过尖锐，CEO 阅读距离看不清。
   ✅ 改用 `--quote`（微软雅黑常规体），并加**浅底块 `#F1ECD9` + 左侧色条**来表达"这是引用"。
2. ✅ **中文正文统一微软雅黑**（`--sans` 以它为首）。
3. ⚠️ **SVG 内的 `font-family` 不能用 CSS 变量**（`font-family="var(--sans)"` 不生效）。
   ✅ 必须写字面串：`font-family="Microsoft YaHei, PingFang SC, sans-serif"`。
4. ❌ 不允许任何装饰艺术字体、手写体、艺术符号。

---

## 5. 字号比例尺（Type Scale）

> ⚠️ **最重要的一条规则：字号宁大勿小。** body 不低于 15.5px、标签不低于 11px，否则 CEO 阅读距离会吃力。新报告必须直接采用下表，不得使用更小字号。

### 5.1 基础

| 元素 | 字号 | 字重 | 字体 | 其他 |
|------|------|------|------|------|
| `body` 正文 | **15.5px** | 400 | sans | line-height 1.55 |
| `.eyebrow` 章节眉 | 12px | 700 | sans | ls 4px / 大写 / navy |
| `.page-title` h1 | **40px** | 600 | serif | line-height 1.1 |
| `.lead` 裁判式结论 | **21px** | 600 | serif | line-height 1.45 |
| `.section-label` 区块标题 | 11.5px | 700 | sans | ls 2.5px / 大写 / ink-faint |
| `.page-no` 页码 | 12px | — | serif | ink-faint |
| `.page-footer` 页脚来源 | 11px | — | sans | ink-faint |
| 小字说明块 | 11–11.5px | — | sans | line-height 1.65–1.7 |

### 5.2 组件大数字（衬线，半粗）

| 组件 | 数字字号 | 标签字号 |
|------|---------|---------|
| KPI 数字墙 `.kpi-card .num` | **52px** | label 12px / sub 13px / note 12.5px |
| 红线大字 `.redline-box .big` | **100px** | — |
| 画像编号/占比 `.persona-card` | no 42px / pct 38px | name 21px / traits 13.5px |
| 变化卡 `.change-card` | delta 32px / range 19px | name 23px |
| RICE 分 `.rice-card .v` | 32px | task 15px |
| 热力表数字 `.heatmap` | 17px | th 12px |
| TOP3 编号 `.top3-card .no` | 38px | name 17px / quote 13.5px |
| 表格 `.pain-table` / `.task-table` | td 13px / 12px | th 11.5px / 11px |
| 哑铃 `.dumbbell-row` | 14px / lab 13.5px | delta 12px |
| Take-away `.takeaway` | headline 20px | action 14.5px |
| 跨画像冲突 `.tension-box` | body 19px | — |

> **判断法则**：一页里"最该被看见的数字"用 38–100px；支撑指标 17–32px；标签注释 11–14px。**层级要拉开**，不要所有字一样大。

---

## 6. 布局与页面

### 6.1 容器与页面尺寸

```css
.report{ max-width:1240px; margin:0 auto; }     /* 整份报告居中 */
.page{
  min-height:1480px;          /* ⚠️ 关键：用 min-height，不用 aspect-ratio */
  padding:48px 54px 36px;
  display:flex; flex-direction:column;
  background:var(--paper);
  box-shadow:0 6px 30px rgba(0,0,0,.08);
}
```

> ⚠️ **避免大片留白（最常见的错误）：**
> ❌ 不要用 `aspect-ratio` 固定页面比例 —— 会把页面强行拉高、内容只占 60–70%、底部大片空白。
> ✅ 改用 `min-height:1480px` —— 给足高度但让内容**自然撑满**，内容多了页面会自己变长，绝不裁切。
> ✅ 底部的 `.spacer` 用 `flex:0 0 24px`（固定小间距），**不要用 `flex:1`**（会把页脚顶到底部、中间留空）。

### 6.2 间距尺度（8 的倍数为主）

- 区块间距 `margin-bottom`：常用 `14 / 20 / 22 / 26 / 32`
- 栅格 `gap`：`16 / 20 / 22 / 30 / 32 / 36`
- 工具类：`.mb-2=14px` `.mb-3=22px` `.mb-4=32px`
- 双栏 `.grid-2{gap:32px}`、三栏 `.grid-3{gap:22px}`

### 6.3 页眉 / 页脚（每页固定结构）

```
┌ 页眉 ──────────────────────────────────┐
│ CHAPTER 02 · 业务影响驾驶舱        02 / 12 │  ← eyebrow(左) + page-no/meta(右)
│ 这一期的产品健康度。                       │  ← h1.page-title
│ ───────────────────────────────────────│  ← .page-rule（1.2px 实线 ink）
└─────────────────────────────────────────┘
            ...（核心内容）...
┌ 页脚 ──────────────────────────────────┐
│ ─────────────────────────────────────  │  ← .footer-rule（1.2px rule）
│ 数据：Amazon US n=38,809 ...  VOC ... Q2 │  ← 来源(左) + 落款(右)
└─────────────────────────────────────────┘
```

- 页脚来源行**必填**：写明各数据源样本量（FT 纪律：来源永远在右下区）。
- 落款固定：`VOC Editorial Intelligence · 2026 Q2`。

### 6.4 ⚠️ 章节标号体系（不用内部维度编号）

> **为什么：** 框架的维度登记号 `D0/D1/D2…` 会因维度按受众跳选（如 D2 后接 D8）与"章节先后"冲突、给读者造成歧义，因此报告中不直接显示。

**现行规则：**
- 章节眉用 **`CHAPTER NN · 模块名`**，`NN = 物理页码序号`（与页脚 `NN / 12` 完全一致，目录、页眉、页脚三处统一）。
- 目录 `.toc` 用 `页码序号 + 模块名`，不出现 `Dxx`。
- ❌ 正文交叉引用**禁止**写 `Dxx`；✅ 一律用**模块名称**（如 `D8`→`问题归因`、`D6`→`变通行为`、`D2 P0 #3`→`痛点地图 P0 #3`）。
- 维度登记号 `Dxx` 只活在《VOC 分析维度框架.md》（内部方法论工具）和 HTML 代码注释里，**不渲染给读者**。

---

## 7. 组件库

> 以下为高频组件的结构骨架与关键样式。完整 CSS 以基准模板为准。所有组件共享 §3 配色、§5 字号。

### 7.1 KPI 数字墙（驾驶舱核心）
3 列网格，每卡 = 顶部状态色条 + 标签 + 52px 大数字 + 副值 + 状态注。
```html
<div class="kpi-grid">                          <!-- grid 3 列, gap 16 -->
  <div class="kpi-card"><div class="top-bar red"></div><div class="body">
    <div class="label">退货率</div>
    <div class="num">8.7%</div>
    <div class="sub">↑ 1.8pp（环比）</div>
    <div class="note red">固件 v2.3 触发</div>
  </div></div>
  ...
</div>
```
状态色条 `.top-bar` 用 `red/amber/green` 三态，呼应红黄绿分类。

### 7.2 CEO Take-away（黑底强调块）
每页结论收束，或驾驶舱页给 CEO 的一句话。`background:var(--ink)` 白字。
```html
<div class="takeaway">
  <div class="tag">CEO TAKE-AWAY</div>            <!-- 12px ls4 -->
  <div class="headline">本期最大风险：……形成法律级落差。</div> <!-- 20px serif -->
  <div class="action">立即行动：……启动召回评估。</div>      <!-- 14.5px -->
</div>
```

### 7.3 优先级表（pain-table / task-table）
极细横线、无外框；首行 `th` 大写小字 + 1.5px ink 下边线；数字列右对齐；优先级/严重性用符号 + 色。
```html
<table class="pain-table">
  <thead><tr><th>#</th><th>标签</th><th>提及率</th><th>差评率</th><th>环比</th><th>严重性</th><th>归因</th><th>优先级</th></tr></thead>
  <tbody>
    <tr><td class="num">01</td><td>电池续航 短</td><td class="num">19.6%</td>
        <td class="neg-high">39.8%</td><td class="delta-high">+28%</td>
        <td><span class="dot red"></span> 严重</td><td>硬件/电池</td><td class="p0">P0</td></tr>
  </tbody>
</table>
```

### 7.4 严重性分级 + 四级定义说明（⚠️ 必附）
堆叠条展示占比，**下方必须附四级定义**（含义 + 时效），否则读者看色块无法判断依据。
```html
<div class="severity-bar">
  <div style="flex:4;background:var(--red)">4%</div>
  <div style="flex:38;background:#D75A4A">38%</div>
  <div style="flex:42;background:var(--amber)">42%</div>
  <div style="flex:16;background:var(--green)">16%</div>
</div>
<!-- 四级定义：◆红线=安全/法律 24h上报｜●严重=退货/1星 当周｜●痛感=能忍 当版本｜○改进=好评建议 backlog -->
```

### 7.5 红线问题独立卡（◆ red 描边）
红线问题**必须单独成块**，不与其他问题混排；右侧 100px 大数字显示红线数量。

### 7.6 引用类卡（原话 / 营销素材回溯 / 信号词）
- 用户原话：`--quote` 字体 + `#F1ECD9` 底 + 左色条（见 §4.3）。
- 营销素材回溯卡 `.trace-card`：模块名(red 15.5px) + 来源 + 实际反馈 + 整改建议(navy)。
- 信号词条 `.signal-row`：`关键词 + 条形 + 计数`，负向 red、正向 green。

### 7.7 阶段诊断卡 / 画像卡 / 跨画像冲突
- `.diag-card`：白底 + 左侧 3px 色条（避免大字号下显空）。
- `.persona-card`：左侧 10px 色条 + 编号 42px + 占比 38px + 一句话诉求 + 引用块。
- `.tension-box`：黑底块，承载跨维度结论（19px serif）。

### 7.8 数据图计算说明块（⚠️ 必附，小字）
> **为什么：** 凡是"算出来的分数/指数"（如 SKU 健康分、优先级总分、RICE），图表下方**必须附公式 + 判级标准 + 本期计算示例**，否则读者看到红黄绿无法复算、不信任。

规则：
- 用 **11–11.5px 小字**，`border-top:1px dashed var(--rule)` 与图表分隔。
- 公式分量用 2×2 网格；判级阈值用符号 + 色；
- **计算示例必须与图表里展示的数字自洽**（读者能一眼复算）。
- 缺真实数据的分量标注「估算」，并写明"接入后台数据后精确化"（保持诚实基调）。

```html
<div style="margin-top:16px;padding-top:12px;border-top:1px dashed var(--rule);font-size:11.5px;color:var(--ink-soft);line-height:1.7">
  <div style="font-weight:700;letter-spacing:1.5px;color:var(--ink-faint);font-size:11px;margin-bottom:8px">健康分计算方法 · 0–100 分制加权求和</div>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:4px 28px;margin-bottom:9px">…四分量…</div>
  <div style="display:flex;gap:18px;flex-wrap:wrap;margin-bottom:9px">…判级阈值…</div>
  <div style="background:var(--light-bg);padding:8px 12px;font-family:var(--serif)">…本期计算示例 = 76.75 ≈ 77（健康）…</div>
</div>
```

### 7.9 目录 TOC
顶部白卡，`页码序号(navy serif) + 模块名`，两列虚线分隔，可点击锚点跳转。

---

## 8. 图表 / 可视化规范（SVG）

### 8.1 通用规则（FT 图表纪律）

- 用**内联 SVG**（无外部库、无 JS），`viewBox` 自适应，`width:100%`。
- 标题居左用 `.section-label`；副标题写**计算口径/方法**；**数据来源放页脚右下**。
- 坐标轴：单位明示，刻度数字（10px ink-faint）优先于刻度线（0.4–0.5px rule）。
- 颜色严格用色板；AKASO 自身用 navy，竞品用 red/amber。
- 网格线极细 `stroke:#D8D2C2; stroke-width:0.4–0.6`。

### 8.2 分析 → 图表类型映射（强制）

| 内容 | 图表 |
|------|------|
| 驾驶舱 KPI | 数字墙 + 红黄绿状态条 |
| 情感分布 | 环形图（星级占比）+ 趋势折线 |
| 痛点地图 | 气泡图（x=提及率 / y=差评率 / 大小=环比 / 色=严重性）+ 排行表 |
| 亮点 / 正面 | 横向条形 |
| 改善验证 / 预期落差 | 哑铃图（上期 vs 本期 / 期望 vs 实际） |
| 归因 | 环形（硬/固/软/其他）+ 归因×严重性热力表 |
| 情感时间线 | 折线弧（横轴=使用阶段） |
| 画像 | 卡片（编号 + 占比 + 一句话） |
| 竞品 | 雷达图（多边形叠加） |
| 下一代机会 | Kano 四象限气泡图 |
| 行动清单 | 任务卡墙 / 表 + RICE 排序 |

### 8.3 ⚠️ SVG 图表关键规则（逐条，务必遵守）

1. **图例移出 SVG，用 HTML 排在图表上方**。
   ❌ 把 `<g>` 图例画在 SVG 内角落 → 会压住轴标签/数据。✅ 用 HTML flex 行图例（色块/虚线 + 文字）。
2. **雷达图：刻度数字要避让顶部轴标签**。
   把整盘下移（加大 viewBox 高度、`translate` 下移），刻度数字横向左移（`x=-12` 右对齐），与顶部轴标签拉开 ≥14px。
3. **Kano / 散点：气泡收进绘图区安全区，标签统一在气泡下方，象限标题压在四角**。
   ❌ 气泡贴边/飘出框、标签放气泡上方挤进标题区。✅ 气泡 `cy` 留足边距，标签 `text` 在 `cy + r + 12`；底部行标签才放上方。
4. **哑铃图：数字标签必须与圆点对齐且不重叠**。
   ✅ 标签 `left` = 对应圆点 `left`，加 `transform:translateX(-50%)` 居中；标签放 `top:0`、圆点 `top:50%`、落差值放 `bottom`；标签 `line-height:1`；连接条区 `height≥52px`。
5. **SVG 文本 `font-family` 写字面串**，不用 `var(--sans)`（见 §4.3）。
6. **气泡/散点坐标换算写注释**，便于校对：
   `cx = 左边距 + 值/量程 × 绘图宽；cy = 底边距 − 值/量程 × 绘图高`（注意 SVG 的 y 轴向下）。
7. **排序类图表/表格必须标明排序依据**（如「按差评率从高到低」），并真的按该序排列。

---

## 9. 内容与文案规范

1. **每页一个命题**：lead 一句话结论（≤22 字）→ 图表矩阵 → 一行行动指令收束。
2. **数字带 n 与置信度**：`n<30` 标灰「⚠ 低置信度」。
3. **交叉引用用模块名**，不用 Dxx（见 §6.4）。
4. **排序图标注依据**（见 §8.3-7）。
5. **算出来的分数必附计算说明**（见 §7.8）。
6. **状态用几何符号**（◆●○），不用 emoji（见 §3.3）。
7. **诚实基调**：缺第一方数据时标「估算」，不伪造精确值。
8. 文案语言：中文为主，英文术语/原话保留原文；数字、单位、百分号统一半角。

---

## 10. 发布前检查清单（Checklist）

> 每份新报告交付前逐条核对。**这张表汇总了全部关键质量要求。**

**布局与留白**
- [ ] 页面用 `min-height` 而非 `aspect-ratio`；无大片底部留白
- [ ] `.spacer` 为 `flex:0 0 24px`，未用 `flex:1`
- [ ] 每页内容自然撑满约 90% 高度

**字体字号**
- [ ] `body` ≥ 15.5px；标签 ≥ 11px；大数字层级拉开（38–100px）
- [ ] 中文正文 = 微软雅黑；标题/大数字 = 衬线
- [ ] 英文引言用 `--quote` 常规体 + 底块/色条，**无衬线斜体**
- [ ] SVG 文本 `font-family` 为字面串，非 `var()`

**图表（重点防错位）**
- [ ] 图例在 SVG 外（HTML），未遮挡数据/轴标签
- [ ] 雷达刻度与轴标签不重叠
- [ ] Kano/散点气泡在框内、标签不挤标题、不飘出
- [ ] 哑铃数字与圆点对齐居中、不重合
- [ ] 排序图已按声明的依据排列并标注

**内容**
- [ ] 无任何渲染可见的 `Dxx`；交叉引用为模块名
- [ ] 章节眉 = `CHAPTER NN · 模块名`，NN 与页码、目录一致
- [ ] 算分图表（健康分/优先级/RICE）已附公式 + 判级 + 自洽示例
- [ ] 严重性堆叠条下附四级定义
- [ ] 每个指标带 n；低样本标灰
- [ ] 每页页脚有数据来源（右下）

**颜色与符号**
- [ ] 只用 §3 色板；无额外颜色
- [ ] 状态符号为 ◆●○，无 emoji
- [ ] 红=差评/危险、琥珀=警戒、绿=健康、navy=品牌，语义未混用

---

## 11. 新报告生产流程

1. **复制底座**：附录 A 的 `<style>` + 附录 B 的页面骨架。
2. **定章节**：按受众从维度库挑选维度，定叙事顺序，编 `CHAPTER 01…NN`（CEO 必含：驾驶舱 / 情感 / 落差 / 归因含红线 / 下一代 / 行动清单）。
3. **填数据面板**：每页 lead 结论 + §7 组件 + §8 图表。
4. **配可视化**：按 §8.2 选图型，按 §8.3 防错位。
5. **附说明**：算分图表加计算说明，严重性加四级定义。
6. **过 Checklist**：§10 逐条核对。
7. **交付**：浏览器直接打开；如需 PDF 用浏览器 `Ctrl+P`（A3 / 自定义纸张），打印样式已内置。

---

## 附录 A：CSS 底座（复制即用）

```css
:root{
  --paper:#F8F5EE; --ink:#1A1A1A; --ink-soft:#4A4A4A; --ink-faint:#9A9A9A;
  --rule:#D8D2C2; --navy:#1B3A5C; --red:#C0392B; --amber:#D89614;
  --green:#2E6B47; --neutral:#7F8C8D; --light-bg:#EFEAD8;
  --serif:'Source Serif Pro','Source Han Serif SC','Noto Serif SC','Songti SC','SimSun','宋体',serif;
  --sans:'Microsoft YaHei','微软雅黑','PingFang SC','Source Han Sans SC','Noto Sans SC','Segoe UI','Helvetica Neue',sans-serif;
  --quote:'Microsoft YaHei','微软雅黑','PingFang SC','Source Han Sans SC','Segoe UI','Helvetica Neue',sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{background:#E8E2D0;font-family:var(--sans);color:var(--ink);font-size:15.5px;line-height:1.55;-webkit-font-smoothing:antialiased}
body{padding:36px 20px}
.report{max-width:1240px;margin:0 auto}
.page{background:var(--paper);width:100%;min-height:1480px;padding:48px 54px 36px;margin:0 auto 36px;position:relative;display:flex;flex-direction:column;box-shadow:0 6px 30px rgba(0,0,0,.08)}
@media print{ body{padding:0;background:#fff} .report{max-width:none} .page{box-shadow:none;margin:0;page-break-after:always;min-height:auto;height:100vh} }

.eyebrow{font-size:12px;font-weight:700;letter-spacing:4px;color:var(--navy);text-transform:uppercase}
.page-no{font-size:12px;letter-spacing:2px;color:var(--ink-faint);text-align:right;font-family:var(--serif)}
.page-meta{font-size:10px;letter-spacing:2.5px;color:var(--ink-faint);text-align:right;margin-top:4px}
h1.page-title{font-family:var(--serif);font-size:40px;font-weight:600;margin:10px 0 18px;line-height:1.1}
.page-rule{height:1.2px;background:var(--ink);margin-bottom:22px}
.lead{font-family:var(--serif);font-size:21px;font-weight:600;line-height:1.45;margin-bottom:26px;max-width:96%}
.lead.red{color:var(--red);margin-top:-14px}
.section-label{font-size:11.5px;letter-spacing:2.5px;color:var(--ink-faint);font-weight:700;text-transform:uppercase;margin-bottom:12px}
.head-row{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:10px}
.spacer{flex:0 0 24px}                 /* ⚠️ 不要用 flex:1 */
.footer-rule{flex:0 0 1.2px;background:var(--rule);margin:18px 0 10px}
.page-footer{display:flex;justify-content:space-between;font-size:11px;color:var(--ink-faint);gap:30px}
.page-footer .right{letter-spacing:2px;flex-shrink:0}

/* 状态符号 */
.dot{display:inline-block;width:11px;height:11px;border-radius:50%;vertical-align:middle}
.dot.red{background:var(--red)} .dot.amber{background:var(--amber)}
.dot.green{background:var(--green)} .dot.neutral{background:var(--neutral)}
.diamond{display:inline-block;width:10px;height:10px;background:var(--red);transform:rotate(45deg);vertical-align:middle}
.ring{display:inline-block;width:11px;height:11px;border:1.5px solid var(--green);border-radius:50%;vertical-align:middle}

/* 栅格工具 */
.grid-2{display:grid;grid-template-columns:1fr 1fr;gap:32px}
.grid-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:22px}
.mb-2{margin-bottom:14px}.mb-3{margin-bottom:22px}.mb-4{margin-bottom:32px}
```

---

## 附录 B：页面骨架

```html
<section class="page" id="pN">
  <div class="head-row">
    <div>
      <div class="eyebrow">CHAPTER 0N · 模块名</div>
      <h1 class="page-title">一句裁判式的页面命题。</h1>
    </div>
    <div>
      <div class="page-no">0N / 总页数</div>
      <div class="page-meta">AKASO 〔型号〕 · VOC INSIGHT</div>
    </div>
  </div>
  <div class="page-rule"></div>

  <div class="lead">≤22 字的核心结论，数字打头。</div>

  <!-- 核心数据面板 / 图表（§7 组件 + §8 图表） -->

  <div class="spacer"></div>
  <div class="footer-rule"></div>
  <div class="page-footer">
    <span>数据：Amazon US n=…；Reddit …；YouTube …（口径说明）。</span>
    <span class="right">VOC Editorial Intelligence · 〔报告期〕</span>
  </div>
</section>
```

---

## 附录 C：相关文档

| 文档 | 角色 |
|------|------|
| `VOC报告设计规范.md`（本文件） | **视觉/呈现规范**——怎么做得好看且可信、风格一致 |
| `VOC分析维度框架.md` | **内容/分析框架**——分析哪些维度、指标怎么算（Dxx 在此处使用） |
| `VOC数据源清单.md` | **数据采集**——从哪爬、用什么工具 |
| `设计哲学-Editorial-Intelligence.md` | 风格的"立意宣言"（本规范是其工程化落地版） |
| `AKASO_EK7000_VOC报告_2026Q2.html` | **黄金样板**——任何不确定处以此文件为准 |

> 三份规范分工：**框架管"分析什么"，数据源管"数据哪来"，本规范管"怎么呈现"。** 产出新报告时三者配合使用。

---

*本规范是活文档。如在实践中验证出新的有效做法，请在对应章节补充，并同步更新 §10 检查清单。*
