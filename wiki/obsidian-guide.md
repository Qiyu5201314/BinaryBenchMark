---
title: Obsidian 使用指南
type: guide
status: stable
created: 2026-10-29
updated: 2026-11-01
sources: []
tags: [guide, obsidian, workflow]
---

# Obsidian 使用指南

> 这套 Vault 的目标：把「读论文 → 提取 → 研究 → 产出」变成一个可持续积累的闭环。
> Obsidian 负责浏览、搜索、连接和复盘；Agent 负责写入、交叉引用和巡检。

## 1. 目录结构

```text
BinaryBenchMark/
├── AGENTS.md                  # 维护协议：三层架构、页面规范、Ingest/Query/Lint
├── README.md                  # 项目总览
├── raw/                       # 原始素材（只增不改）
│   ├── README.md              # 素材索引
│   ├── notes/                 # 作者笔记 / 研究总纲
│   ├── papers/                # 论文 PDF + arXiv HTML
│   ├── github/                # GitHub README / 文档 / LICENSE
│   ├── official/              # 官方项目页、实验室博客、厂商报告
│   └── articles/              # 公众号 / 实验室解读
└── wiki/                      # LLM 维护的知识库
    ├── dashboard.md           # 工作台
    ├── index.md               # 全库目录
    ├── overview.md            # 领域总览
    ├── log.md                 # 活动日志
    ├── entities/              # 基准、平台、组织
    ├── concepts/              # 方法、指标、评测范式
    ├── sources/               # 一份素材一页摘要
    ├── comparisons/           # 横向对比
    ├── research/              # 研究地图、开放问题、方法笔记
    ├── projects/              # 自己的研究项目
    ├── _templates/            # 模板
    ├── inbox/                 # 快速捕获
    └── journal/               # 每日记录
```

## 2. 每天先打开哪里

| 入口 | 用途 |
|---|---|
| [[dashboard]] | 待处理、最近更新、类型统计、孤立页、来源清单 |
| [[index]] | 全库目录，按实体/概念/来源/对比/研究/项目分类 |
| [[log]] | 活动日志，记录每次 Ingest、Query、Lint |
| `raw/README.md` | 原始素材索引，找 PDF/README/官方页 |

> Dashboard 的 Dataview 查询需要在**阅读视图**（`Ctrl+E`）下渲染；编辑视图里显示为代码块是正常的。

## 3. 三个 QuickAdd 命令

在命令面板（`Ctrl+P`）中搜索并运行：

| 命令 | 作用 | 结果 |
|---|---|---|
| `QuickAdd: Capture Source` | 录入一篇论文/文章/官方文档 | 提示「来源标题」，用 `wiki/_templates/source.md` 在 `wiki/sources/` 创建来源页 |
| `QuickAdd: Capture Idea` | 随手记一个想法 | 在 `wiki/inbox/Inbox.md` 追加 `- 日期时间 想法`；文件不存在会自动创建 |
| `QuickAdd: Daily` | 打开今天的日记 | 在 `wiki/journal/` 打开/创建当天日记，套用 `wiki/_templates/daily.md` |

建议到 **设置 → 快捷键** 给这三个命令绑定快捷键，例如：

- `Ctrl+Alt+S`：Capture Source
- `Ctrl+Alt+I`：Capture Idea
- `Ctrl+Alt+D`：Daily

## 4. 每天 10 分钟

1. `Ctrl+Alt+D` 打开今日日记。
2. 写下今天要读的论文 / 要解决的问题。
3. 读论文时用 `Ctrl+Alt+I` 记录想法，不要直接改 Wiki 页面。
4. 晚上打开 [[dashboard]]：
   - 看「待处理（draft / needs-review）」；
   - 处理 `wiki/inbox/` 里未归档的想法；
   - 在日记里写「今日输入 / 想法 / 待归档」。

## 5. 每周 30 分钟

