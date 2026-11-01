---
title: 活动日志
type: log
status: stable
created: 2026-10-29
updated: 2026-11-01
sources: []
tags: [log]
---

# 活动日志

> 追加式记录。格式：`## [YYYY-MM-DD] 操作 | 对象`。

## [2026-10-29] init | 构建 BinaryBenchMark Wiki

- 读取 raw 层：`raw/papers/`、`raw/github/`、`raw/official/`、`raw/articles/`、`raw/README.md`。
- 建立 schema：`AGENTS.md`。
- 建立目录：`wiki/{entities,concepts,sources,comparisons,research,projects}`。
- 初始页面：8 实体 + 10 概念 + 9 来源 + 2 对比 + 3 研究 + 1 项目 + overview/index/log。

### 关键决策

- 不移动 raw 素材，直接把现有四个目录视为 raw 层，避免破坏原 `README.md` 的路径。
- 文件名用英文 kebab-case，正文中文，保证 Obsidian 兼容。
- 所有数字保留「来源 + 版本 + 日期 + 范围」，冲突集中到 [[open-questions]]。

---

## [2026-10-29] ingest | CyberGym 论文

- 新建：[[cybergym-paper]]
- 更新：[[cybergym]]、[[vulnerability-reproduction]]、[[difficulty-levels]]、[[arvo-oss-fuzz]]、[[benchmark-validity]]
- 关键数字：1,507 漏洞 / 188 项目；L1 最好 22.0%；L0 3.5%；34 0day / 18 不完整补丁。
- 待验证：论文 Table 2 污染分析细节、Appendix C 成本设置。

## [2026-10-29] ingest | CyberGym 仓库、官网、FAQ、提交规范

- 新建：[[cybergym-repo-and-site]]
- 更新：[[cybergym]]、[[evaluation-isolation]]、[[benchmark-validity]]、[[cost-aware-evaluation]]、[[reproducibility]]
- 关键事实：数据 240GB / binary-only 130GB / full server 10TB；主榜要求 final-submission；2026-08 起要求成本上报；本地部署 + 防火墙。
- 冲突：论文 22% vs 官网/提交指南「接近饱和」vs 公众号 95%+；已记入 [[open-questions]]。

## [2026-10-29] ingest | AgentCyberRange 论文

- 新建：[[agentcyberrange-paper]]
- 更新：[[agentcyberrange]]、[[webexploitbench]]、[[postexploitbench]]、[[open-book-vs-closed-book]]、[[post-exploitation]]
- 关键数字：110 漏洞 / 15 apps / 8 ranges / 156 hosts；GPT-5.5+Codex Web 16.1%、内网 31.7%；提示后 33.0% / 46.3%。
- 待验证：Table 4 中 Pass@1 > Pass@3 Avg. 的定义；Cost (M) 单位。

## [2026-10-29] ingest | CAGE 文档

- 新建：[[cage-docs]]
- 更新：[[cage]]、[[agent-harness]]、[[evaluation-isolation]]、[[reproducibility]]
- 关键事实：run equation；manage vs serve；19 步 trial 生命周期；proxy.jsonl 是事实来源；cage corpus 导出训练数据。

## [2026-10-29] ingest | 复旦文章与白泽公众号

- 新建：[[fudan-and-wechat-articles]]
- 更新：[[fudan-sslab]]、[[agentcyberrange]]、[[open-book-vs-closed-book]]
- 关键叙事：CyberGym 是开卷考，真实攻击是闭卷实战；Confluence → Wiki → GitLab 凭证失败案例。
- 冲突：文章规模（25/近 500/30/近 400；30/200+/50+/300+）vs 论文（15/110/8/156）；已记入 [[open-questions]]。

## [2026-10-29] ingest | Anthropic 评测事故报告

