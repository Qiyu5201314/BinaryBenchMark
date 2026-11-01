---
title: BinaryBenchMark Wiki 索引
type: index
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - README.md
tags: [index, navigation]
---

# BinaryBenchMark Wiki 索引

> 研究对象：AI Agent 网络安全能力评测，重点 CyberGym、AgentCyberRange、CAGE，以及「二进制闭卷评测」。
> 维护协议：[AGENTS.md](../AGENTS.md)。raw 素材见 `raw/papers/`、`raw/github/`、`raw/official/`、`raw/articles/`。

## 快速入口

| 我想… | 从这里开始 |
|---|---|
| 日常入口 / 待办看板 | [[dashboard]] |
| Obsidian 怎么用 | [[obsidian-guide]] |
| 快速了解两个基准 | [[cybergym]] → [[agentcyberrange]] → [[cybergym-vs-agentcyberrange]] |
| 理解「开卷 vs 闭卷」 | [[open-book-vs-closed-book]] |
| 做研究/找选题 | [[research-map]] → [[open-questions]] → [[binary-closed-book-eval]] |
| 看 2023–2026 前沿版图 | [[binary-closed-book-landscape]] |
| 搭评测环境 | [[cage]] → [[evaluation-isolation]] → [[reproducibility]] |
| 看领域全貌 | [[overview]] |
| 查数字口径 | [[open-questions]] 的「口径冲突」 |
| 看活动记录 | [[log]] |

---

## 实体（entities）

| 页面 | 一句话 |
|---|---|
| [[cybergym]] | UC Berkeley 的大规模漏洞复现基准：1,507 漏洞 / 188 项目，开卷式 |
| [[agentcyberrange]] | 复旦的闭卷式端到端靶场评测：Web 打点 + 内网后渗透 |
| [[cage]] | AgentCyberRange 的通用评测执行框架：容器、代理、快照、判分 |
| [[webexploitbench]] | Web 打点赛道：15 应用 / 110 漏洞 / 17 类 |
| [[postexploitbench]] | 内网后渗透赛道：8 靶场 / 156 主机 |
| [[arvo-oss-fuzz]] | CyberGym 的漏洞语料来源与可复现数据集 |
| [[fudan-sslab]] | AgentCyberRange 提出者，主张「闭卷实战」 |
| [[anthropic]] | 在 System Card 中使用 CyberGym，并公开评测环境事故 |
| [[cve-bench]] | 40 个真实 Web CVE 的闭卷利用基准，zero-day 10% / one-day 12.5–13% |
| [[aixcc]] | DARPA AI Cyber Challenge：7 个开源 CRS，决赛 54 发现 / 43 修补 |
| [[cybench]] | 40 个专业 CTF，FST 难度锚 + 子任务，17.5% unguided |
| [[nyu-ctf-bench]] | 200 个 CTF，开发集/测试集分离，pwn/rev 子集 |
| [[autopenbench]] | 33 个渗透任务，里程碑部分分，全自主 21% / 半自主 64% |
| [[sec-bench]] | 自动造题：200 个真实 CVE，$0.87/instance，PoC 18% / patch 34% |
| [[slyp]] | COTS 二进制 Agent：28 个 0day、16 个 CVE、debugger-verified PoC |
| [[enigma]] | CTF Agent + 交互式工具（gdb）；NYU 13.5%；soliloquizing |
| [[veritas]] | stripped binary 推理：静态 witness + 运行时 oracle；90% recall |
| [[vulbinllm]] | stripped binary CWE 检测：Juliet F1 74–99% |

## 概念（concepts）

| 页面 | 一句话 |
|---|---|
| [[open-book-vs-closed-book]] | 开卷/闭卷不是二分，而是「漏洞信息/源码/阶段/目标知识」四维 |
| [[vulnerability-reproduction]] | 给描述+代码，生成 PoC，pre 崩溃 / post 不崩溃 |
| [[difficulty-levels]] | L0–L3 与 L0–L2 的提示梯度，及 Pass@k 指标 |
| [[post-exploitation]] | 拿到立足点之后的凭证、隧道、横向、提权、免杀 |
| [[cyber-range]] | 隔离、可复现的企业式网络靶场 |
| [[benchmark-validity]] | 构念、污染、reward hacking、方差、验证器、覆盖 |
| [[evaluation-isolation]] | 评测环境必须真的断网；Anthropic 三起事故的教训 |
| [[cost-aware-evaluation]] | 成功率之外的成本、时间与 cost-success frontier |
| [[agent-harness]] | 模型 ≠ 系统；harness 是评测中最大的混杂变量之一 |
| [[reproducibility]] | 数据/镜像/端点/验证器的版本化与固定 |

