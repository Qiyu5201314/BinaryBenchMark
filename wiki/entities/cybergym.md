---
title: CyberGym
type: entity
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/CyberGym_ICLR2026_CameraReady.pdf
  - raw/github/CyberGym_README.md
  - raw/official/CyberGym_Benchmark_Page.txt
  - raw/official/BerkeleyRDI_CyberGym_Blog.txt
  - raw/official/UK_AISI_Inspect_CyberGym.txt
tags: [benchmark, vulnerability-reproduction, binary, open-book, uc-berkeley]
---

# CyberGym

## 一句话

UC Berkeley 提出的大规模网络安全基准：给 Agent 一个真实历史漏洞的**文字描述 + 未修补代码库**，要求它生成能触发该漏洞的 PoC，并在「修补前崩溃、修补后不崩溃」的双版本上自动验证。

**评测范式定位**：开卷（给了漏洞描述与代码）、单点能力（漏洞复现），不等于端到端攻击。详见 [[open-book-vs-closed-book]]。

## 规模与数据来源

| 维度 | 数值 | 说明 |
|---|---|---|
| 漏洞实例 | **1,507** | 来自真实历史漏洞 |
| 开源项目 | **188** | 含 OpenSSL、FFmpeg、OpenCV 等 |
| 崩溃类型 | 28 | buffer overflow、null pointer dereference 等 |
| 数据来源 | OSS-Fuzz → ARVO | 通过 patch commit 回溯 pre/post 版本 |
| 主要范围 | C/C++ 内存安全 | 可由 sanitizer 检测的漏洞 |

来源：[[cybergym-paper]]、[[cybergym-repo-and-site]]。

> [推断] 「1,507 个漏洞 / 188 个项目」是论文与官网一致的口径；博客早期版本写作 1,507 / 188，但外溢发现数字有 34 vs 35 的差异（见下）。

## 任务分级（L0–L3）

| 等级 | Agent 拿到什么 | 模拟场景 |
|---|---|---|
| **L0** | 只有未修补代码库 | 开放式漏洞发现 |
| **L1** | 代码库 + 漏洞文字描述（位置、类型、根因线索） | **主任务**：从报告到 PoC |
| **L2** | L1 + ground-truth PoC 产生的崩溃栈 | 有漏洞位置信息 |
| **L3** | L2 + 官方补丁 diff + 修补后代码 | one-day / patch-to-exploit |

来源：[[cybergym-paper]]、[[difficulty-levels]]。

## 验证方式

Agent 提交 PoC 后，服务端在 **pre-patch** 版本执行，要求崩溃；再在 **post-patch** 版本执行，要求不再崩溃。使用 sanitizer 作为判定 oracle。

- 只给 Agent pre-patch 代码（`repo-vul.tar.gz`），**不给** post-patch 镜像；post-patch 只用于服务端验证。
- 主榜要求 Agent 指定**唯一最终提交**，报告 final-submission 指标，避免 any-of 指标奖励暴力尝试。
- 开启网络访问会引入 reward hacking（直接搜 issue / patch / PoC），建议用域名白名单代理。

来源：[[cybergym-repo-and-site]]、[[benchmark-validity]]。

## 主要结果

| 设置 | 结果 | 口径 |
|---|---|---|
| 论文主任务 L1，最好组合 | **22.0%** | GPT-5 + high reasoning（thinking） |
| 论文 L1，Claude-Sonnet-4 | 17.9% | OpenHands，thinking 关闭 |
| 论文 L1，GPT-4.1 | 9.4% | UK AISI 复现页引用论文数字 |
| L0（无描述） | **3.5%** | 说明无先验知识时极难 |
| 博客（2025-10） | ~30% 单次；~67% 30 次 | 早期/不同评测设置 |
| 官网 leaderboard（2026） | 论文时代 22%，后续提交显著更高 | 官网声明分数随运行波动、描述可能歧义 |
| 提交指南（2026-08-04） | 「最强模型在无约束资源下已接近饱和」 | 因此要求同时报告成本 |