- 新建：[[anthropic-incident-report]]
- 更新：[[anthropic]]、[[evaluation-isolation]]、[[cyber-range]]、[[benchmark-validity]]
- 关键事实：141,006 次运行 → 3 起事故 / 6 次运行 / 3 家组织；PyPI 包被 15 个真实系统下载；根因是断网配置错误。
- 影响：把「评测环境隔离」提升为独立概念页。

## [2026-10-29] ingest | Berkeley RDI 前沿 AI 网络安全报告

- 新建：[[berkeley-frontier-ai-report]]
- 更新：[[research-map]]、[[benchmark-validity]]
- 关键观点：边际风险评估 + 杀伤链；攻击侧影响更强；现有基准不覆盖完整攻击链。

## [2026-10-29] ingest | UK AISI Inspect Evals CyberGym

- 新建：[[uk-aisi-inspect-cybergym]]
- 更新：[[reproducibility]]、[[benchmark-validity]]、[[cybergym]]
- 关键事实：gpt-4.1 复现 11.61% vs 论文 9.4%；[2-A] 修复「测错 PoC」导致旧结果作废；[3-B] 固定镜像 digest。

## [2026-10-29] ingest | 两篇相关研究

- 新建：[[related-work]]
- 更新：[[cost-aware-evaluation]]、[[benchmark-validity]]
- 关键观点：成本感知评测；攻防 scaling 不同；自然代码变换可显著混淆 Agent，构成基准有效性威胁。

---

## [2026-10-29] synthesize | 建立研究地图与项目页

- 新建：[[research-map]]、[[open-questions]]、[[methodology-notes]]、[[binary-closed-book-eval]]
- 综合判断：二进制闭卷（只给二进制、不给源码/描述）目前没有成熟基准；这是本库的选题方向。
- 待验证：H1–H5 假设需要实验；现有素材不足以证明。

---

## [2026-10-29] lint | 初次巡检

- 已修正：`webexploitbench-source`、`postexploitbench-source` 断链；`benchmark-contamination` 死链；自链接。
- 已知路径式链接：`research/`、`comparisons/`、`projects/` 前缀的 wikilink 在 Obsidian 中可用；若迁移到其他工具，可统一改为文件名。
- 待补：CyberGym Appendix、AgentCyberRange Appendix、verifier 实现、更多第三方复现。
- 见 [[open-questions]] 的「待补素材」。

---

## [2026-10-29] config | 添加 Obsidian 工作台与模板

- 新建：[[dashboard]]（Dataview 待处理/最近更新/统计/孤立页/研究页/来源清单）。
- 新建：`wiki/_templates/source.md`、`entity.md`、`concept.md`、`daily.md`。
- 更新：[[index]] 快速入口与统计；`AGENTS.md` 目录结构。
- 说明：模板使用 Obsidian 核心 Templates 语法（`{{title}}`、`{{date:YYYY-MM-DD}}`），无需 Templater 即可用；Dataview 查询需要安装 Dataview 插件。

---

## [2026-10-29] restructure | 统一 raw/ 目录

- 移动：原 `papers`、`github`、`official`、`articles` 四个目录 → `raw/` 下。
- 移动：根目录 `README.md` → `raw/README.md`；新建根目录 `README.md` 作为项目总览。
- 移动：`二进制闭卷测试指南.md` → `raw/notes/二进制闭卷测试指南.md`。
- 新建：[[binary-closed-book-guide]]（总纲的结构化摘要）。
- 批量更新：36 个 wiki 文件、278 处 raw 路径引用（旧路径前缀 → `raw/` 前缀）。
- 更新：`AGENTS.md` 三层架构与目录树；[[index]] 来源清单与统计；[[binary-closed-book-eval]]、[[overview]] 增加总纲入口。
- 校验：278 条 raw 路径全部存在；wikilink 0 断链；无重复 `raw/` 前缀。

---

## [2026-10-29] config | Obsidian 配置完成

