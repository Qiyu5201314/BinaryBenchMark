# BinaryBenchMark

> 研究对象：AI Agent 网络安全能力评测，重点是 CyberGym、AgentCyberRange，以及「二进制闭卷评测」。
> 建立日期：2026-10-29；最近更新：2026-11-01（新增 2023–2026 前沿素材与对比页）

## 结构

```text
BinaryBenchMark/
├── AGENTS.md                  # 维护协议：三层架构、页面规范、Ingest/Query/Lint
├── README.md                  # 本文件：项目总览
├── raw/                       # 原始素材（只增不改）
│   ├── README.md              # 素材索引
│   ├── notes/                 # 作者笔记 / 研究总纲
│   ├── papers/                # 论文 PDF + arXiv HTML
│   ├── github/                # GitHub README / 文档 / LICENSE
│   ├── official/              # 官方项目页、实验室博客、厂商报告
│   └── articles/              # 公众号 / 实验室解读
└── wiki/                      # LLM 维护的知识库
    ├── index.md               # 全库目录 + 查询入口
    ├── dashboard.md           # Obsidian Dataview 工作台
    ├── overview.md            # 领域总览与阅读地图
    ├── log.md                 # 活动日志
    ├── entities/              # 基准、平台、组织、数据集
    ├── concepts/              # 方法、指标、评测范式
    ├── sources/               # 一份素材一页的结构化摘要
    ├── comparisons/           # 跨对象横向对比
    ├── research/              # 研究地图、开放问题、方法笔记
    └── projects/              # 自己的研究项目
```

## 快速开始

| 我想… | 从这里开始 |
|---|---|
| 浏览整个 Wiki | [wiki/index.md](wiki/index.md) |
| 看领域全貌 | [wiki/overview.md](wiki/overview.md) |
| 看 2023–2026 前沿版图 | [wiki/comparisons/binary-closed-book-landscape.md](wiki/comparisons/binary-closed-book-landscape.md) |
| 日常待办/巡检 | [wiki/dashboard.md](wiki/dashboard.md)（需 Dataview） |
| 学 Obsidian 操作 | [wiki/obsidian-guide.md](wiki/obsidian-guide.md) |
| 查原始素材 | [raw/README.md](raw/README.md) |
| 读二进制闭卷总纲 | [raw/notes/二进制闭卷测试指南.md](raw/notes/二进制闭卷测试指南.md) |
| 了解维护规则 | [AGENTS.md](AGENTS.md) |

## 原则

1. **raw 只增不改**：原始论文、README、网页、文章、作者笔记是事实依据。
2. **wiki 由 LLM 维护**：摘要、实体页、概念页、交叉引用、冲突处理都由 Agent 完成。
3. **数字必须带口径**：来源、版本、日期、范围缺一不可；冲突集中记录在 `wiki/research/open-questions.md`。
4. **先读来源，再下结论**：每个关键事实都要能追溯到 `wiki/sources/` 和 raw 文件。

## 公开仓库范围

本仓库只包含 `wiki/`、`AGENTS.md`、`README.md`、`.gitignore` 等可公开内容。

- `raw/` 原始论文、官方页面、GitHub README 因版权与体积原因**不随仓库上传**；本地使用时请将 `raw/` 放回仓库根目录。
- `.obsidian/` 为个人 Obsidian 状态与插件 bundle，默认整体忽略。
- Wiki 页面中的 `raw/...` 是本地来源路径，公开仓库中不会存在对应文件；原始 URL 仍保留在各来源页的「来源」小节。

## 使用方式

- 用 Obsidian 打开本目录作为 vault；浏览、搜索、图谱用 Obsidian，写入与交叉引用维护交给 Agent。
- 新增素材放进 `raw/` 对应子目录，然后让 Agent 按 `AGENTS.md` 执行 Ingest。
- 定期让 Agent 按 `AGENTS.md` 的 Lint 流程巡检。

## 版权与使用

- 论文、README、官方页面、System Card 的版权归原作者/机构所有；本目录仅用于内部研究阅读。
- CyberGym 代码为 Apache-2.0；AgentCyberRange 各仓库许可证见 `raw/github/*_LICENSE.txt`。
- 引用时请使用原始 URL 与正式版本（CyberGym 用 ICLR 2026；AgentCyberRange 用 arXiv v2）。
