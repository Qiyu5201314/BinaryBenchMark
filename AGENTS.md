# AGENTS.md — BinaryBenchMark 知识库维护协议

> 版本：v1.1（2026-11-01）
> 变更：新增「辅助页」schema（index / overview / dashboard / log / guide），明确其 frontmatter、统计与 Lint 规则。
> 适用对象：任何具备本地文件读写能力的 AI Agent（TraeCode / Claude Code / Codex / Gemini CLI 等）
> 本文件是这套 Wiki 的 **schema**：它定义知识如何组织、如何引用、如何演化。修改本文件会改变后续所有会话的行为，请谨慎。

---

## 0. 这个库是什么

**研究对象**：AI Agent 在网络安全方向的能力评测，尤其是「二进制/漏洞利用」相关的基准、靶场、执行框架与评测方法。

**当前核心对象**：
- CyberGym：以真实历史漏洞 PoC 复现为核心的大规模基准（开卷式）。
- AgentCyberRange：以 Web 打点 + 内网后渗透为核心的闭卷式靶场评测。
- CAGE：支撑 AgentCyberRange 的通用评测执行框架。

**最终服务的研究问题**（见 `wiki/projects/binary-closed-book-eval.md`）：
> 如何构建一套「二进制闭卷评测」，在仅给目标入口/二进制、不给漏洞描述与源码的条件下，测量 Agent 自主发现并利用漏洞的能力？

---

## 1. 三层架构

| 层 | 位置 | 谁拥有 | 规则 |
|---|---|---|---|
| **Raw sources** | `raw/papers/`、`raw/github/`、`raw/official/`、`raw/articles/`、`raw/notes/`、`raw/README.md` | 人 | **只增不改**。原始论文、README、网页、文章全文、作者笔记。事实的最终依据。 |
| **The wiki** | `wiki/` | LLM | LLM 创建、更新、交叉引用、维护一致性。人可以读，但不要手工维护链接。 |
| **The schema** | `AGENTS.md`（本文件） | 人 + LLM | 操作手册。能力不足时优先改这里，而不是在页面里打补丁。 |

> 说明：原始素材统一放在 `raw/` 下；`raw/` 只增不改。`wiki/` 是 LLM 维护层，`AGENTS.md` 是 schema。新增素材放入 `raw/` 的对应子目录，并在 `raw/README.md` 或 `wiki/sources/` 中登记。

---

## 2. 目录结构

```text
BinaryBenchMark/
├── AGENTS.md                  # 本文件：schema
├── README.md                  # 项目总览（指向 wiki 与 raw）
├── raw/                       # 原始素材层（只增不改）
│   ├── README.md              # 原始素材索引
│   ├── notes/                 # 作者笔记 / 研究总纲
│   ├── papers/                # 论文 PDF + arXiv HTML
│   ├── github/                # GitHub README / 文档 / LICENSE
│   ├── official/              # 官方项目页、实验室博客、厂商报告
│   └── articles/              # 公众号 / 实验室解读
└── wiki/                      # LLM 维护层
    ├── index.md               # 全库目录 + 查询入口（辅助页）
    ├── dashboard.md           # Dataview 工作台（辅助页）
    ├── log.md                 # 追加式活动日志（辅助页）
    ├── overview.md            # 领域总览与阅读地图（辅助页）
    ├── obsidian-guide.md      # Obsidian 使用指南（辅助页）
    ├── _templates/            # Obsidian 模板（source/entity/concept/daily）
    ├── entities/              # 实体：基准、平台、组织、数据集
    ├── concepts/              # 概念：方法、指标、评测范式
    ├── sources/               # 来源摘要：一份素材一页
    ├── comparisons/           # 对比：跨对象横向分析
    ├── research/              # 研究：问题地图、开放问题、方法笔记
    └── projects/              # 项目：自己的研究计划与产出
```

---

## 3. 页面类型与命名

文件名一律 **英文 kebab-case**，页面标题用中文，便于 Obsidian 链接与跨平台兼容。页面分两类：**内容页**与**辅助页**。

### 3.1 内容页（content pages）

| 类型 | 目录 | 命名示例 | 职责 |
|---|---|---|---|
| entity | `entities/` | `cybergym.md` | 一个基准/平台/组织/数据集是什么、规模、结果、局限 |
| concept | `concepts/` | `open-book-vs-closed-book.md` | 一个可复用的评测概念、范式、指标或方法论 |
| source | `sources/` | `cybergym-paper.md` | 一份原始素材的结构化摘要 + 可引用要点 |
| comparison | `comparisons/` | `cybergym-vs-agentcyberrange.md` | 两个及以上对象的横向对比 |
| research | `research/` | `open-questions.md` | 研究问题、方法、空白、下一步 |
| project | `projects/` | `binary-closed-book-eval.md` | 自己的研究项目页（目标、假设、里程碑） |