1. 打开 [[dashboard]]，看「孤立页（无入链）」：给孤立页补链接，或合并。
2. 看「按类型统计」：确认没有页面类型失控。
3. 看「最近更新」：检查本周新增/修改的页面。
4. 让 Agent 按 `AGENTS.md` 跑一次 Lint：
   - 矛盾、过时、孤立页、缺失链接；
   - 是否需要新增/合并实体页或概念页。
5. 在 [[log]] 追加一条周检记录。

## 6. 读一篇论文的完整流程

### 6.1 放入素材
- 把 PDF 放进 `raw/papers/`。
- 如果是 GitHub/官方页，放进 `raw/github/` 或 `raw/official/`。

### 6.2 在 Obsidian 里读
- 打开 PDF，使用 **PDF++** 高亮/批注。
- 关键段落用 PDF++ 复制引用，粘贴到来源页或日记。
- 需要全文搜索时，用 **Omnisearch**（命令面板搜索 `Omnisearch`）：
  - 已启用 **Text Extractor**，PDF/图片文字会被抽取后参与搜索；
  - 首次索引大 PDF 可能需要一点时间。

### 6.3 建来源页
- 运行 `QuickAdd: Capture Source`。
- 输入论文标题（例如 `CyberGym`），得到 `wiki/sources/cybergym.md`。
- 手动补：作者、年份、链接、核心结论、关键数字。

### 6.4 让 Agent 提取
对 Agent 说：

```text
按 AGENTS.md 提取 raw/papers/xxx.pdf
```

Agent 会：
- 完善来源页；
- 更新相关 `entities/`、`concepts/`；
- 建立双向链接；
- 在 `wiki/log.md` 追加记录。

### 6.5 检查
- 来源页的 `sources:` 是否指向正确的 raw 文件。
- 关键数字是否带口径（版本、日期、范围）。
- 新页面是否有入链（从 [[index]] 或相关实体页链接过来）。

## 6.6 新增素材/笔记怎么放

先判断三件事：**是不是外部来源、内容还会不会改、是否需要长期引用**。

| 情况 | 放哪里 | 推荐做法 |
|---|---|---|
| 论文 PDF | `raw/papers/` | 放入 PDF → `QuickAdd: Capture Source` 建来源页 → 让 Agent 提取 |
| GitHub README / 文档 | `raw/github/` | 保存 README/文档全文 → 建来源页 → Agent 提取 |
| 官方项目页 / 实验室博客 / 厂商报告 | `raw/official/` | 保存正文或 HTML → 建来源页 |
| 公众号 / 解读文章 | `raw/articles/` | 保存全文，顶部注明原始 URL 和抓取日期 |
| 自己的研究总纲 / 长笔记 | `raw/notes/` | 作为“作者原始笔记”保存，只增不改；让 Agent 建摘要或研究页 |
| 一句话想法 / 临时灵感 | `wiki/inbox/Inbox.md` | 直接 `QuickAdd: Capture Idea` |
| 正在演化的研究判断 / 方法笔记 | `wiki/research/` | 让 Agent 整理成研究页，保留来源引用 |
| 项目方案 / 开题 / 论文草稿 | `wiki/projects/` | 作为一个 project 页，引用 Wiki 里已有页面 |
| 截图 / 图片 / 附件 | `wiki/_attachments/` | 在来源页或研究页中用 `![[...]]` 引用 |
| 会议/讨论记录 | `raw/notes/` 或 `wiki/research/` | 原始记录放 raw；结构化结论放 research |

### 命名建议

```text
raw/papers/CyberGym_arXiv_2506.02548v3.pdf
raw/github/CyberGym_README.md
raw/official/Anthropic_SystemCard_2025.md
raw/articles/WeChat_二进制闭卷_2026-10-29.md
raw/notes/二进制闭卷测试指南.md
```

### 三种入口

