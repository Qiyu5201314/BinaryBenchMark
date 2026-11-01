---
title: 来源摘要：Slyp
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
tags: [source, binary-only, cots, windows, agent, real-world]
---

# 来源摘要：Slyp

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | Agentic Vulnerability Reasoning on Windows COM Binaries |
| 作者 / 机构 | Hwiwon Lee, Jongseong Kim, Lingming Zhang（UIUC） |
| 版本 / 日期 | arXiv v1（2026-05-06） |
| 本地文件 | `raw/papers/Slyp_arXiv_2605.05000v1.pdf` |
| 原始 URL | https://arxiv.org/abs/2605.05000 |

## 核心主张

1. Windows COM 服务是本地提权的高价值攻击面；竞态漏洞难以用 fuzzer 或静态分析可靠发现。
2. 端到端 agentic pipeline 可以同时做「发现」与「debugger-verified PoC 生成」。
3. 关键不是通用 coding agent，而是三组专用工具接口：二进制探索、COM 检查、动态调试。
4. 在真实生产服务上可发现并验证 0day，说明二进制闭卷 agent 已具备现实影响力。

## 关键数字

| 指标 | 数值 | 口径 / 备注 |
|---|---|---|
| 基准 | 20 个 COM 对象 / 40 个漏洞案例 | 竞态条件 |
| F1 | 0.973（GPT-5.4） | 优于生产 coding agent 最多 +0.208 |
| 静态分析对照 | COMRace++ 0.299 F1 | Slyp 约为其 3.3× |
| PoC 生成 | 67.5%（27/40） | 最强配置；四个配置合计 34/40 |
| 生产部署 | 28 个未知漏洞 / 9 个 COM 服务 | MSRC 全部确认 |
| CVE / 赏金 | 16 个 CVE / $140,000 | 论文口径 |
| 模型 | 6 个 backbone（GPT / Claude） | 模型无关的 pipeline |

## 方法

- **ReAct loop**：标准推理-行动循环，加长程二进制分析的工程特性。
- **三个 MCP 工具**：
  1. 二进制探索：反编译、虚表/虚调用解析；
  2. COM 检查：接口元数据、激活模板；
  3. 动态调试：编译、执行、崩溃捕获。
- **上下文管理**：pre-compaction checkpointing，跨上下文压缩保留跨函数发现。
- **任务验证**：防止 agent 过早终止；PoC 必须被 debugger 验证。
- **对照**：Codex、Claude Code 在相同 prompt 下比较；加全部二进制工具后仍有差距。

## 方法学启示

- **这是目前最接近「二进制闭卷 + 真实利用」的公开工作**：不给源码，只给 COTS 二进制与调试接口；产出是 PoC，不是仅检测。
- **工具接口决定上限**：生产 agent 在默认工具下几乎无法验证 PoC；加上 COM/调试工具后大幅提升。
- **真实 0day 是最强外部效度证据**：28 个 MSRC 确认漏洞、16 个 CVE，说明不是合成题。
- **但它是「专用流水线」而非通用基准**：任务是竞态类 COM 漏洞，不能直接当作通用二进制闭卷分数。

## 局限

- 只覆盖 Windows COM 竞态条件；其他漏洞类型、其他平台未验证。
- 基准仅 20 个对象 / 40 个案例，规模小。
- 依赖 COM 元数据与专用调试接口；闭卷程度是「无源码」，不是「无任何系统信息」。
- 生产发现依赖 MSRC 确认，存在披露延迟与选择偏差。
- 没有公开的统一评测协议/排行榜，难以跨团队比较。

## 可引用原句

> "Slyp ... discovers 28 previously unknown vulnerabilities across nine COM services, all confirmed by MSRC with 16 CVEs assigned and $140,000 in bounties."

> "Production coding agents in their default setup ... verify essentially no cases."

## 相关页面

- [[slyp]]
- [[veritas]]
- [[vulbinllm]]
- [[binary-closed-book-guide]]
- [[open-book-vs-closed-book]]
- [[benchmark-validity]]

## 来源

- `raw/papers/Slyp_arXiv_2605.05000v1.pdf`
