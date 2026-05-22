# Bright Data MCP 设置保姆级指南

> Bright Data MCP 是本工作流的**主用采集工具**（反爬强、覆盖广、速度快），用来抓取 Amazon / Reddit / YouTube / App Store 等平台的真实评论。
> 这份指南手把手教你：注册 → 进 MCP 配置页 → 勾选工具组 → 复制连接 URL → 接入你的 AI 工具（Claude 桌面端 / Claude Code CLI / Cursor / Windsurf 等）→ 验证可用。**全程约 5 分钟，从零开始也能照做。**
>
> 💡 **关于费用：** 第一个工具组 **Search & Extract 完全免费**（每月 **5,000 次调用**，以控制台 Usage 为准），足够产出多份报告；其余工具组按量付费、**按需开通**。**本工作流只开免费组也能跑通**——付费组只是让部分平台的数据更省事。额度耗尽 / 连接异常时，工作流会自动切到免费备选工具（见 `02_VOC数据源清单.md`），不会卡住。

---

## 设置总览

```
① 注册 / 登录 → ② 进 AI Gateways → MCP → Configure MCP → ③ 勾选工具组（免费组必选）→ ④ 复制页面生成的连接 URL → ⑤ 接入你的 AI 工具 → ⑥ 验证
```

---

## 第 1 步 · 注册并登录 Bright Data 控制台

1. 浏览器打开控制台：**https://brightdata.com/cp**
2. 没有账号就用邮箱注册（新账户通常自带免费额度 / 试用额度）；有账号直接登录。

---

## 第 2 步 · 打开 MCP 配置页

1. 登录后看**左侧边栏** → 找到 **AI Gateways（人工智能网关）**。
2. 点进去 → 菜单里选 **MCP**。
3. 点 **Configure MCP（配置 MCP）**，进入工具组选择页面。

---

## 第 3 步 · 勾选要开通的工具组（免费组必选，付费按需）

页面是一个工具组清单。**只有第一个 `Search & Extract` 免费，其余都按量付费**——参照下表勾选：

| 工具组 | 费用 | 装了能干嘛 | VOC 要不要 |
|--------|------|-----------|-----------|
| **Search & Extract** | ✅ **免费**（5,000 次/月）| 通用搜索 + 网页抓取：搜目标 URL、把任意页面抓成 Markdown / HTML、批量抓取 | **必选** —— 工作流主力，光靠它就能抓 Amazon / Reddit / YouTube 等页面 |
| **E-commerce** | 付费 | Amazon 等电商**专用**工具，评论 / Q&A 直接返回干净的结构化数据 | 可选 —— 评论量大时更省事、更整齐 |
| **Social Media** | 付费 | Reddit / YouTube / Instagram / TikTok / X 的专用抓取工具 | 可选 —— 想要社媒结构化数据时再开 |
| **App Stores** | 付费 | Apple App Store / Google Play 评论专用工具 | 可选 —— 重点分析 App 体验时开 |
| **Browser Automation** | 付费 | 浏览器自动化（点击、翻页、JS 渲染） | 一般用不上 |
| **Finance / Business / Research / Travel** | 付费 | 金融行情、企业情报、代码仓库、酒店点评等 | VOC 用不上，不用开 |

> 💡 **预算有限就只勾 `Search & Extract`（免费）。** 工作流会用它的 `scrape_as_markdown` 去抓各平台页面（已验证可行），其余缺口交给免费备选体系补上，照样出完整报告。付费组以后想用了随时回来加。

勾完点页面右下角 **Continue to Configure（继续配置）**。

---

## 第 4 步 · 复制页面生成的连接 URL

确认工具组后，**页面下方会自动生成一条专属连接 URL，点「复制」即可**——里面的令牌（token）和工具组（groups）都已自动填好，**你不用手动拼、也不用改任何东西**。它长这样（已脱敏）：

```
https://mcp.brightdata.com/sse?token=【一串你的令牌】&groups=【你开通的组】
```

- 实际示例（脱敏）：`https://mcp.brightdata.com/sse?token=6427fd87-…-ae1739bc4df8&groups=advanced_scraping`
  - `advanced_scraping` 就是免费组 **Search & Extract** 的内部代号；多开几个组时 `groups=` 会自动列出多个。
- ⚠️ **这条 URL 等同账号密码**（含你的令牌）——别发群里 / 截图外传；若泄露，回控制台吊销令牌后重新生成。
- 💡 后面接入任何 AI 工具，用的都是这同一条 URL。其中 `token=` 后面那串就是你的 **API Token**（个别"本地命令"接法会单独用到，见第 5 步接法二）。

---

## 第 5 步 · 把 URL 接入你的 AI 工具（按你用的工具选一种）

> **原理（一句话）：** 把第 4 步复制的那条 URL 填进所用 AI 工具的「MCP / 连接器」设置里就行。**不同工具只是入口不同，URL 完全一样。**
>
> 大多数工具支持两种接法，任选其一：
> - **接法一 · 远程 URL（最简单，推荐）：** 直接填第 4 步复制的 URL，无需安装任何东西。
> - **接法二 · 本地命令（需先装 Node.js）：** 用命令 `npx -y @brightdata/mcp`，并设环境变量 `API_TOKEN` = URL 里 `token=` 那串。

### 5A · Claude 桌面端 ⭐（我们验证过，最省事）

