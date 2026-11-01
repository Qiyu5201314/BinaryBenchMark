---
title: VulBinLLM
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf
tags: [entity, binary-only, stripped-binary, vulnerability-detection, llm]
---

# VulBinLLM

## 一句话

UCLA/Cisco 的 stripped binary 漏洞检测框架：反编译时注入 CWE 注释 + 长上下文记忆 + CoT，在 Juliet 编译样本上做 CWE 分类，部分类别显著优于 LATTE。

**评测范式定位**：二进制闭卷的「检测层」；只判 CWE，不触发、不利用。

## 规模与结果

| CWE | LATTE F1 | Vul-BinLLM F1 |
|---|---|---|
| CWE-78 命令注入 | 96.33% | 91.70% |
| CWE-134 格式化字符串 | 93.99% | 99.48% |
| CWE-190 整数溢出 | 62.05% | 98.99% |
| CWE-606 循环条件 | 74.24% | 85.4% |

- 数据：Juliet v1.3，GCC 编译，去符号/调试信息，20,000+ 二进制。
- 模型：GPT-4o。
- 结论：CWE-190 提升最大；CWE-78 反而略低于 LATTE。

## 方法

- **反编译注释**：加入漏洞/弱点注释，但不改代码结构。
- **CWE 分类**：给多个候选 CWE 描述，让模型判断是否存在目标弱点。
- **长上下文**：记忆管理 agent 处理超窗口二进制。
- **防记忆**：每次查询混入多个 CWE。

## 为什么重要

- 证明 stripped binary 上「反编译 + LLM」可以做漏洞检测。
- 提供二进制检测的基线数字（F1 74–99%）。
- 揭示 CWE 之间的巨大差异：不是单一「二进制漏洞检测能力」。

## 局限

- 只在 Juliet 合成数据上评测，未验证真实项目/固件。
- 只做 CWE 分类，不做 PoV/利用。
- 注释注入属于信息契约灰区。
- 缺跨架构/优化级别的系统消融。

## 相关页面

- [[vulbinllm-paper]]
- [[veritas]]
- [[slyp]]
- [[binary-closed-book-guide]]
- [[benchmark-validity]]

## 来源

- `raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf`；https://arxiv.org/abs/2505.22010