- Vault：`BinaryBenchMark`（含 `raw/`、`wiki/`、`AGENTS.md`、`README.md`）。
- 文件与链接：新笔记 `wiki/inbox`；附件 `wiki/_attachments`；内部链接用 Wiki 链接语法；自动更新链接；显示行号；显示所有文件类型。
- 模板：核心 Templates 与 Templater 均指向 `wiki/_templates`。
- 日记：目录 `wiki/journal`；模板 `wiki/_templates/daily.md`。
- 社区插件（13 个，均已启用）：Dataview、QuickAdd、Omnisearch、Text Extractor、PDF++、Linter、Templater、Calendar、Periodic Notes、Excalidraw、Kanban、Git、Style Settings。
- 主题：Minimal（已启用）。
- Git：已执行 `git init`，新增 `.gitignore`（忽略 workspace/cache/trash 等）。
- 验证：dashboard 在阅读视图下正常渲染 Dataview 表格；模板页已从查询中排除。
- 未配置：QuickAdd 的具体 choices、Linter 规则、Periodic Notes 的周/月模板、图谱分组与自定义快捷键；可按需再加。

---

## [2026-10-29] config | QuickAdd 三个捕获命令

- **Capture Source**（Template）：模板 `wiki/_templates/source.md`；文件名 `{{VALUE:来源标题}}`；目标文件夹 `wiki/sources`。
- **Capture Idea**（Capture）：目标 `wiki/inbox/Inbox.md`；格式 `- {{DATE:YYYY-MM-DD HH:mm}} {{VALUE:想法}}`；文件不存在时自动创建。
- **Daily**（Macro）：执行 `日记: 打开/创建今天的日记`。
- 三个 choice 均已加入命令面板（`command: true`），可在 `Ctrl+P` 中直接搜索运行。
- 配置落盘于 `.obsidian/plugins/quickadd/data.json`。

---

## [2026-10-29] docs | Obsidian 使用指南

- 新建 [[obsidian-guide]]：目录结构、每日/每周流程、论文阅读流程、插件速查、快捷键、维护规则、常见问题、下一步清单。
- 补充 6.6「新增素材/笔记怎么放」：归档判断表、命名建议、三种入口、添加后检查清单。
- 补充 6.7「Wiki 更新后的验收与阅读」：变更检查、按更新类型阅读、四件核对、5 分钟版与 Agent 变更报告提示词。
- 补充 6.8「看完 Dashboard 之后怎么操作」：待处理、最近更新、类型统计、孤立页、研究/项目、来源清单、5/15 分钟版与 Agent 指令。
- 已在 [[index]] 的「快速入口」和根目录 `README.md` 中加入入口。

---

## [2026-11-01] ingest | 2023–2026 二进制闭卷/黑盒 Agent 前沿（10 篇论文 + 5 个 README）

- 下载论文到 `raw/papers/`：CVE-Bench、Cybench、NYU CTF Bench、EnIGMA、AutoPenBench、VulBinLLM、Veritas、Slyp、AIxCC FuzzingBrain、SEC-bench。
- 下载 README/LICENSE 到 `raw/github/`：CVE-Bench、Cybench、NYU_CTF_Bench、AutoPenBench、AIxCC_FuzzingBrain。
- 下载官方页到 `raw/official/`：AIxCC ARPA-H 结果、AIxCC 归档、Team Atlanta 博客（HTML + TXT）。
- 新建来源页：[[cve-bench-paper]]、[[cybench-paper]]、[[nyu-ctf-bench-paper]]、[[enigma-paper]]、[[autopenbench-paper]]、[[vulbinllm-paper]]、[[veritas-paper]]、[[slyp-paper]]、[[aixcc-paper]]、[[sec-bench-paper]]。
- 新建实体页：[[cve-bench]]、[[cybench]]、[[nyu-ctf-bench]]、[[autopenbench]]、[[sec-bench]]、[[slyp]]、[[aixcc]]、[[enigma]]、[[veritas]]、[[vulbinllm]]。
- 新建对比页：[[binary-closed-book-landscape]]（四个梯队、六个可复用机制、仍未填的空白）。
- 更新：[[benchmark-landscape]]、[[benchmark-validity]]、[[agent-harness]]、[[difficulty-levels]]、[[open-book-vs-closed-book]]、[[research-map]]、[[open-questions]]、[[binary-closed-book-eval]]、[[index]]、`raw/README.md`。

