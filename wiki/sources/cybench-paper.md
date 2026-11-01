---
title: 来源摘要：Cybench
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/github/Cybench_README.md
tags: [source, benchmark, ctf, agent, harness]
---

# 来源摘要：Cybench

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | Cybench: A Framework for Evaluating Cybersecurity Capabilities and Risks of Language Models |
| 作者 / 机构 | Andy K. Zhang, Neil Perry, Riya Dulepet 等（Stanford） |
| 版本 / 日期 | arXiv v4（2025-04-12）；ICLR 2025 Oral |
| 本地文件 | `raw/papers/Cybench_arXiv_2408.08926v4.pdf`、`raw/github/Cybench_README.md` |
| 原始 URL | https://arxiv.org/abs/2408.08926 ；https://cybench.github.io/ |

## 核心主张

1. 用 40 个专业级 CTF 任务构造开源、可复现的 Agent 网络安全基准。
2. 用「人类首次解出时间（FST）」给任务难度提供客观锚点。
3. 为难题设计**子任务（subtasks）**，提供部分分与更细的能力画像。
4. 系统比较 8 个模型 × 4 种 agent scaffold，证明 harness 是重要混杂变量。

## 关键数字

| 指标 | 数值 | 口径 / 备注 |
|---|---|---|
| 任务数 | 40 | 4 个 CTF：HackTheBox、SekaiCTF、Glacier、HKCert；2022–2024 |
| 类别 | 6 | crypto、web、rev、forensics、pwn、misc |
| 模型数 | 8 | GPT-4o、o1-preview、Claude 3 Opus/3.5 Sonnet、Mixtral、Gemini 1.5 Pro、Llama 3 70B/3.1 405B |
| Scaffold | 4 | structured bash、action-only、pseudoterminal、web search |
| 最强 unguided | 17.5% | Claude 3.5 Sonnet |
| 最强 subtask-guided | 17.5% | GPT-4o |
| 最强 subtask 分 | 46.8% | OpenAI o1-preview |
| 难度跨度 | FST 2 分钟–24 小时 54 分 | 约 136× |
| 能力阈值 | 约 11 分钟 | 无子任务提示时，FST > 11 分钟的任务基本解不出 |

## 方法

- **任务规格**：description + starter files + evaluator（flag 匹配）。
- **环境**：Kali Linux 容器，agent 执行 bash、观察输出、维护 memory。
- **子任务**：把「找凭证 → 找漏洞代码 → 构造 exploit → 取 flag」拆成可顺序计分的小问。
- **评分**：unguided（二值）、subtask-guided（最终子任务二值）、subtask（子任务比例分）。
- **效率**：记录 token 与时间。

## 方法学启示

- **FST 是可迁移的难度标尺**：比「简单/中等/困难」的主观标签更可复核。
- **子任务是部分分的最小实现**：不改变最终目标，但让失败可归因。
- **harness 必须报告**：同一模型换 scaffold，结果会变；Claude 3.5 与 GPT-4o 对 scaffold 的响应方向不同。
- **污染控制**：任务集中在 2022–2024，近半在 2023-12 之后发布。

## 局限

- CTF 任务天然带提示与游戏化结构，不等同真实渗透。
- 40 个任务的类别分布不均，单类别样本量小。
- flag 匹配只判最终答案，不判攻击路径质量。
- 子任务由作者编写，可能引入作者对「正确路径」的偏好。
- 模型与 scaffold 的交互显著，跨论文比较必须锁定设置。

## 可引用原句

> "Without subtask guidance, agents ... successfully solved complete tasks that took human teams up to 11 minutes to solve."

> "The effects of agent scaffolding are model-dependent."

## 相关页面

- [[cybench]]
- [[nyu-ctf-bench]]
- [[enigma]]
- [[agent-harness]]
- [[difficulty-levels]]
- [[benchmark-validity]]

## 来源

- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`
- `raw/github/Cybench_README.md`
