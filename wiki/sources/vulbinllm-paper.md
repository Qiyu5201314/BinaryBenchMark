---
title: 来源摘要：VulBinLLM
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf
tags: [source, binary-only, stripped-binary, vulnerability-detection, llm]
---

# 来源摘要：VulBinLLM

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | VulBinLLM: LLM-powered Vulnerability Detection for Stripped Binaries |
| 作者 / 机构 | Nasir Hussain, Haohan Chen, Chanh Tran 等（UCLA；Cisco Research） |
| 版本 / 日期 | arXiv v1（2025-05-28） |
| 本地文件 | `raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf` |
| 原始 URL | https://arxiv.org/abs/2505.22010 |

## 核心主张

1. stripped binary 的漏洞检测仍是开放问题；LLM 可以先反编译、再推理。
2. 在反编译阶段注入「漏洞/弱点注释」，但不改变代码结构与功能，可提升后续推理。
3. 用 in-context learning + chain-of-thought + 长期记忆，扩展可分析的二进制规模。
4. 在 Juliet 编译样本上，Vul-BinLLM 在部分 CWE 上显著优于 LATTE。

## 关键数字

| CWE | LATTE Acc | Vul-BinLLM Acc | LATTE F1 | Vul-BinLLM F1 |
|---|---|---|---|---|
| CWE-78（命令注入） | 96.46% | 96.55% | 96.33% | 91.70% |
| CWE-134（格式化字符串） | 93.88% | 99.74% | 93.99% | 99.48% |
| CWE-190（整数溢出） | 62.1% | 99.34% | 62.05% | 98.99% |
| CWE-606（循环条件未检查） | 73.33% | 74.54% | 74.24% | 85.4% |

- 数据：Juliet Test Suite v1.3，GCC 编译，去除调试信息与符号表，生成 20,000+ 二进制样本。
- 模型：GPT-4o。
- 结论：对 CWE-190 提升最大（F1 +36.9pp），对 CWE-78 反而略低于 LATTE。

## 方法

- **反编译优化**：在反编译结果中加入 CWE 相关注释，帮助模型聚焦数据流。
- **CWE 分类**：给模型多个候选 CWE 描述，要求判断是否存在目标弱点。
- **上下文扩展**：用记忆管理 agent 处理超出上下文窗口的二进制。
- **防记忆**：每次查询混入多个 CWE，避免模型只靠模式匹配。

## 方法学启示

- **二进制闭卷的「理解」层可以先于「利用」层被评测**：VulBinLLM 只做检测，不做触发/利用。
- **注释注入属于「信息契约」的灰区**：它不改变二进制，但改变了 agent 看到的反编译视图；在闭卷基准中必须明确是否允许。
- **CWE 级别不均衡**：同一方法在不同 CWE 上差异巨大，说明「二进制漏洞检测」不是一个单一能力。
- **合成数据的天花板**：Juliet 是合成样本，真实项目上的泛化未验证。

## 局限

- 只在 Juliet 合成数据集上评测，未覆盖真实项目/固件/商业软件。
- 只做 CWE 分类，不做 PoV 触发、根因归因或利用。
- 反编译注释由人工/规则注入，可能引入额外信息。
- 缺少跨架构（ARM/MIPS）与优化级别（O2/LTO）的系统消融。
- 未报告误报在真实代码库中的代价。

## 可引用原句

> "Recognizing vulnerabilities in stripped binary files presents a significant challenge in software security."

> "Vul-BinLLM adds vulnerability and weakness comments without altering the code structure or functionality."

## 相关页面

- [[vulbinllm]]
- [[veritas]]
- [[slyp]]
- [[binary-closed-book-guide]]
- [[open-book-vs-closed-book]]
- [[benchmark-validity]]

## 来源

- `raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf`
