---
title: SEC-bench
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
tags: [entity, benchmark-construction, automation, poc, patching]
---

# SEC-bench

## 一句话

UIUC 提出的自动化安全基准构建框架：用多 agent 从真实 CVE 自动复现漏洞、生成 PoC 与金补丁，$0.87/实例构建 200 个任务；SOTA agent PoC 18.0%、补丁 34.0%。

**评测范式定位**：源码级安全工程任务（PoC + patch），不是二进制闭卷；但它的「自动造题」流水线是二进制闭卷最需要的能力。

> [推断] 「最需要」的依据是二进制样本构建成本高、版本矩阵复杂；但源码级 $0.87/instance 不能直接外推到二进制，后者会显著更贵。

## 规模与设置

| 维度 | 数值 | 说明 |
|---|---|---|
| 实例 | 200 个真实 CVE | 自动验证、隔离容器、金补丁 |
| 构建成本 | $0.87/instance | 比 CodeAct 单 agent 提升 85.7% |
| 任务 | PoC generation、vulnerability patching | 两个独立任务 |
| Agent | SWE-agent、OpenHands、Aider | 统一比较 |

## 主要结果

| 指标 | 数值 | 说明 |
|---|---|---|
| PoC 成功率 | ≤18.0% | 全量 200 实例 |
| 补丁成功率 | ≤34.0% | 全量 200 实例 |
| 80 子集 | SWE-agent 33.8% patch、12.5% PoC | 论文报告 |
| 全量 OpenHands | 34.0% patch、18.0% PoC | 论文报告 |

## 为什么重要

- **自动化造题**：把「人工 5–24 小时/CVE」降到 $0.87，直接回应规模瓶颈。
- **PoC + 金补丁双 oracle**：PoC 证明漏洞存在，金补丁证明修复方向。
- **多 agent 构建器**：Preprocessor / Verifier / Evaluator 分工，可迁移到二进制样本流水线。
- **成本可计量**：为「造 100+ 题」提供了工程可行性证据。

## 局限

- 源码级，不是二进制闭卷。
- CVE 选择偏向有 bug report / 公开 PoC 的样本。
- 构建依赖网络、依赖版本、构建环境，长尾项目失败率高。
- 金补丁可能只修表面症状；PoC 通过 ≠ 语义正确。
- 200 个实例的漏洞类型分布未充分平衡。

## 对二进制闭卷的启示

- 可复用「Preprocessor → Verifier → Evaluator」三段式：抽取 → 复现 → 打包评分。
- 二进制版本需要额外的「构建矩阵」：vuln / fixed_single / fixed_release / asan_vuln / asan_fixed。
- 自动化验证器本身要抽样审计，否则批量污染。
- $0.87 是源码级成本；二进制构建（多架构、多优化级别）会显著更高，需要分层投入。

## 相关页面

- [[aixcc]]
- [[vulnerability-reproduction]]
- [[reproducibility]]
- [[benchmark-validity]]
- [[binary-closed-book-guide]]

## 来源

- `raw/papers/SEC-bench_arXiv_2506.11791v1.pdf`；https://arxiv.org/abs/2506.11791
- 代码：https://github.com/SEC-bench/SEC-bench
- 数据：https://hf.co/datasets/SEC-bench/SEC-bench
