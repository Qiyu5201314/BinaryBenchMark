---
title: 来源摘要：Veritas
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
tags: [source, binary-only, stripped-binary, agent, semantic-grounding]
---

# 来源摘要：Veritas

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | Veritas: Grounding LLM Agents for Reliable Vulnerability Reasoning over Stripped Binaries |
| 作者 / 机构 | Xinran Zheng, Alfredo Pesoli, Marco Valleri, Suman Jana, Lorenzo Cavallaro（UCL / BynarIO / Columbia） |
| 版本 / 日期 | arXiv v3（2026-07-18；v1 2026-05-14） |
| 本地文件 | `raw/papers/Veritas_arXiv_2605.15097v3.pdf` |
| 原始 URL | https://arxiv.org/abs/2605.15097 |

## 核心主张

1. Agent 能定位可疑代码，但常常丢失「决定漏洞是否成立」的语义证据，形成 **semantic gap**。
2. stripped binary 下这个 gap 更严重：证据分散在有噪声的 lifted IR 与有损的反编译视图里。
3. 解法是把漏洞推理拆成三步：**静态切片 → 双视图声明 → 运行时验证**。
4. 只有把「声明」绑定到 witness-backed 数据流并用运行时 oracle 确认，才可靠。

## 关键数字

| 指标 | 数值 | 口径 / 备注 |
|---|---|---|
| 基准 | 20 个真实 OOB 漏洞 / 10 个项目 | 带 flow-level ground truth；构建耗时 40+ 小时 |
| Recall | 90%（18/20） | stripped binary |
| 误报 | 623 个候选全部验证，0 FP；抽样 957 个候选，2 FP | 验证成本高 |
| AFL++ | 10% recall，0 FP | 受限于有界 fuzzing 预算 |
| RepoAudit | 30%（6/20） | agentic baseline |
| Codex / Claude Code | 25% / 35% recall | Claude Code 误报多 |
| 单候选成本 | Discover 约 30.3 秒、$0.30 | Validator 是主要成本 |

## 方法

- **Slicer**：从 lifted LLVM IR 恢复 witness-backed 的 source-to-sink 流；输出可检查的传播证据。
- **Discover**：把反编译代码与 IR witness 对齐，构造漏洞声明（claim），而不是让模型自由发挥。
- **Validator**：多 agent 通过引导式调试与运行时 oracle（Valgrind 等）确认声明。
- **实例化**：只做 out-of-bounds 这一类漏洞；其他类型留作未来工作。
- **对照**：静态分析、动态分析、二进制分析、fuzzing、agentic baseline；source-level baseline 作为强参考。

## 方法学启示

- **「定位」不等于「证明」**：LLM 可以指出 sink，但会丢掉 sizeof 缩放、模运算回绕、对象边界等决定性细节。
- **静态证据 + 运行时证据**是二进制闭卷 Oracle 的可行组合：静态负责召回，运行时负责精确。
- **验证成本是瓶颈**：623 个候选、957 次验证，说明高召回必须配合低成本剪枝。
- **可解释的中间产物**（witness、claim、runtime trace）本身就是评分依据，而不只是最终崩溃。

## 局限

- 只覆盖 out-of-bounds；UAF、类型混淆、竞态未验证。
- 基准仅 20 个样本，规模小，统计功效有限。
- 依赖 RetDec lifted IR 与 Valgrind，工具链重；优化二进制仍是 future work。
- 运行时验证主导成本，难以直接扩展到大规模基准。
- 真实案例只有 1 个 Apple CVE，外部效度有限。

## 可引用原句

> "Vulnerability reasoning requires more than access to program artifacts."

> "Veritas achieves 90% recall ... reports no false positives among 623 exhaustively validated candidates."

## 相关页面

- [[veritas]]
- [[vulbinllm]]
- [[slyp]]
- [[binary-closed-book-guide]]
- [[benchmark-validity]]
- [[reproducibility]]

## 来源

- `raw/papers/Veritas_arXiv_2605.15097v3.pdf`
