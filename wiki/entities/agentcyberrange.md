---
title: AgentCyberRange
type: entity
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/github/AgentCyberRange_ORG_README.md
  - raw/github/WebExploitBench_README.md
  - raw/github/PostExploitBench_README.md
  - raw/articles/Fudan_AgentCyberRange_Article.md
  - raw/articles/WeChat_CyberGym_AgentCyberRange.md
tags: [benchmark, cyber-range, closed-book, post-exploitation, web-exploitation, fudan]
---

# AgentCyberRange

## 一句话

复旦大学等提出的开放多靶场评测基础设施：只给 Agent 一个目标入口（Web URL 或内网 IP），让它在真实 Web 应用与企业式内网中**自主发现攻击面、完成利用并横向移动**，用运行时证据自动判分。

**评测范式定位**：闭卷（不给漏洞描述/源码）、端到端（Web 打点 + 内网后渗透）。与 [[cybergym]] 形成互补。详见 [[open-book-vs-closed-book]]。

## 两条赛道

| 赛道 | 数据集 | 规模（论文版） | 评什么 |
|---|---|---|---|
| Web 打点 | [[webexploitbench]] | 110 漏洞 / 15 真实 Web 应用 / 17 类漏洞 | 黑盒探索隐藏端点与参数，构造 exploit 并验证影响 |
| 内网后渗透 | [[postexploitbench]] | 8 套企业式内网 / 156 台主机 | 入口利用、凭证复用、隧道、横向移动、提权、持久化、免杀 |

来源：[[agentcyberrange-paper]]、`raw/github/AgentCyberRange_ORG_README.md`。

## 难度分级（L0–L2）

与 CyberGym 不同，AgentCyberRange 的两条赛道分别定义提示等级：

| 等级 | Web 赛道 | 内网赛道 |
|---|---|---|
| **L0** | 只给目标应用 URL | 只给入口 IP |
| **L1** | 额外告诉哪些 URL 有漏洞 | 额外给内网拓扑（可达子网/主机） |
| **L2** | 额外告诉每个漏洞的类型 | 额外给具体 CVE / 弱点 / 凭证位置等提示 |

设计目的：区分 Agent 卡在「找不到入口」还是「不会利用」。来源：[[agentcyberrange-paper]]、[[difficulty-levels]]。

## 执行框架

[[cage]]（Cybersecurity Agent Gym & Evaluation）是配套的评测执行框架：Docker 隔离、并行 trial、容器内 LLM 代理拦截、状态快照、运行时验证、断点续跑、inspector 回放。

## 主要结果（论文版，Level-0）

| 系统 | Web Pass@1 | Web Pass@3 (Avg.) | Web Pass@3 (Max) | 内网 Pass@1 | 内网 Pass@3 (Avg.) |
|---|---|---|---|---|---|
| **GPT-5.5 + Codex** | 19.09% | 16.06% | **28.18%** | **31.71%** | **31.71%** |
| Claude-Opus-4.7 + Claude Code | 16.36% | 14.55% | 26.36% | — | — |
| Qwen-3.7-Max + Qwen Code | — | 12.42% | — | — | — |
| 其余系统 | — | 3.03%–8.18% | — | — | — |

- 论文摘要口径：GPT-5.5 + Codex 在 Web 打点 **16.1%**、内网后渗透 **31.7%**；给更具体提示后升到 **33.0% / 46.3%**。
- 内网 Level-2 最高 **46.34%**。
- 6 个前沿系统、统一 prompt 与预算；每个任务 Web 150 步、内网 500 步、2 小时超时。

来源：[[agentcyberrange-paper]]。

> 注意：论文表格中 GPT-5.5 的 Pass@1（19.09%）高于 Pass@3 Avg.（16.06%），与通常直觉相反。引用时以原文表格为准，不要自行改算；这可能是多次运行的聚合方式或样本选择导致。`status: needs-review` 的细节见 [[research/open-questions]]。

## 关键发现

- **入口发现是 Web 赛道主瓶颈**：漏洞深度从 2 增加到 6，检测率从 35% 降到 11%；给出 URL 级提示可提升最多 21.81 个百分点。
- **内网瓶颈在信息搜集与链式利用**：Agent 会在无利用价值的主机上浪费步数；拿下 Confluence 后不会去 Wiki 里找 GitLab 凭证，导致后续链路中断。
- **运行间方差大**：同一 Agent、同一设置，不同 attempt 结果差异明显；很多漏洞只在一次运行中出现。
- **防御适应性**：Agent 会在 webshell 被杀软删除后变异 payload 恢复立足点；但也会反复触发蜜罐、留下告警日志。
- **benchmark 外发现**：GPT-5.5 + Codex 在 ComfyUI 发现任意文件写 0day；还发现未标注的 one-day。
- **专用渗透 Agent 未显著占优**：PentestGPT-V2 在失败案例上同样失败，说明瓶颈不只在编排。

来源：[[agentcyberrange-paper]]、[[fudan-and-wechat-articles]]。

## 规模口径差异（必须注明）

同一项目在不同材料中数字差异很大，**不是笔误，而是版本/统计口径不同**：

| 来源 | 时间 | Web 应用 | 漏洞 | 内网 | 主机 |
|---|---|---|---|---|---|
| 论文 arXiv v2 | 2026-06 | 15 | 110 | 8 | 156 |
| 复旦实验室文章 | 2026-07 | 25 | 近 500 | 30 | 近 400 |
| 白泽公众号 | 2026（抓取 10-29） | 30 | 超 200（0day/1day） | 50+ | 超 300 |
| 根 README 索引 | 2026-10-29 | 15 | 110 | 8 | 156（另有 266 任务口径） |

> [推断] 论文版是「可复现基线」，文章是「平台当前/累计口径」，可能包含未纳入论文的靶场。引用时写清来源与日期，禁止混用。详见 [[research/open-questions]]。

来源：[[agentcyberrange-paper]]、[[fudan-and-wechat-articles]]。

## 局限

- 只覆盖 Web 打点 + 内网后渗透，不覆盖钓鱼、Windows 域、云 IAM、供应链、社工。
- 结果依赖 Agent harness、prompt、工具集、预算；API 不稳定与安全拒答会降低分数（Claude-Opus-4.7 出现 12 次拒答）。
- 验证器依赖可观测运行时证据，可能低估「部分进展」或替代攻击路径。
- benchmark 外漏洞需人工验证，不能直接计入主分。
- 靶场真实性与隔离性存在张力；评测环境本身可能被逃逸（见 [[evaluation-isolation]]）。

来源：[[agentcyberrange-paper]]。

## 相关页面

- [[cybergym]]：开卷复现的对照
- [[cage]]：执行框架
- [[webexploitbench]]、[[postexploitbench]]
- [[post-exploitation]]、[[cyber-range]]
- [[open-book-vs-closed-book]]
- [[comparisons/cybergym-vs-agentcyberrange]]

## 来源

- 论文：`raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`；https://arxiv.org/abs/2606.14295
- 组织 README：`raw/github/AgentCyberRange_ORG_README.md`；https://github.com/AgentCyberRange
- 复旦文章：`raw/articles/Fudan_AgentCyberRange_Article.md`；https://security.fudan.edu.cn/news/content/2026-08-20-004/
- 白泽公众号：`raw/articles/WeChat_CyberGym_AgentCyberRange.md`；https://mp.weixin.qq.com/s/giumeYfpF0tmQ6CKK5KiCg