> 来源页与实体页可能指向同一对象（如 `sources/cve-bench-paper.md` 与 `entities/cve-bench.md`）。**来源页文件名必须加 `-paper` / `-docs` / `-article` 等后缀**，避免与实体页同名导致 Obsidian wikilink 歧义。

### 3.2 辅助页（auxiliary pages）

辅助页不放具体研究结论，只承担导航、总览、工作台、日志与操作说明。

| 类型 | 文件 | 职责 | 约束 |
|---|---|---|---|
| index | `wiki/index.md` | 全库目录、快速入口、统计 | 必须列出所有内容页；统计与实际一致 |
| overview | `wiki/overview.md` | 领域总览、阅读地图 | 可以引用内容页；不承载具体事实的唯一来源 |
| dashboard | `wiki/dashboard.md` | Dataview 查询工作台 | 只读查询，不写结论 |
| log | `wiki/log.md` | 追加式活动日志 | 只追加，不修改历史条目；不参与内容统计 |
| guide | `wiki/obsidian-guide.md` | Obsidian 使用指南、工作流说明 | 可含操作示例；示例中的 `[[...]]` 不算真实链接 |

**辅助页规则**：

- 辅助页**不参与** `index.md` 的「内容页合计」统计，单独列为「辅助页」。
- 辅助页不是 entity/concept/source，**不应被当作知识来源引用**。
- `log.md` 是历史记录：允许保留旧结论，但必须在新条目中说明更新，不得回头改写历史。
- `dashboard.md` 只做查询，不写判断；统计口径以 `index.md` 为准。
- `guide` 页可以包含 `[[...]]`、`![[...]]` 等语法示例；Lint 应跳过代码块与行内代码，不把它们当断链。
- 辅助页仍必须有完整 frontmatter；`sources` 可以为 `[]`。

**一页一主题**。不要在一页里塞两个实体；用 `[[链接]]` 连接。

---

## 4. Frontmatter 规范

每个页面必须有 YAML frontmatter：

```yaml
---
title: CyberGym
type: entity
status: stable          # stable | draft | needs-review | deprecated
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/github/CyberGym_README.md
tags: [benchmark, vulnerability-reproduction, binary]
---
```

- `type` 必须是第 3.1 / 3.2 节定义的类型；未定义的类型应先改本文件，再建页。
- `sources` 写 **raw 层相对路径**，不是 URL；URL 写在正文「来源」小节。
- `status=draft` 表示内容尚未与原文逐条核对；`needs-review` 表示存在已知冲突或口径疑问。
- 辅助页（index / overview / dashboard / log / guide）允许 `sources: []`；只有直接引用 raw 时才填写。
- 修改页面必须更新 `updated`。

---

## 5. 写作与引用规范

### 5.1 事实必须可追溯

每个关键事实后面标注来源页，例如：

```markdown
CyberGym 包含 1,507 个漏洞实例、188 个开源项目。来源：[[cybergym-paper]]
```

来源页内部再列 raw 路径 + 原始 URL。**禁止**在没有来源页的情况下直接断言数字。

### 5.2 区分三种语气

- 原文事实：直接陈述，附来源。
- 综合推断：用 `> [推断]` 引用块，说明依据。
- 待验证：用 `> [待验证]`，写明缺什么证据。

### 5.3 数字必须带口径

同一对象在不同版本/时间可能有不同规模。凡数字，尽量写全：

```markdown
AgentCyberRange（论文版，arXiv v2，2026-06）：110 漏洞 / 15 Web 应用 / 8 内网 / 156 主机
AgentCyberRange（复旦文章，2026-07）：25 Web 应用 / 近 500 漏洞 / 30 内网 / 近 400 靶机
```

**禁止**把不同口径的数字拼成一句话。遇到冲突，在页面内设「口径差异」小节，并标 `status: needs-review`。

### 5.4 链接规则

- 内部链接：`[[cybergym]]`、`[[open-book-vs-closed-book]]`。
- 外部链接：原始 URL 用普通 Markdown 链接。
- raw 文件：用反引号写相对路径，如 `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`。
- 不在链接文本里加反引号（Obsidian 不识别）。

### 5.5 语言

正文中文；专有名词、指标、API 名保留英文原文。首次出现时给中文解释。

---

## 6. 三大工作流

### 6.1 Ingest（提取）

当用户说「把这份新材料加进 Wiki」或往 raw 目录放入新文件：

