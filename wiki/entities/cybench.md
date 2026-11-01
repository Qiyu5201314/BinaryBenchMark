---
title: Cybench
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/github/Cybench_README.md
tags: [entity, benchmark, ctf, agent, harness]
---

# Cybench

## 一句话

Stanford 提出的专业级 CTF Agent 基准：40 个 2022–2024 年 CTF 任务、6 类、带人类首次解出时间（FST）与子任务，系统比较 8 个模型 × 4 种 scaffold。

**评测范式定位**：闭卷 CTF（给题面与起始文件，不给解法），包含 pwn/rev 等二进制相关类别，但不是「只给二进制」。

## 规模与设置

| 维度 | 数值 | 说明 |
|---|---|---|
| 任务 | 40 | HackTheBox、SekaiCTF、Glacier、HKCert |
| 类别 | 6 | crypto、web、rev、forensics、pwn、misc |
| 难度锚 | FST 2 分钟–24 小时 54 分 | 人类首次解出时间 |
| 模型 | 8 | GPT-4o、o1-preview、Claude 3 Opus/3.5、Mixtral、Gemini、Llama |
| Scaffold | 4 | structured bash、action-only、pseudoterminal、web search |
| 子任务 | 每个任务可有 | 提供部分分与分阶段诊断 |

## 主要结果

| 指标 | 数值 | 说明 |
|---|---|---|
| 最强 unguided | 17.5% | Claude 3.5 Sonnet |
| 最强 subtask-guided | 17.5% | GPT-4o |
| 最强 subtask 分 | 46.8% | o1-preview |
| 能力阈值 | 约 11 分钟 FST | 无提示时超过即基本解不出 |

## 为什么重要

- **FST 是可复核的难度标尺**：比主观难度标签更稳定。
- **子任务是部分分的最小实现**：保留最终目标，同时让失败可归因。
- **harness 被显式比较**：同一模型换 scaffold 会变，跨论文比较必须锁定设置。
- **pwn/rev 是二进制能力的代理**：可作为二进制闭卷的「上游」参照。

## 局限

- CTF 带提示与游戏化结构，不等于真实漏洞利用。
- 40 个任务、单类别样本少。
- flag 匹配只判答案，不判路径。
- 子任务由作者编写，可能偏向标准解法。
- 不是二进制闭卷：题面、起始文件、flag 都提供了额外信息。

## 对二进制闭卷的启示

- 可直接复用「FST + 子任务」的难度设计：二进制题也可以用人类专家首次解出时间锚定难度。
- scaffold 必须作为变量报告；二进制闭卷里对应「工具清单 + harness」。
- 子任务可映射到八级任务谱系（理解 → 定位 → 触发 → 利用）。

## 相关页面

- [[cybench]]
- [[nyu-ctf-bench]]
- [[enigma]]
- [[autopenbench]]
- [[difficulty-levels]]
- [[agent-harness]]

## 来源

- [[cybench-paper]]
- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`；https://arxiv.org/abs/2408.08926
- `raw/github/Cybench_README.md`；https://github.com/andyzorigin/cybench