1. **外部素材（推荐走 QuickAdd）**
   - 文件放进 `raw/` 对应子目录；
   - `Ctrl+P` → `QuickAdd: Capture Source`；
   - 输入标题，得到 `wiki/sources/<标题>.md`；
   - 对 Agent 说：
     ```text
     按 AGENTS.md 提取 raw/papers/xxx.pdf，更新相关实体/概念页，并在 log.md 追加记录。
     ```

2. **临时想法**
   - `Ctrl+P` → `QuickAdd: Capture Idea`；
   - 输入想法，自动追加到 `wiki/inbox/Inbox.md`；
   - 每周整理时再决定归档到 `concepts/`、`research/` 或删除。

3. **自己的长笔记**
   - 如果它是“原始材料/不会回头改”：放 `raw/notes/`，再让 Agent 建摘要；
   - 如果它是“会持续演化的综合判断”：直接让 Agent 建到 `wiki/research/`，并引用来源页。

### 添加后检查

- [ ] 文件是否在正确的 `raw/` 子目录；
- [ ] 来源页的 `sources:` 是否指向该 raw 文件；
- [ ] 关键数字是否带口径（版本、日期、范围）；
- [ ] 是否至少有一个入链（从 [[index]]、实体页或研究页链接过来）；
- [ ] [[log]] 是否追加了本次操作。

> 批量素材不要一次全丢给 Agent。建议一次 1–3 篇，先确认提取质量，再继续。

## 6.7 Wiki 更新后的验收与阅读

每次 Agent 完成 Ingest、Query 回灌或 Lint 后，不要重读全库。按下面顺序看。

### 第一步：看变更

1. 打开 [[log]]，看最后一条记录：新增/修改了哪些页面。
2. 打开 [[dashboard]]：
   - 待处理（draft / needs-review）
   - 孤立页（无入链）
   - 按类型统计
3. 打开 [[index]]：新页面是否已经登记。

### 第二步：按更新类型读变更页

| 更新类型 | 优先读 | 再读 | 只在需要时读 |
|---|---|---|---|
| 新论文/素材 Ingest | 新来源页的「核心结论 / 关键数字」 | 被它更新的 2–4 个实体/概念页 | 原始 PDF 的争议段落 |
| Query 回灌 | 新增/修改的 research / comparison 页 | 它引用的来源页 | 原始数据 |
| Lint | [[log]] 中的变更摘要 | 被修改的页面 | 冲突涉及的原文件 |
| 项目产出 | project 页 | 它引用的 concepts | 相关 sources |

> 不要从第一页读到最后一页。只读「本次变更涉及的页面」和它们的直接邻居。

### 第三步：核对四件事

- [ ] 来源页的 `sources:` 指向正确的 raw 文件；
- [ ] 关键数字带口径（版本、日期、范围）；
- [ ] 新页面至少有一个入链；
- [ ] 矛盾/过时内容已写入 [[open-questions]]。

### 第四步：决定下一步

- **接受**：内容正确，不改，继续使用。
- **让 Agent 修**：指出具体页面和问题，例如「cybergym-paper 的 22% 与榜单 95% 口径冲突，请统一」。
- **暂缓**：把问题加入 `wiki/inbox` 或 [[open-questions]]，不要当场硬改。

### 5 分钟版

1. 看 [[log]] 最后一条。
2. 看 [[dashboard]] 的「待处理」。
3. 读 1 个新来源页 + 1 个被更新的概念页。
4. 确认 `sources:` 路径和关键数字。
5. 决定：接受 / 让 Agent 修 / 暂缓。

### 让 Agent 给你变更报告

```text
请给我本次 Wiki 更新的变更报告：
1. 新增/修改的页面清单
2. 每个页面涉及哪些 raw 来源
3. 新出现或仍未解决的矛盾
4. 建议我优先阅读的 3 个页面及原因
5. 是否有孤立页或缺失链接
```

## 6.8 看完 Dashboard 之后怎么操作

按 Dashboard 的区块顺序处理，不要跳着看。

