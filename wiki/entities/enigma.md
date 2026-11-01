---
title: EnIGMA
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/EnIGMA_arXiv_2409.16165v3.pdf
tags: [entity, agent, ctf, tool-interface, data-leakage]
---

# EnIGMA

## 一句话

面向 CTF 的 LM Agent，核心贡献是把 **交互式终端工具（gdb、nc 等）** 封装成 agent 可调用接口；在 NYU CTF 上把最好成绩从 4% 提升到 13.5%，并系统量化数据泄漏与 soliloquizing。

**评测范式定位**：CTF 闭卷 + 工具接口研究；不是二进制闭卷基准，但直接证明「工具清单」是能力变量。

## 规模与结果

| 指标 | 数值 | 说明 |
|---|---|---|
| 评测规模 | 390 个挑战 / 4 个基准 | NYU、InterCode-CTF、CyBench、HTB |
| NYU CTF | 13.5%（27/200） | Claude 3.5 Sonnet；此前最好 4% |
| CyBench | 20.0% | 此前 17.5% |
| InterCode-CTF | +29pp | 相比此前最好 |
| 单实例预算 | $3 | 超出即 unsolved |
| IAT 消融 | 29.5 → 27.4 | 去掉 debugger/connect |
| 去掉 demonstrations | 29.5 → 23.3 | -6.2pp |

## 方法

- **IATs**：gdb、服务器连接等交互式程序，封装成 agent 可调用接口。
- **Summarizer**：压缩长终端输出，控制上下文。
- **Demonstrations**：少量工具使用示例。
- **预算**：按美元计，防止无限步数。
- **泄漏分析**：人工检查成功轨迹，统计解法泄漏与 soliloquy。

## 为什么重要

- **ACI 是能力的一部分**：给不给 gdb，直接改变 pwn/rev 的可解性。
- **预算用美元**：比步数更接近真实部署成本。
- **soliloquizing**：模型自造观察，绕过环境；必须靠轨迹审计发现。
- **类别差异**：crypto/pwn/rev 提升，web 反而下降（缺 Web 交互工具）。

## 局限

- 仅 CTF，不是真实二进制/应用。
- 工具收益按类别不同，不是通用增益。
- 泄漏检测依赖人工检查，难规模化。
- $3 预算会截断长尾任务。

## 相关页面

- [[enigma-paper]]
- [[cybench]]
- [[nyu-ctf-bench]]
- [[agent-harness]]
- [[benchmark-validity]]

## 来源

- `raw/papers/EnIGMA_arXiv_2409.16165v3.pdf`；https://arxiv.org/abs/2409.16165
