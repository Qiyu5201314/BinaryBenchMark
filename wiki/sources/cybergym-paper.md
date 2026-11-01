---
title: 来源摘要：CyberGym 论文（ICLR 2026）
type: source
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/CyberGym_ICLR2026_CameraReady.pdf
tags: [source, paper, cybergym]
---

# 来源摘要：CyberGym 论文

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | CyberGym: Evaluating AI Agents' Real-World Cybersecurity Capabilities at Scale |
| 作者 | Zhun Wang, Tianneng Shi, Jingxuan He, Matthew Cai, Jialin Zhang, Dawn Song |
| 机构 | UC Berkeley |
| 版本 | arXiv v3（2026-03）；ICLR 2026 camera-ready |
| 原文 | `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`、`raw/papers/CyberGym_ICLR2026_CameraReady.pdf` |
| URL | https://arxiv.org/abs/2506.02548 ；https://openreview.net/forum?id=2YvbLQEdYt |

## 核心主张

1. 现有安全基准规模小、任务玩具化、或只测单一技能；需要大规模、真实、可执行验证的基准。
2. CyberGym 用 1,507 个真实历史漏洞（188 个项目）构建任务，要求 Agent 生成 PoC 复现漏洞。
3. 最强组合在 L1 仅 22.0%；说明真实漏洞复现仍很难。
4. 基准之外，Agent 还发现 34 个 0day、18 个不完整补丁，证明能力有现实影响。

## 方法

### 数据构建
- 从 OSS-Fuzz 漏洞语料回溯 patch commit。
- 抽取：pre/post 代码库、Docker 构建环境、原始 PoC、补丁、commit message。
- 用 GPT-4.1 改写 commit message 生成漏洞描述。
- 质量筛选：Informative / Reproducible / Non-redundant。

### 任务定义
- 主任务 L1：给漏洞描述 + pre-patch 代码，生成 PoC。
- 判定：pre-patch 崩溃、post-patch 不崩溃；sanitizer 为 oracle。
- 四级难度 L0–L3（见 [[difficulty-levels]]）。

### 实验
- 11 个 LLM，同一 OpenHands 框架。
- 4 个 Agent（OpenHands、Codex CLI、EnIGMA、Cybench agent）用同一 GPT-4.1。
- thinking / non-thinking 对比；步数、PoC 长度、难度等级分析。

## 关键结果

| 实验 | 结果 |
|---|---|
| 最好组合 L1 | 22.0%（GPT-5 + high reasoning） |
| Claude-Sonnet-4 L1 | 17.9% |
| GPT-4.1 L1 | 9.4%（UK AISI 引用） |
| L0 无描述 | 3.5% |
| 长 PoC（>100B，占 65.7%） | 约 10% |
| 四 Agent 成功并集 | 18.4%（接近单个最好结果两倍） |

## 外溢发现

- 759 个 PoC 在 60 个项目触发 post-patch 崩溃；确认 17 个不完整补丁（15 个项目）。
- 10 个此前未知 0day，平均潜伏 969 天。
- 开放式扫描 431 个 OSS-Fuzz 项目、1,748 个可执行文件：GPT-4.1 7 个 0day，GPT-5 22 个（4 个重叠）。

> 注意：博客/官网/论文对 0day 与不完整补丁的总数有 34/18 与 35/17 的差异，见 [[cybergym]]。

## 局限（作者自述）

- 只覆盖 C/C++ 内存安全、sanitizer 可检测漏洞。
- 不覆盖完整攻击链。
- 描述可能歧义。
- 未讨论成本（后续提交指南补充）。

## 可引用原句

> "The best-performing combination ... achieves only a 17.9% success rate."（Claude-Sonnet-4 + OpenHands，thinking 关闭）

> "For level 0, only 3.5% instances can be successfully reproduced without access to the text description."

> "The agent is tasked to create a PoC to reproduce the target vulnerability."

## 相关页面

- [[cybergym]]
- [[vulnerability-reproduction]]
- [[arvo-oss-fuzz]]
- [[benchmark-validity]]
- [[related-work]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/CyberGym_ICLR2026_CameraReady.pdf`
- https://arxiv.org/abs/2506.02548
