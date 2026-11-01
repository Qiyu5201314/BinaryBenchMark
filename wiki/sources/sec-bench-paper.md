---
title: 来源摘要：SEC-bench
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
tags: [source, benchmark-construction, automation, poc, patching]
---

# 来源摘要：SEC-bench

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | SEC-bench: Automated Benchmarking of LLM Agents on Real-World Software Security Tasks |
| 作者 / 机构 | Hwiwon Lee, Ziqi Zhang, Hanxiao Lu, Lingming Zhang（UIUC / Purdue） |
| 版本 / 日期 | arXiv v1（2025-06-13）；NeurIPS 2025（本地为 arXiv 预印本） |
| 本地文件 | `raw/papers/SEC-bench_arXiv_2506.11791v1.pdf` |
| 原始 URL | https://arxiv.org/abs/2506.11791 ；https://github.com/SEC-bench/SEC-bench |

## 核心主张

1. 现有安全基准依赖合成题或简化漏洞数据，无法代表真实安全工程。
2. 用多 agent scaffold **自动**从真实 CVE 构建「可复现 PoC + 金补丁」的基准。
3. 自动化把单实例构建成本降到 **$0.87**，使大规模扩展成为可能。
4. 在 200 个真实 CVE 上，SOTA code agent 的 PoC 生成最高 18.0%，补丁最高 34.0%。

## 关键数字

| 指标 | 数值 | 口径 / 备注 |
|---|---|---|
| 验证实例 | 200 个真实 CVE | 自动构建、隔离环境、金补丁 |
| 单实例成本 | $0.87 | 比 CodeAct 单 agent 脚手架提升 85.7% |
| PoC 生成成功率 | ≤18.0% | SWE-agent / OpenHands / Aider |
| 补丁成功率 | ≤34.0% | 同上 |
| 子集 | 80-instance subset | SWE-agent 33.8% patch、12.5% PoC |
| 全量 | 200 instances | OpenHands 34.0% patch、18.0% PoC |

## 方法

- **Preprocessor**：从 CVE 数据库与 bug report 中抽取实例，统一为可交互环境。
- **Verifier**：多 LLM agent 在隔离容器中复现漏洞，过滤无法可靠复现的样本。
- **Evaluator**：把验证后的实例打包成 Docker 镜像，附带 PoC 与金补丁，形成可评分任务。
- **任务**：PoC generation（生成触发漏洞的输入）与 vulnerability patching（生成修复 diff）。
- **质量**：强调「可复现 PoC」是判定漏洞与补丁有效性的前提。

## 方法学启示

- **样本制作可以自动化**：这直接回应了二进制闭卷「造题成本高」的核心风险。
- **金补丁 + PoC 是双 oracle**：PoC 证明漏洞存在，金补丁证明修复方向；两者结合可降低标注噪声。
- **成本是关键指标**：$0.87/instance 让「100+ 任务」从手工项目变成可流水线。
- **多 agent 构建器本身也需要验证**：Verifier 的误判会污染整个数据集。

## 局限

- 仍是源码级任务，不是二进制闭卷；但构建流程可迁移。
- CVE 选择偏向有公开 bug report / PoC 的样本，存在选择偏差。
- 自动构建依赖网络、依赖版本、构建环境，长尾项目仍会失败。
- 金补丁可能只修表面症状，PoC 通过不等于语义正确。
- 200 个实例的漏洞类型分布未充分平衡。

## 可引用原句

> "SEC-bench ... automatically creates high-quality software vulnerability datasets with reproducible artifacts at a cost of only $0.87 per instance."

> "SOTA code agents ... achieve at most 18.0% success in PoC generation and 34.0% in vulnerability patching."

## 相关页面

- [[sec-bench]]
- [[vulnerability-reproduction]]
- [[reproducibility]]
- [[benchmark-validity]]
- [[binary-closed-book-guide]]

## 来源

- `raw/papers/SEC-bench_arXiv_2506.11791v1.pdf`