### 1. 待处理（draft / needs-review）

逐页打开，按页面类型判断：

| 页面类型 | 检查什么 | 通过后 |
|---|---|---|
| source | `sources:` 路径、关键数字口径 | 改 `status: stable` |
| entity / concept | 定义清楚、至少 2 个入链/出链 | 改 `status: stable` |
| research | 有证据链、遗留问题已记录 | 改 `status: stable` |
| project | 下一步动作明确、状态准确 | 更新 `status` / 下一步 |

不能确认的，保留 `needs-review`，并在页面里写一句「待确认：……」。

### 2. 最近更新

- 看最近 5 条，确认这些改动是你预期的。
- 打开 [[log]] 对应记录，核对新增/修改页面。
- 如果出现意外改动，让 Agent 解释或回滚。

### 3. 按类型统计

- 看 `sources` 是否明显多于 `concepts` / `entities`。
- 如果是，说明「读得多、提炼少」，让 Agent 提出本轮该补哪些概念页/实体页。

### 4. 孤立页（无入链）

- 优先从 [[index]]、相关 entity/concept/research 页补链接。
- 找不到归属的：合并到相近页面，或移到 `wiki/inbox` 等待归档。
- 不要为了消掉孤立页而硬加无意义链接。

### 5. 研究页与项目页

- 选一个当前最重要的 research/project 页。
- 写清「下一步动作」：读哪篇、补哪个实验、问 Agent 哪个问题。
- 完成后更新状态和 [[log]]。

### 6. 来源清单

- 看最新来源页是否都有对应的 raw 文件。
- 新来源是否已进入相关实体/概念页，而不是只躺在 sources 里。

### 7. 快速操作

- 新素材：`QuickAdd: Capture Source`
- 临时想法：`QuickAdd: Capture Idea`
- 今日记录：`QuickAdd: Daily`

### 5 分钟版

1. 处理 1–2 个「待处理」页。
2. 给 1 个孤立页补入链。
3. 选 1 个 research/project 页写下下一步。

### 15 分钟版

1. 清空「待处理」。
2. 处理「孤立页」。
3. 看「按类型统计」，决定本轮补哪些概念页。
4. 让 Agent 按 `AGENTS.md` 跑一次 Lint。
5. 在 [[log]] 追加周检记录。

### 可以直接发给 Agent 的指令

```text
根据 dashboard 的待处理列表，逐页检查：
1. sources 路径是否正确
2. 关键数字是否有口径
3. 是否至少有一个入链
能确认的改为 stable；不能确认的保留 needs-review 并写明原因。
最后更新 log.md，并给我一份变更摘要。
```

```text
为 dashboard 孤立页列表中的页面补入链：
优先从 index、相关 entity/concept/research 页面链接；
无法归类的合并或移到 wiki/inbox。
完成后更新 log.md。
```

## 7. 回答一个研究问题

1. 先问 Agent，要求回答带来源页引用。
2. 如果答案有复用价值，让 Agent **回灌**成新页面：
   - 概念性答案 → `wiki/concepts/`；
   - 跨对象比较 → `wiki/comparisons/`；
   - 研究判断/假设 → `wiki/research/`；
   - 可执行方案 → `wiki/projects/`。
3. 不要只把答案留在聊天记录里。

## 8. 插件速查

