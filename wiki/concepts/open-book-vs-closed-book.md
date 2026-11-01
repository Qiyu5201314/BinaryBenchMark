---
title: 开卷评测 vs 闭卷评测
type: concept
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/articles/Fudan_AgentCyberRange_Article.md
  - raw/articles/WeChat_CyberGym_AgentCyberRange.md
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
  - raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf
tags: [evaluation-paradigm, information-asymmetry, open-book, closed-book]
---

# 开卷评测 vs 闭卷评测

## 定义

这是复旦团队提出的一个**比喻式分类**，不是形式化 taxonomy：

- **开卷**：Agent 已被告知目标漏洞的大致范围、类型、位置，甚至拿到代码/补丁；核心任务是「分析并复现」。
- **闭卷**：只给目标入口（URL/IP），Agent 需要自己发现攻击面、判断弱点、完成利用，并在获得权限后继续推进。

来源：[[fudan-and-wechat-articles]]。

## 但「开闭卷」其实是多维的

> [推断] 只用「开卷/闭卷」二分容易掩盖关键差异。更可操作的做法是拆成四个维度：

| 维度 | 开卷端 | 闭卷端 | 例子 |
|---|---|---|---|
| **漏洞信息** | 给描述、栈、补丁 | 完全不给 | CyberGym L1–L3 vs L0 |
| **源码** | 给完整代码库 | 只给二进制/入口 | CyberGym vs 真实黑盒 |
| **攻击阶段** | 单点复现 | 端到端链式 | CyberGym vs AgentCyberRange |
| **目标知识** | 给拓扑/URL/凭证 | 只给入口 IP/URL | AgentCyberRange L2 vs L0 |
| **制品形态** | 给源码仓库 | 只给 stripped 二进制/固件 | CyberGym vs [[slyp]] |

因此：
- CyberGym L1 是「漏洞信息开卷 + 源码开卷 + 单点任务」。
- CyberGym L0 是「漏洞信息闭卷 + 源码开卷 + 单点任务」。
- AgentCyberRange L0 是「漏洞信息闭卷 + 源码闭卷 + 端到端任务」。
- 真正的「二进制闭卷」还应再加一条：**源码也不给，只给二进制**。

## 对照表

| 维度 | CyberGym（L1） | AgentCyberRange（L0） |
|---|---|---|
| 初始信息 | 漏洞描述 + 未修补源码 | 目标 URL / 入口 IP |
| 是否给漏洞位置 | 给（描述含大致位置） | 不给 |
| 攻击阶段 | 单点 PoC 复现 | Web 打点 + 内网后渗透 |
| 判定 | pre-patch 崩溃 / post-patch 不崩溃 | 运行时 marker / verifier + LLM judge |
| 主要能力 | 代码理解、PoC 构造 | 探索、利用、链式推进、隐蔽 |
| 现实对应 | 漏洞报告 → 复现 | 渗透测试 / 真实入侵 |

来源：[[cybergym]]、[[agentcyberrange]]。

## 2026 前沿修正：闭卷至少五个开关

2023–2026 的新工作说明，「开卷/闭卷」还必须再拆细，否则会把不同东西混为一谈：

| 类型 | 代表工作 | 给了什么 | 没给什么 |
|---|---|---|---|
| Web 闭卷 | [[cve-bench]]、[[autopenbench]]、[[agentcyberrange]] | 入口 URL/IP、任务目标 | 漏洞位置、利用方法 |
| 二进制检测 | [[vulbinllm]]、[[veritas]] | stripped binary | 源码、符号、漏洞描述 |
| 二进制利用 | [[slyp]] | COTS 二进制 + COM 元数据 + 调试接口 | 源码、符号、漏洞位置 |
| CTF | [[cybench]]、[[nyu-ctf-bench]]、[[enigma]] | 题面、起始文件、flag 目标 | 解法 |
| 源码修复 | [[aixcc]]、[[sec-bench]] | 源码 + fuzz harness | 漏洞位置、补丁 |

结论：

- **源码闭卷 ≠ 二进制闭卷**：前者仍可读变量名、类型、控制流；后者只有反编译/汇编。
- **去掉源码 ≠ 完全闭卷**：CTF 题面、flag 目标、COM 元数据、调试接口都是信息。
- **闭卷程度要按通道审计**：二进制、容器、网络、元数据、harness、工具集，每个通道都可能泄漏。

详见 [[binary-closed-book-landscape]]。

## 为什么这个区分重要

1. **避免误读榜单**：CyberGym 高分不等于能自主打穿真实目标；它测的是复现，不是端到端攻击。
2. **定位瓶颈**：AgentCyberRange 用 L0→L1→L2 区分「找不到入口」与「不会利用」；Web 赛道主要卡在入口发现，内网赛道主要卡在信息搜集与链式利用。
3. **设计新基准**：如果研究目标是「二进制闭卷」，需要同时控制「漏洞信息」「源码」「攻击阶段」「目标知识」「制品形态」五个开关，而不是只改一个。

来源：[[agentcyberrange-paper]]、[[fudan-and-wechat-articles]]。

## 常见误用

- 把 CyberGym 的 22%（论文）与排行榜 95%+ 直接比较——测量设置不同。
- 把 AgentCyberRange 论文的 110 漏洞与文章的近 500 漏洞混用——版本/口径不同。
- 说「闭卷一定更难」——不一定；闭卷减少了信息，但也可能让评分更依赖探索策略与预算，难度不完全可比。

详见 [[research/open-questions]]。

## 相关页面

- [[cybergym]]
- [[agentcyberrange]]
- [[cve-bench]]
- [[slyp]]
- [[veritas]]
- [[vulbinllm]]
- [[cybench]]
- [[binary-closed-book-landscape]]
- [[difficulty-levels]]
- [[vulnerability-reproduction]]
- [[post-exploitation]]
- [[comparisons/cybergym-vs-agentcyberrange]]

## 来源

- `raw/articles/Fudan_AgentCyberRange_Article.md`
- `raw/articles/WeChat_CyberGym_AgentCyberRange.md`
- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf`
- `raw/papers/Slyp_arXiv_2605.05000v1.pdf`
- `raw/papers/Veritas_arXiv_2605.15097v3.pdf`
- `raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf`
