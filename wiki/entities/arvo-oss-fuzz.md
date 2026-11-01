---
title: ARVO 与 OSS-Fuzz
type: entity
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/official/BerkeleyRDI_CyberGym_Blog.txt
tags: [dataset, vulnerability-corpus, fuzzing, sanitizer]
---

# ARVO 与 OSS-Fuzz

## 一句话

OSS-Fuzz 是 Google 的持续模糊测试项目，产出大量真实崩溃；ARVO 是可复现的漏洞数据集。CyberGym 从 OSS-Fuzz 漏洞语料回溯 patch commit，构建「修补前 / 修补后」双版本任务。

## 两者关系

- **OSS-Fuzz**：持续对开源项目做 fuzzing，发现崩溃并记录 PoC、修复提交。
- **ARVO**：从 OSS-Fuzz 等来源整理出的**可复现**漏洞数据集，强调能稳定复现。
- **CyberGym**：按 patch commit 回溯，抽取 pre/post 代码库、Docker 构建环境、原始 PoC、补丁、commit message，并用 GPT-4.1 改写 commit message 生成漏洞描述。

来源：[[cybergym-repo-and-site]]、[[cybergym-paper]]。

## 为什么重要

- 提供**真实、可验证、可扩展**的漏洞来源，而不是人工构造的玩具题。
- sanitizer 作为可靠的判定 oracle：崩溃/不崩溃是客观信号。
- 支持大规模自动化构建：1,507 个实例、188 个项目。

## 数据质量流程

CyberGym 的筛选：

| 步骤 | 规则 |
|---|---|
| Informative | 移除缺少漏洞上下文、或一个 commit 修多个问题的记录 |
| Reproducible | 在 pre/post 可执行文件上重跑 ground-truth PoC |
| Non-redundant | 通过 crash trace 比较去重 |

来源：[[cybergym-paper]]。

## 局限

- 主要覆盖 C/C++ 内存安全，sanitizer 可检测的崩溃类型。
- 漏洞描述由 commit message 改写，可能不完整或有歧义。
- 依赖历史数据，存在知识截止/污染风险（模型可能见过 patch）。

来源：[[cybergym-paper]]、[[benchmark-validity]]。

## 相关页面

- [[cybergym]]
- [[vulnerability-reproduction]]
- [[benchmark-validity]]
- [[difficulty-levels]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`；https://arxiv.org/abs/2506.02548
- `raw/official/BerkeleyRDI_CyberGym_Blog.txt`；https://rdi.berkeley.edu/blog/cybergym/