### 关键判断

- 二进制闭卷从「完全空白」推进到「领域专用可用」：[[slyp]] 在 COTS Windows 二进制上发现 28 个 0day、16 个 CVE；[[veritas]] 在 stripped binary 上 90% recall。
- 但仍没有「stripped binary + 仅入口 + 发现并利用 + 可归因 Oracle + 公开可复现」的统一基准。
- 可复用机制：FST 难度锚（Cybench）、里程碑部分分（AutoPenBench）、多出口判据（CVE-Bench）、PoV 作为 oracle（AIxCC）、自动造题（SEC-bench）、工具接口作为变量（EnIGMA/Slyp）。

### 口径与更正

- AIxCC 决赛合成漏洞：早期 70 → ARPA-H 官方更正为 63；54 发现、43 修补。
- CVE-Bench one-day：正文 12.5%、摘要/图注 13%；zero-day 10%。
- Slyp 生产数据：论文 28 漏洞 / 16 CVE / $140k；作者主页后续写 40 漏洞 / 31 CVE / $203k，口径不同，暂用论文数字。
- 已记入 [[open-questions]] 的 A5、A6。

### 校验

- wikilink：0 断链（除 `obsidian-guide` 中的示例占位符）。
- 重复页名：0（来源页统一加 `-paper` 后缀，避免与实体页同名）。
- frontmatter `sources`：全部存在。
- 新 raw 文件：10 篇论文 PDF + 5 README + 5 LICENSE + 3 官方页 HTML/TXT。

---

## [2026-11-01] lint | 全仓体检

### 范围

- Wiki：60 个 Markdown 页面（18 实体 / 10 概念 / 20 来源 / 3 对比 / 3 研究 / 1 项目 / 5 辅助页）。
- Raw：84 个文件，58.1 MB；`.obsidian/` 16.2 MB。
- 检查项：frontmatter、wikilink、重复页名、孤立页、raw 路径、索引统计、Markdown 结构、PDF 有效性、大文件重复、关键数字一致性。

### 结果

- Frontmatter 必填键：**0 缺失**（修复前 `log.md`、`obsidian-guide.md` 缺 `sources`）。
- 重复页名：**0**。
- 孤立页：**0**。
- Wikilink：**921 条，0 真断链**；唯一命中是 `obsidian-guide.md` 里的示例 `![[...]]`，属正则误报。
- Frontmatter `sources` 路径：**0 缺失**。
- Raw 路径引用：**0 缺失**（除 `obsidian-guide` 的示例占位符）。
- `raw/README.md` 覆盖率：**84/84**。
- Markdown 结构：**0** 个代码围栏不配对、表格管道数不一致、标题跳级。
- PDF：13 个全部为有效 `%PDF`；无 0 字节文件、无重复大文件。
- 索引统计与实际一致：18 / 10 / 20 / 3 / 3 / 1 / 55。

### 已修复

- [[cybergym-vs-agentcyberrange]] 的过时结论「二进制闭卷目前没有成熟基准」→ 改为「领域专用可行（[[slyp]]、[[veritas]]），但无统一公开基准」。
- [[aixcc]] / [[aixcc-paper]] 的「7 个 CRS 全部开源」→ 精确为「均承诺开源；ARPA-H 公告时 5 个已发布」。
- [[index]] 的 CVE-Bench 口径从「one-day 13%」精确为「12.5–13%」。
- [[open-questions]] A6 的「早期报道/第三方」→「ARPA-H 早期版本 / 第三方」，与官方更正说明一致。
- [[sec-bench-paper]] 标注「NeurIPS 2025（本地为 arXiv 预印本）」。
- `raw/README.md` 补齐 AIxCC `.txt` 与 PostExploitBench/WebExploitBench LICENSE 的索引。
- `log.md`、`obsidian-guide.md` 补 `sources: []`；新增页面补充 `[推断]` 标注。

