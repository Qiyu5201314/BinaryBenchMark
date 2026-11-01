---
title: Veritas
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
tags: [entity, binary-only, stripped-binary, agent, semantic-grounding]
---

# Veritas

## 一句话

UCL/BynarIO/Columbia 的 stripped binary 漏洞推理框架：把「定位」升级为「可验证的声明」——静态切片给 witness，LLM 构造 claim，多 agent 用运行时 oracle 验证；20 个真实 OOB 案例 90% recall。

**评测范式定位**：二进制闭卷的「检测/推理层」；不做利用，但提供了可复用的 Oracle 设计。

## 规模与结果

| 指标 | 数值 | 说明 |
|---|---|---|
| 基准 | 20 个真实 OOB / 10 个项目 | flow-level ground truth；构建 40+ 小时 |
| Recall | 90%（18/20） | stripped binary |
| 误报 | 623 候选 0 FP；957 抽样 2 FP | 验证成本高 |
| AFL++ | 10% recall | 有界 fuzzing |
| RepoAudit | 30%（6/20） | agentic baseline |
| Codex / Claude Code | 25% / 35% | Claude Code 误报多 |
| 单候选成本 | 30.3 秒 / $0.30 | Validator 为主 |

## 方法

- **Slicer**：从 lifted LLVM IR 恢复 witness-backed source-to-sink 流。
- **Discover**：对齐反编译代码与 IR witness，构造漏洞 claim。
- **Validator**：多 agent 引导式调试 + 运行时 oracle（Valgrind 等）。
- **范围**：只做 out-of-bounds；其他类型留作未来工作。

## 为什么重要

- **定位 ≠ 证明**：LLM 会丢掉 sizeof 缩放、模回绕、对象边界等决定性细节。
- **静态 + 运行时**是二进制 Oracle 的可行组合：静态负责召回，运行时负责精确。
- **中间产物可评分**：witness、claim、runtime trace 本身就是评分依据。
- **成本可计量**：为「高召回 + 低误报」提供了成本基线。

## 局限

- 只覆盖 OOB；UAF、类型混淆、竞态未验证。
- 20 个样本规模小。
- 依赖 RetDec + Valgrind，工具链重；优化二进制未覆盖。
- 运行时验证主导成本，难规模化。

## 相关页面

- [[veritas-paper]]
- [[vulbinllm]]
- [[slyp]]
- [[binary-closed-book-guide]]
- [[benchmark-validity]]
- [[reproducibility]]

## 来源

- `raw/papers/Veritas_arXiv_2605.15097v3.pdf`；https://arxiv.org/abs/2605.15097