1. 打开 **Claude 桌面应用**。
2. 左下角头像/菜单 → **Settings**（或 **Customize**）。
3. 找到 **Connectors（连接器）** → 点 **Add custom connector（添加自定义连接器）**。
4. **Name** 填 `Bright Data`；**URL** 粘贴第 4 步复制的完整链接。
5. 保存 → 按提示**连接 / 授权**。列表里出现 "Bright Data（已连接）" 即成功。

### 5B · Claude Code CLI（命令行版）

1. 先确认装了 Claude Code：终端输入 `claude --version`，有版本号即可。
2. 执行下面这条命令（**URL 必须用英文双引号包住**，因为里面有 `&`；URL 是 `/sse` 端点，所以 `--transport` 用 `sse`）：
   ```bash
   claude mcp add --transport sse brightdata "你第 4 步复制的完整 URL"
   ```
   - 想让所有项目都能用，加 `-s user`：`claude mcp add -s user --transport sse brightdata "…同上…"`
3. 验证：输入 `claude mcp list`，看到 `brightdata` 且状态为 ✓ Connected 即成功。
4. 重配 / 删除：`claude mcp remove brightdata` 后重新添加。
   - 备选（本地命令，需先装 Node.js）：
     ```bash
     claude mcp add brightdata --env API_TOKEN=【URL里的token那串】 -- npx -y @brightdata/mcp
     ```

### 5C · 其它 AI 工具（Cursor / Windsurf / VS Code / Cline 等）

支持 MCP 的工具，基本都是在它的「MCP 设置」里**新增一个 server**，把下面的 JSON 填进去即可（这是通用写法）：

**通用 JSON · 接法一（远程 URL，推荐）**
```json
{
  "mcpServers": {
    "brightdata": {
      "url": "你第 4 步复制的完整 URL"
    }
  }
}
```
**通用 JSON · 接法二（本地命令，需先装 Node.js）**
```json
{
  "mcpServers": {
    "brightdata": {
      "command": "npx",
      "args": ["-y", "@brightdata/mcp"],
      "env": { "API_TOKEN": "【URL里的token那串】" }
    }
  }
}
```

各工具「入口在哪」（具体以各家官方文档为准）：

| 工具 | 入口 / 配置文件 |
|------|----------------|
| **Cursor** | Settings → **MCP** → Add new MCP server；或编辑 `~/.cursor/mcp.json` |
| **Windsurf** | Settings → **Cascade → MCP**；或编辑 `~/.codeium/windsurf/mcp_config.json` |
| **VS Code（1.102+ 原生支持）** | 命令面板输入 **MCP: Add Server**；或编辑 `.vscode/mcp.json`（注意 VS Code 用 `"servers"` + `"type":"sse"` + `"url"`，字段名与上面 JSON 略不同） |
| **Cline / Continue（VS Code 插件）** | 插件内 **MCP Servers → Edit / Configure**，JSON 用上面的 `mcpServers` 写法 |
| **其它任何支持 MCP 的工具** | 在设置里搜 **"MCP"**，找到"添加 server / 连接器"处，类型选 **Remote / URL（SSE）**，URL 填第 4 步复制的链接 |

> **找不到入口？记住一句话：** 在你的工具里搜关键词 **"MCP"**，找到能「添加 server / 连接器」的地方，**优先粘那条 URL**；若它只接受"命令"不接受 URL，就改用接法二的 `npx` 写法（先装好 Node.js）。

---

## 第 6 步 · 验证连接成功

1. **重启 / 重连**你的 AI 工具（让它重新加载 MCP 连接）。
2. 确认工具已挂载：可用工具里应出现名字含 `search_engine`、`scrape_as_markdown`、`scrape_as_html` 等的 MCP 工具。
3. 让 AI 做一次最小测试（搜一次产品、或抓一个亚马逊页），能返回数据即代表设置成功——这正是工作流的 **Step 2 小范围试爬验证**。
4. 回到工作流，回复一句「**已连接**」，即可继续。

---

## 额度与计费

- **免费额度：** `Search & Extract` 组约 **5,000 次调用 / 月**（以控制台 **Billing / Usage** 页面实际显示为准）。
- **查用量：** 控制台 → **Billing / Usage** 可随时查看本月已用额度。
- **额度耗尽怎么办：** 不用担心——工作流会自动切到免费备选工具（Reddit `.json` API、YouTube Data API、iTunes RSS、`r.jina.ai` 等，详见 `02_VOC数据源清单.md`），照常出报告。

---

## 常见问题排查

| 现象 | 原因 / 解决 |
|------|------------|
| 连接器加了但工具没出现 | 重启 AI 工具；检查 URL 是否**完整复制**（别漏掉尾部 `&groups=…`） |
| 提示 Token 无效 / 401 | URL 复制不全，或令牌已被吊销；回控制台 Configure MCP 重新复制 |
| 某工具返回 `customer is not active` | 该工具属于**付费组、还没开通**；回控制台 Configure MCP 勾选对应组，或让工作流改用免费的 `scrape_as_markdown` 替代 |
| 返回空 / 频繁超时 | 可能触发限速或额度不足；让工作流切免费备选体系 |
| 不想付费 | 只勾免费的 `Search & Extract` 组即可，工作流照样出报告（部分平台改用通用抓取，速度稍慢） |
