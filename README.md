# AKASO VOC report · 使用说明

这是一套**可复用的 VOC（用户之声）调研报告工作流**。把它交给团队任何成员，他们都能用 Claude Code 产出与样板**风格完全一致**的产品洞察报告——数据驱动、面向 CEO 与各部门、Editorial Intelligence 风格（灵感来自 The Economist / Financial Times 数据新闻）。

---

## 一、文件夹里有什么

```
AKASO VOC report/
├── 使用说明.md                         ← 你正在读的这份
└── AKASO-VOC-report/                   ← 这就是 skill 本体（整个文件夹拷走即可用）
    ├── SKILL.md                        ← 工作流主文件（Claude 会自动读取执行）
    └── references/                     ← 工作流引用的全部资料
        ├── 01_VOC分析维度框架.md         ← 分析什么（D0–D15 维度与算法）
        ├── 02_VOC数据源清单.md           ← 数据哪来（含免费备选工具体系）
        ├── 05_BrightData_MCP_设置指南.md ← 怎么连（Bright Data 保姆级设置）
        ├── 03_设计哲学.md                ← 风格立意
        ├── 04_VOC报告设计规范.md          ← 怎么呈现（精确到 px/hex 的规范）
        └── 样板_AKASO_EK7000_VOC报告.html ← 黄金视觉样板
```

---

## 二、怎么安装这个 skill

把整个 **`AKASO-VOC-report/`** 文件夹（注意：是里层那个、含 `SKILL.md` 的文件夹）拷贝到 Claude Code 的 skills 目录：

- **个人级（所有项目可用）**
  - Windows：`C:\Users\<你的用户名>\.claude\skills\AKASO-VOC-report\`
  - macOS / Linux：`~/.claude/skills/AKASO-VOC-report/`
- **项目级（仅当前项目可用）**
  - `<你的项目>/.claude/skills/AKASO-VOC-report/`

拷贝后，`SKILL.md` 必须直接位于 `…/.claude/skills/AKASO-VOC-report/SKILL.md`，`references/` 与它同级。重启 Claude Code 即可识别。

---

## 三、怎么使用

在 Claude Code 里用自然语言触发即可，例如：

- 「帮我做一份 **AKASO Brave 7** 的 VOC 用户洞察报告」
- 「调研一下 **AKASO EK7000** 的口碑、痛点和竞品对比」
- 或显式调用：`/AKASO-VOC-report`

Claude 会自动按工作流推进：

```
问产品型号 → 连接 Bright Data MCP → 小范围试爬验证 → 多源采集评论
          → D0–D15 维度分析 → 用 /canvas-design + 设计规范出 HTML → 自检 → 交付
```

> 如果你在第一句话里就说了产品型号，它不会再问你。

---

## 四、前置依赖

| 依赖                                 | 是否必须         | 说明                                                         |
| ------------------------------------ | ---------------- | ------------------------------------------------------------ |
| Claude Code（支持 Skills）           | ✅ 必须           | 用来运行本 skill                                             |
| `/canvas-design` skill               | ⭕ 推荐（非必须） | 生成阶段的设计增强。**没有也能产出风格一致的报告**——工作流会自动检测，缺失时引导启用，并回退到「设计规范+样板」路径产出 |
| **Bright Data MCP** 账号 + API Token | ⭕ 推荐           | 主用采集工具（每月有免费额度）；保姆级设置见 `references/05_BrightData_MCP_设置指南.md`，首次使用时 Claude 会手把手教你连接 |
| 免费备选工具                         | ——               | **没有 Bright Data 也能用**：Claude 会自动切换到 Reddit `.json` API、YouTube Data API、iTunes RSS、`r.jina.ai` 等免费方案（详见 `references/02`） |

---

## 五、几个重要约定（已写进工作流，团队需知晓）

1. **先小范围验证，再批量采集**——避免连接没配好就浪费配额。
2. **报告默认 HTML 格式**；需要其他格式（PDF/PPT/海报）直接告诉 Claude。
3. **报告里不出现 D0/D1/D2 这类内部维度编号**——读者看到的是「CHAPTER 序号 + 模块名」。
4. **数据必须真实**——所有数字来自实际采集到的评论；缺后台数据（如退货率）会标注「估算」，不编造。
5. **风格统一**——固定配色（奶白底 + 红/琥珀/绿/海军蓝）、衬线大数字、无衬线正文、几何状态符号（◆●○，不用 emoji）。**视觉一致性由 `references/04_VOC报告设计规范.md` + 黄金样板 HTML 锁定，不依赖 canvas-design**；任何不确定处以 `样板_AKASO_EK7000_VOC报告.html` 为准。

---

## 六、想调整风格 / 维度怎么办

- 改**分析维度或指标算法** → 编辑 `references/01_VOC分析维度框架.md`
- 加**新数据源或备选工具** → 编辑 `references/02_VOC数据源清单.md`
- 改**视觉风格（颜色/字号/组件）** → 编辑 `references/04_VOC报告设计规范.md`，并同步更新其 §10 检查清单

这些都是"活文档"，团队可以持续完善——只要保持 `references/` 的文件名不变，工作流会自动跟随最新内容。