## 来源摘要（sources）

| 页面 | 对应 raw 素材 |
|---|---|
| [[cybergym-paper]] | CyberGym 论文（arXiv v3 / ICLR 2026） |
| [[cybergym-repo-and-site]] | CyberGym README / FAQ / SUBMISSION / 官网 / 博客 |
| [[agentcyberrange-paper]] | AgentCyberRange 论文（arXiv v2） |
| [[cage-docs]] | CAGE README 与 docs |
| [[fudan-and-wechat-articles]] | 复旦实验室文章 + 白泽公众号 |
| [[anthropic-incident-report]] | Anthropic 评测事故报告 + System Card 文本 |
| [[berkeley-frontier-ai-report]] | Berkeley RDI 前沿 AI 网络安全影响报告 |
| [[uk-aisi-inspect-cybergym]] | UK AISI Inspect Evals 的 CyberGym 复现 |
| [[related-work]] | 成本感知评测 + 自然代码变换 |
| [[binary-closed-book-guide]] | 作者自撰《二进制闭卷测试指南》：信息契约、Oracle、任务谱系、Level A/B |
| [[cve-bench-paper]] | 真实 Web CVE 闭卷利用；40 CVE；10%/12.5–13% |
| [[cybench-paper]] | 40 个专业 CTF；FST + 子任务；scaffold 比较 |
| [[nyu-ctf-bench-paper]] | 200 个 CTF；开发集/测试集；失败原因分类 |
| [[enigma-paper]] | 交互式工具（gdb）；soliloquizing；NYU 13.5% |
| [[autopenbench-paper]] | 33 个渗透任务；里程碑；21% vs 64% |
| [[vulbinllm-paper]] | stripped binary 的 CWE 检测；Juliet |
| [[veritas-paper]] | stripped binary；静态 witness + 运行时；90% recall |
| [[slyp-paper]] | COTS 二进制；28 个 0day；debugger PoC |
| [[aixcc-paper]] | AIxCC + FuzzingBrain；PoV + 补丁；官方数字 |
| [[sec-bench-paper]] | 自动造题；$0.87/instance；PoC 18% / patch 34% |

## 对比（comparisons）

| 页面 | 一句话 |
|---|---|
| [[cybergym-vs-agentcyberrange]] | 开卷复现 vs 闭卷端到端：互补而非替代 |
| [[benchmark-landscape]] | CyberGym、AgentCyberRange、CTF、CVE、渗透基准的版图与空白 |
| [[binary-closed-book-landscape]] | 2023–2026 前沿版图：四个梯队、六个可复用机制、仍未填的空白 |

## 研究（research）

| 页面 | 一句话 |
|---|---|
| [[research-map]] | 任务/环境/Agent/验证/度量 五层框架 |
| [[open-questions]] | 13 个开放问题 + 6 组口径冲突 |
| [[methodology-notes]] | 基准设计步骤、指标模板、偏误清单、实验矩阵 |

## 项目（projects）

| 页面 | 一句话 |
|---|---|
| [[binary-closed-book-eval]] | 「只给二进制、不给源码/描述」的评测研究计划（draft） |

---

## 统计

- 实体页：18
- 概念页：10
- 来源页：20
- 对比页：3
- 研究页：3
- 项目页：1
- 内容页合计：55
- 辅助页：5（index、[[overview]]、[[dashboard]]、[[log]]、[[obsidian-guide]]）
- 另有：`_templates/` 下 4 个模板

## 维护提示

- 新增素材 → 先在 `sources/` 建页，再更新本索引与相关实体/概念页。
- 发现数字冲突 → 标 `needs-review`，写入 [[open-questions]]。
- 定期巡检 → 见 [AGENTS.md](../AGENTS.md) 第 6.3 节。
