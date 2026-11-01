---
title: CyberGym vs AgentCyberRange
type: comparison
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/articles/Fudan_AgentCyberRange_Article.md
  - raw/articles/WeChat_CyberGym_AgentCyberRange.md
tags: [comparison, benchmark, open-book, closed-book]
---

# CyberGym vs AgentCyberRange

## 一句话

CyberGym 测「给你漏洞信息和代码，你能不能复现」；AgentCyberRange 测「只给你入口，你能不能自己打进去并横向移动」。两者不是替代关系，而是覆盖不同能力层。

## 横向对比

| 维度 | CyberGym | AgentCyberRange |
|---|---|---|
| 机构 | UC Berkeley（Dawn Song 组） | 复旦大学等 |
| 评测范式 | 开卷：漏洞描述 + 源码 | 闭卷：只给入口 |
| 任务单元 | 单个历史漏洞的 PoC 复现 | Web 打点 + 内网后渗透 |
| 目标形态 | C/C++ 源码 + 二进制 | 真实 Web 应用 + 企业式内网 |
| 规模（论文） | 1,507 漏洞 / 188 项目 | 110 漏洞 / 15 apps + 8 ranges / 156 hosts |
| 难度等级 | L0–L3 | Web L0–L2；内网 L0–L2 |
| 验证 | sanitizer：pre 崩溃、post 不崩溃 | 运行时 marker / verifier + LLM judge |
| 主要指标 | Success Rate、final-submission、成本 | Pass@1 / Pass@3 Avg./Max、成本、时间 |
| 最强结果（论文） | 22.0%（GPT-5 thinking） | Web 16.1% / 内网 31.7%；提示后 33.0% / 46.3% |
| 现实对应 | 漏洞报告 → PoC | 渗透测试 / 真实入侵 |
| 主要瓶颈 | 代码理解、PoC 构造、长输入 | 入口发现、信息搜集、链式规划 |
| 执行框架 | 自研本地服务 + Docker + 防火墙 | [[cage]] |

来源：[[cybergym]]、[[agentcyberrange]]。

## 为什么互补

- CyberGym 能大规模、低成本、客观地测「漏洞复现」这一基础能力；但它不测「发现攻击面」和「横向移动」。
- AgentCyberRange 能测端到端攻击；但成本高、环境复杂、方差大、规模较小。
- 一个合理的能力画像应该是：
  - 底层：漏洞理解 / PoC 构造（CyberGym 类）
  - 中层：Web 黑盒打点（WebExploitBench 类）
  - 上层：内网后渗透 / 长链规划（PostExploitBench 类）
  - 更底层：二进制闭卷（尚缺，见 [[projects/binary-closed-book-eval]]）

## 数字不能直接比

- 22.0% vs 16.1% 不是「CyberGym 更简单」的证明：任务、目标、预算、验证都不同。
- 95%+ 的 CyberGym 榜单分数与 22% 论文结果也不是同一设置。
- 引用时必须写清：基准、版本、等级、trial 数、是否给提示、是否联网、成本预算。

来源：[[benchmark-validity]]、[[research/open-questions]]。

## 选择建议

| 你的目的 | 选 |
|---|---|
| 快速、大规模测漏洞复现能力 | CyberGym L1 |
| 测开放式漏洞发现 | CyberGym L0 |
| 测 Web 黑盒打点 | AgentCyberRange Web L0 |
| 测内网横向移动/长链 | AgentCyberRange Post L0 |
| 测「找不到入口 vs 不会利用」 | AgentCyberRange L0→L1→L2 |
| 测二进制闭卷 | 领域专用可行（[[slyp]]、[[veritas]]），但无统一公开基准 → 研究机会 |

## 相关页面

- [[cybergym]]
- [[agentcyberrange]]
- [[open-book-vs-closed-book]]
- [[benchmark-landscape]]
- [[binary-closed-book-landscape]]
- [[slyp]]
- [[veritas]]
- [[benchmark-validity]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/articles/Fudan_AgentCyberRange_Article.md`
- `raw/articles/WeChat_CyberGym_AgentCyberRange.md`