| 插件 | 怎么用 | 备注 |
|---|---|---|
| **Dataview** | 打开 [[dashboard]]，切阅读视图 | 工作台查询的核心 |
| **QuickAdd** | `Ctrl+P` 搜 `QuickAdd: Capture Source / Capture Idea / Daily` | 三个命令已配置 |
| **Omnisearch** | `Ctrl+P` 搜 `Omnisearch` | 全文搜索，支持 PDF/OCR |
| **Text Extractor** | 通常由 Omnisearch 调用 | 首次使用会建立索引 |
| **PDF++** | 直接打开 `raw/papers/` 里的 PDF | 高亮、批注、复制引用 |
| **Linter** | `Ctrl+P` 搜 `Linter: Lint current file` | 规则暂用默认，可按需配置 |
| **Templater** | `Ctrl+P` 搜 `Templater: Insert template` | 模板目录 `wiki/_templates` |
| **Calendar** | 右侧边栏日历，点日期打开日记 | 与 Daily notes 联动 |
| **Periodic Notes** | `Ctrl+P` 搜 `Periodic Notes` | 周记/月记模板尚未配置 |
| **Excalidraw** | `Ctrl+P` 搜 `Excalidraw: Create new drawing` | 画研究框架/攻击链 |
| **Kanban** | `Ctrl+P` 搜 `Kanban: Create new board` | 管理论文阅读队列 |
| **Git** | 插件设置里配置自动提交 | 仓库已 `git init`；如提示不是仓库，重启 Obsidian |
| **Style Settings** | 设置 → Style Settings | 调整 Minimal 主题的字体/间距/颜色 |

## 9. 常用快捷键

| 操作 | 快捷键 |
|---|---|
| 快速切换笔记 | `Ctrl+O` |
| 命令面板 | `Ctrl+P` |
| 全局搜索 | `Ctrl+Shift+F` |
| 关系图谱 | `Ctrl+G` |
| 新建笔记 | `Ctrl+N` |
| 切换阅读/编辑 | `Ctrl+E` |
| 打开设置 | `Ctrl+,` |
| 保存 | `Ctrl+S` |

> 自定义快捷键在 **设置 → 快捷键** 中绑定；建议先绑 QuickAdd 的三个命令。

## 10. 维护规则

1. **`raw/` 只增不改**：原始论文、README、网页、文章、作者笔记是事实依据。
2. **`wiki/` 由 LLM 维护**：摘要、实体页、概念页、交叉引用、冲突处理交给 Agent。
3. **数字必须带口径**：来源、版本、日期、范围缺一不可。
4. **先读来源，再下结论**：关键事实要能追溯到 `wiki/sources/` 和 raw 文件。
5. **每次操作写 [[log]]**：Ingest、Query、Lint 都追加一条。

## 11. 常见问题

### Dataview 查询显示为代码块
- 切换到阅读视图：`Ctrl+E`。
- 或确认 **设置 → 第三方插件 → Dataview** 已启用。

### QuickAdd 命令找不到
- `Ctrl+P` 搜 `QuickAdd`，确认能看到 `QuickAdd: Capture Source` 等命令。
- 如果看不到，到 **设置 → 第三方插件** 点「重新加载插件」。

### PDF 搜不到内容
- 确认 **Text Extractor** 已启用。
- 在 Omnisearch 设置里确认 PDF/OCR 索引已开启。
- 大文件首次索引需要时间。

### Git 插件提示不是仓库
- 仓库已在 `BinaryBenchMark` 根目录初始化。
- 重启 Obsidian，或在第三方插件页点「重新加载插件」。
- 在 Git 插件设置里配置自动提交间隔。

### 模板页出现在 Dashboard
- 已在 Dataview 查询中排除 `_templates`。
- 如果还有，检查查询是否被改回旧版本。

## 12. 建议的下一步

- [ ] 给三个 QuickAdd 命令绑定快捷键。
- [ ] 配置 Linter 规则（标题层级、frontmatter 字段、空行）。
- [ ] 配置 Periodic Notes 的周记/月记模板（可复用 `daily`）。
- [ ] 在 Git 插件里开启自动提交（例如每 30 分钟）。
- [ ] 配置关系图谱分组：entities 红、concepts 蓝、sources 灰、research 橙、projects 紫。
- [ ] 用 `AGENTS.md` 的 Lint 流程做第一次全库巡检。

## 相关页面

- [[dashboard]]
- [[index]]
- [[log]]
- [[overview]]
- [[binary-closed-book-guide]]