### 待决策 / 风险

1. **Git 没有初始提交**：`git status` 显示全部文件 untracked，分支 `master` 无 commit。建议先决定是否提交、是否用 Git LFS 管理 58 MB raw。
2. ~~**`.obsidian/` 16.2 MB 未忽略**~~（已修复）：`.gitignore` 已改为整体忽略 `.obsidian/`，见同日 config 条目。
3. ~~**Schema 漂移**~~（已修复）：[[obsidian-guide]] 使用 `type: guide`，但 `AGENTS.md` 第 3 节未定义 `guide`，也未定义 `index/log/dashboard/overview`。已在同日的 schema 条目中新增「辅助页」类型。
4. **3 个 draft + 1 个 needs-review**：[[binary-closed-book-landscape]]、[[binary-closed-book-eval]]、[[methodology-notes]]、[[open-questions]]。属刻意保留，发布前需复核。
5. **待补素材**：BountyBench、Google Big Sleep / Naptime、AIxCC 七个 CRS 代码、Slyp/Veritas/VulBinLLM 代码（若公开），已记入 [[open-questions]]。
6. **批量 ingest 偏离建议流程**：本次一次性引入 10 个来源，偏离 `AGENTS.md` 6.1「一次只处理一个来源，人工复核后再继续」；已逐篇建页，但建议先抽查 2–3 篇再继续扩展。

---

## [2026-11-01] schema | 增加辅助页类型（AGENTS.md v1.1）

- 版本：`AGENTS.md` v1.0 → **v1.1**。
- 新增 3.1「内容页」/ 3.2「辅助页」两节：
  - 内容页：entity、concept、source、comparison、research、project。
  - 辅助页：index、overview、dashboard、log、guide。
- 辅助页规则：
  - 不参与 `index.md` 的「内容页合计」，单独统计；
  - 不作为知识来源引用；
  - `log.md` 只追加、不改历史；
  - `dashboard.md` 只查询、不写结论；
  - `guide` 页的 `[[...]]` 示例在 Lint 中跳过；
  - 允许 `sources: []`。
- Frontmatter：`type` 必须是 schema 已定义类型；未定义类型先改 `AGENTS.md`。
- 命名：来源页加 `-paper` / `-docs` / `-article` 后缀，避免与实体页同名。
- Lint 表新增：重复页名、未定义 type、index 统计不一致、frontmatter sources 路径不存在、辅助页被当来源引用。
- [[index]] 统计改为「内容页合计 55 + 辅助页 5」。
- 校验：60 页全部有完整 frontmatter；辅助页 `type: guide` 已合法；wikilink 仍为 0 真断链。

---

## [2026-11-01] config | 整体忽略 `.obsidian/`

- `.gitignore`：把原来 4 条细粒度规则（workspace.json / workspace-mobile.json / cache / data.json.bak）替换为 `.obsidian/`，整体忽略插件 bundle（16.2 MB）与个人状态。
- `AGENTS.md` 第 10 节：同步为「`.obsidian/` 默认整体忽略；如需共享配置，用 `!.obsidian/app.json` 等例外规则」。
- 验证：`git check-ignore -v .obsidian/app.json` → `.gitignore:2:.obsidian/`；`git status --short` 已不再出现 `.obsidian/`。
- 说明：当前仓库尚无 commit，`.obsidian/` 从未入库，因此不需要 `git rm --cached`；若未来已跟踪，执行 `git rm -r --cached .obsidian` 后再提交。
