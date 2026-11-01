---
title: Slyp
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
tags: [entity, binary-only, cots, windows, agent, real-world]
---

# Slyp

## 一句话

UIUC 的端到端二进制 Agent：在无源码的 Windows COM 二进制中发现竞态漏洞，并生成 debugger-verified PoC；基准 0.973 F1，生产环境发现 28 个未知漏洞、16 个 CVE。

**评测范式定位**：目前最接近「二进制闭卷 + 真实利用」的公开工作；但任务是 COM 竞态这一类，且仍提供 COM 元数据与调试接口。

> [推断] 「最接近」的依据是：不给源码、产出 debugger-verified PoC、在真实 COTS 服务上发现 0day；但它不是通用基准，不能代表跨漏洞类型的二进制闭卷能力。

## 规模与结果

| 维度 | 数值 | 说明 |
|---|---|---|
| 基准 | 20 个 COM 对象 / 40 个漏洞案例 | 竞态条件 |
| F1 | 0.973（GPT-5.4） | 生产 coding agent 最高 0.910 |
| 静态分析对照 | COMRace++ 0.299 F1 | Slyp 约 3.3× |
| PoC 生成 | 67.5%（27/40） | 最强配置；四个配置合计 34/40 |
| 生产发现 | 28 个未知漏洞 / 9 个 COM 服务 | MSRC 全部确认 |
| CVE / 赏金 | 16 个 CVE / $140,000 | 论文口径 |
| 模型 | 6 个 backbone | GPT / Claude，model-agnostic |

## 方法

- **ReAct loop** + 长程二进制分析工程：
  - pre-compaction checkpointing：跨上下文压缩保留跨函数发现；
  - task verification：防止过早终止。
- **三个 MCP 工具接口**：
  1. 二进制探索：反编译、虚表/虚调用解析；
  2. COM 检查：接口元数据、激活模板；
  3. 动态调试：编译、执行、崩溃捕获。
- **PoC 验证**：必须由 debugger 确认，不只是「看起来像」。
- **对照**：Codex、Claude Code 在相同 prompt 下比较；加全部二进制工具后仍有差距。

## 为什么重要

- **真实 0day 外部效度**：28 个 MSRC 确认漏洞，不是合成题。
- **工具接口决定上限**：生产 agent 默认工具下几乎无法验证 PoC；专用工具后大幅提升。
- **发现 + 利用一体**：不是只做漏洞检测，而是端到端 PoC。
- **model-agnostic**：pipeline 不绑定单一模型，说明工程 scaffold 的重要性。

## 局限

- 只覆盖 Windows COM 竞态；其他漏洞类型/平台未验证。
- 基准仅 20 个对象 / 40 个案例，规模小。
- 依赖 COM 元数据与专用调试接口，闭卷程度是「无源码」，不是「无系统信息」。
- 生产发现依赖 MSRC 确认，存在披露延迟与选择偏差。
- 没有公开统一评测协议/排行榜，跨团队不可比。

## 对二进制闭卷的启示

- **最接近的目标形态**：只给二进制 + 调试接口，要求 PoC；可作为项目页 M1 的参照架构。
- **工具清单必须显式**：Slyp 的收益很大程度来自 COM/调试工具，二进制闭卷必须把工具集作为难度变量。
- **PoC 而非「崩溃」**：debugger-verified PoC 比任意崩溃更接近可利用性。
- **真实漏洞是最终效度**：合成题高分不够，需在真实 COTS 软件上验证。

## 相关页面

- [[veritas]]
- [[vulbinllm]]
- [[aixcc]]
- [[binary-closed-book-guide]]
- [[benchmark-validity]]

## 来源

- `raw/papers/Slyp_arXiv_2605.05000v1.pdf`；https://arxiv.org/abs/2605.05000
