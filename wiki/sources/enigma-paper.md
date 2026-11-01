---
title: 来源摘要：EnIGMA
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/EnIGMA_arXiv_2409.16165v3.pdf
tags: [source, agent, ctf, tool-interface, data-leakage]
---

# 来源摘要：EnIGMA

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | EnIGMA: Interactive Tools Substantially Assist LM Agents in Finding Security Vulnerabilities |
| 作者 / 机构 | Talor Abramovich, Meet Udeshi, Minghao Shao 等（Tel Aviv / NYU / Princeton / Stanford） |
| 版本 / 日期 | arXiv v3（2025-06-05）；ICML 2025 |
| 本地文件 | `raw/papers/EnIGMA_arXiv_2409.16165v3.pdf` |
| 原始 URL | https://arxiv.org/abs/2409.16165 ；https://enigma-agent.com/ |

## 核心主张

1. CTF/安全任务的关键瓶颈不只是模型推理，而是 **Agent-Computer Interface（ACI）**。
2. 让 agent 能操作交互式终端程序（gdb、nc 等），比给它更多文本上下文更有效。
3. 在 4 个基准、390 个挑战上取得 SOTA，NYU CTF 从 4% 提升到 13.5%。
4. 数据泄漏可以被量化；并提出「soliloquizing」现象：模型不与环境交互，自生成虚假观察。

## 关键数字

| 指标 | 数值 | 口径 / 备注 |
|---|---|---|
| 评测规模 | 390 个 CTF 挑战 / 4 个基准 | NYU CTF、InterCode-CTF、CyBench、HTB |
| NYU CTF | 13.5%（27/200） | Claude 3.5 Sonnet；此前最好 4% |
| CyBench | 20.0% | Claude 3.5 Sonnet；CyBench agent 17.5% |
| InterCode-CTF | 比此前最好高 29 个百分点 | 论文 Table 1 |
| 单实例预算 | $3 | 超出即标记 unsolved（exit_cost） |
| IAT 消融 | 29.5 → 27.4（-2.1） | 去掉 debugger/connect 工具 |
| 去掉 demonstrations | 29.5 → 23.3（-6.2） | 四项基准聚合 |
| 成功步数 | 多数在前 20 步 | 失败常表现为长时间低进展 |

## 方法

- **Interactive Agent Tools（IATs）**：把 gdb、服务器连接等交互式程序封装成 agent 可调用接口。
- **Summarizer**：对长终端输出做摘要，控制上下文。
- **Demonstrations**：给 agent 少量工具使用示例。
- **评测**：在 NYU CTF、InterCode-CTF、CyBench、HTB 上统一比较；预算按美元计。
- **泄漏分析**：人工检查成功轨迹，统计「解法泄漏」与 soliloquy 的比例。

## 方法学启示

- **工具接口是难度变量**：给不给 gdb，直接改变 pwn/rev 类任务的可解性；这与「二进制闭卷」中「工具清单」必须锁定的判断一致。
- **预算用美元而非步数**：更接近真实部署成本，也能防止「无限步数」掩盖能力差异。
- **soliloquizing 是新的 reward hacking 形式**：模型自造观察，等于绕过环境；必须靠轨迹审计发现。
- **泄漏要分类型**：解法泄漏（见过答案）与自生成观察是两种不同污染。

## 局限

- 工具收益按类别不同：crypto/pwn/rev 提升，web 反而下降（缺少合适的 Web 交互工具）。
- demonstrations 并非总是有益，可能限制探索。
- 仅 CTF，不是真实二进制/真实应用。
- 泄漏检测依赖人工检查成功轨迹，难以规模化。
- 单实例 $3 预算会截断长尾任务。

## 可引用原句

> "These novel Interactive Agent Tools enable LM agents, for the first time, to run interactive utilities, such as a debugger and a server connection tool."

> "Soliloquizing, where the model self-generates hallucinated observations without interacting with the environment."

## 相关页面

- [[enigma]]
- [[agent-harness]]
- [[cybench]]
- [[nyu-ctf-bench]]
- [[benchmark-validity]]
- [[cost-aware-evaluation]]

## 来源

- `raw/papers/EnIGMA_arXiv_2409.16165v3.pdf`