1. **读全文**，不要只读摘要。
2. 在 `sources/` 建来源页：元数据、核心主张、关键数字、方法、局限、可引用原句、raw 路径 + URL。
3. 更新 `index.md`：加一行链接 + 一句话摘要。
4. 更新受影响的 `entities/`、`concepts/`、`comparisons/` 页面；缺页则新建。
5. 若新材料与旧结论冲突，在两边页面标注，并在 `research/open-questions.md` 记录。
6. 追加 `log.md`：

```markdown
## [2026-10-29] ingest | 材料标题
- 新建：[[source-page]]
- 更新：[[cybergym]]、[[benchmark-validity]]
- 冲突/待验证：……
```

**一次只处理一个来源，人工复核后再继续**（Karpathy 原始建议）。

### 6.2 Query（查询）

1. 先读 `index.md` 定位相关页面。
2. 深入页面，必要时回读 raw 原文。
3. 回答必须带 `[[来源页]]` 引用。
4. 如果回答产生了新的综合结论，**回灌**为新的 concept/comparison/research 页面，而不是只留在对话里。
5. 追加 `log.md` 的 query 记录。

### 6.3 Lint（巡检）

建议每新增约 10 个来源跑一次：

| 检查 | 动作 |
|---|---|
| 页面间数字矛盾 | 标 `needs-review`，写清两个口径 |
| 过时结论 | 用新来源覆盖，旧结论保留为「历史口径」 |
| 孤立页（无入链） | 从 `index.md` 或相关页面补链接 |
| 提到的概念没有专页 | 建 stub 页 |
| 缺失交叉引用 | 补 `[[链接]]` |
| 数据缺口 | 记入 `research/open-questions.md` |
| 断链 | 修正文件名或删除死链 |
| 重复页名（同名不同目录） | 来源页加 `-paper` / `-docs` / `-article` 后缀，或重命名 |
| `type` 不在 schema 内 | 归入已有类型；确需新类型时先改本文件 |
| `index.md` 统计与实际不一致 | 分别统计内容页与辅助页，更新「内容页合计」与「辅助页」 |
| frontmatter `sources` 路径不存在 | 修正 raw 路径或删除死链 |
| 辅助页被当作来源引用 | 改引 `sources/` 页或 raw 原文 |

Lint 的产物写入 `log.md`，不要静默修改。Lint 应跳过代码块与行内代码中的 `[[...]]` 示例。

---

## 7. 冲突与版本处理

- 论文版、会议版、官网版、公众号版可能数字不同。**以正式论文为第一优先**，其次官方仓库/官网，最后二手解读。
- 每个数字保留「版本 + 日期 + 范围」三元组。
- 若官网明确说「leaderboard 已接近饱和」，而论文写 22%，两者可能都真实，但测量设置不同；必须在页面解释差异，不能简单取最新。
- 已知冲突集中登记在 `wiki/research/open-questions.md` 的「口径冲突」小节。

---

## 8. 安全、合规与负责任使用

- 本库只做**评测方法研究**，不提供可用于真实攻击的利用步骤。
- raw 中的 PoC、靶场、漏洞细节仅用于受控环境；不得对外部署。
- 引用 CyberGym / AgentCyberRange 的结论时，保留其「dual-use」「responsible disclosure」限定。
- 新增内容若涉及未公开漏洞，必须先确认已按 responsible disclosure 处理，否则不得写入。

---

## 9. 如何扩展

- 新基准：在 `entities/` 建页，并在 `comparisons/benchmark-landscape.md` 加一行。
- 新评测维度（如成本、鲁棒性、防御）：在 `concepts/` 建页，并在 `research/research-map.md` 挂接。
- 新研究想法：写入 `projects/binary-closed-book-eval.md` 或 `research/open-questions.md`，不要散落在对话里。
- 新辅助页：放在 `wiki/` 根目录，`type` 使用 index / overview / dashboard / log / guide；在 `index.md` 的「辅助页」单列，不计入内容页合计。
- 新工作流能力（例如自动抓取 arXiv）：先改本文件，再实现。

---

## 10. Obsidian / Logseq 使用

- `wiki/` 可直接作为 Obsidian vault 打开；`[[wikilink]]` 与 YAML frontmatter 原生兼容。
- 建议把 raw 目录也加入 vault，便于从来源页跳转原文。
- Obsidian 只用于**浏览、图谱、搜索**；写入与交叉引用维护交给 Agent，避免人手改乱。
- 推荐开启：Backlinks、Graph view、Tags、Dataview（可选）。
- `.obsidian/` 默认**整体忽略**（含插件 bundle、个人 workspace、cache）；如需共享配置，用 `!.obsidian/app.json` 等例外规则单独放行。