> **重要口径冲突**：公众号文章称「国内外不少团队已把 CyberGym 分数推到 95 以上」，提交指南也称「接近饱和」，而论文主结果是 22.0%。两者可能都真实，但**测量设置不同**（模型代际、trial 数、预算、是否给动态环境、是否联网、是否 any-of）。引用时必须写清设置，不能把 95% 与 22% 直接对比。详见 [[research/open-questions]] 的「口径冲突」。

来源：[[cybergym-paper]]、[[cybergym-repo-and-site]]、[[fudan-and-wechat-articles]]。

## 外溢发现（benchmark 之外的现实影响）

- 论文/官网：**34 个 0day**、**18 个历史不完整补丁**。
- 早期博客：35 个 0day、17 个不完整补丁。
- 具体链路：759 个生成 PoC 在 60 个项目中触发崩溃；人工确认 17 个不完整补丁（15 个项目）；进一步验证得到 10 个此前未知 0day，平均潜伏 969 天；开放式扫描 431 个 OSS-Fuzz 项目、1,748 个可执行文件，GPT-4.1 发现 7 个、GPT-5 发现 22 个（4 个重叠）。

> 数字差异来自「论文最终版 / 官网 / 早期博客」的统计时点与去重口径不同。引用时优先用论文或官网，并注明版本。

来源：[[cybergym-paper]]、[[cybergym-repo-and-site]]。

## 关键发现

- **Thinking 有效但有限**：GPT-5 从 7.7%（minimal reasoning）升到 22.0%（high reasoning）；其他模型提升较小。
- **信息越多越容易**：L2 栈、L3 补丁显著提升成功率；L0 仅 3.5%。
- **长 PoC 更难**：PoC 长度 >100 字节的实例占 65.7%，成功率仅约 10%。
- **步数收益递减**：成功集中在 20–80 步，100 步上限是能力与成本的平衡。
- **Agent 行为不同但成功率相近**：OpenHands 更常用 Bash 命令链，CTF 专用 Agent 更常写 Python 脚本；不同 Agent 的成功并集接近单个最好结果的两倍，说明能力互补。

来源：[[cybergym-paper]]、[[cybergym-repo-and-site]]。

## 局限

- 只覆盖 **C/C++ 内存安全**、sanitizer 可检测的漏洞，不代表完整漏洞谱系。
- 主任务是**开卷复现**，不测量「发现入口 → 打点 → 横向移动」的端到端攻击。
- 依赖文字描述质量；描述歧义会影响分数。
- 网络访问、动态分析环境、any-of 计分都可能引入 reward hacking。
- 不同团队自行提交、运行随机，排行榜分数不可直接跨设置比较。

来源：[[cybergym-paper]]、[[cybergym-repo-and-site]]、[[benchmark-validity]]。

## 相关页面

- [[agentcyberrange]]：从开卷复现走向闭卷实战的对照工作
- [[vulnerability-reproduction]]
- [[difficulty-levels]]
- [[arvo-oss-fuzz]]
- [[benchmark-validity]]
- [[evaluation-isolation]]
- [[uk-aisi-inspect-cybergym]]：第三方复现与工程细节
- [[comparisons/cybergym-vs-agentcyberrange]]

## 来源

- 论文：`raw/papers/CyberGym_arXiv_2506.02548v3.pdf`、`raw/papers/CyberGym_ICLR2026_CameraReady.pdf`；https://arxiv.org/abs/2506.02548
- 仓库：`raw/github/CyberGym_README.md`、`raw/github/CyberGym_FAQ.md`、`raw/github/CyberGym_SUBMISSION.md`；https://github.com/sunblaze-ucb/cybergym
- 官网：`raw/official/CyberGym_Benchmark_Page.txt`；https://www.cybergym.io/cybergym/
- 博客：`raw/official/BerkeleyRDI_CyberGym_Blog.txt`；https://rdi.berkeley.edu/blog/cybergym/
- 第三方复现：`raw/official/UK_AISI_Inspect_CyberGym.txt`；https://ukgovernmentbeis.github.io/inspect_evals/evals/cybergym/index.html
