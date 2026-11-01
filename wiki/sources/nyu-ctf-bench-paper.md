---
title: 来源摘要：NYU CTF Bench
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/NYU_CTF_Bench_arXiv_2406.05590v3.pdf
  - raw/github/NYU_CTF_Bench_README.md
tags: [source, benchmark, ctf, pwn, reverse-engineering]
---

# 来源摘要：NYU CTF Bench

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | NYU CTF Bench: A Scalable Open-Source Benchmark Dataset for Evaluating LLMs in Offensive Security |
| 作者 / 机构 | Minghao Shao, Sofija Jancheska, Meet Udeshi 等（NYU / NYU Abu Dhabi） |
| 版本 / 日期 | arXiv v3（2025-02-18）；NeurIPS 2024 Datasets & Benchmarks |
| 本地文件 | `raw/papers/NYU_CTF_Bench_arXiv_2406.05590v3.pdf`、`raw/github/NYU_CTF_Bench_README.md` |
| 原始 URL | https://arxiv.org/abs/2406.05590 ；https://github.com/NYU-LLM-CTF/NYU_CTF_Bench |

## 核心主张

1. 构建 200 个来自 CSAW CTF（2017–2023）的开源、可 docker 化基准。
2. 用统一的 function-calling 框架让 LLM 调用 8 类安全工具。
3. 把 CTF 当作可自动判分的 Agent 能力测试：拿到 flag 即成功。
4. 用「开发集/测试集」分离 agent 设计与最终评分，降低过拟合。

## 关键数字

| 指标 | 数值 | 口径 / 备注 |
|---|---|---|
| 挑战数 | 200 | 6 类：crypto、forensics、pwn、rev、web、misc |
| 开发集 | 55 | 可当作 train split，用于 agent 设计 |
| 模型数 | 5 | GPT-3.5、GPT-4、Claude、Mixtral、LLaMA |
| 工具数 | 8 | 含 pwntools、Ghidra 等，按类别启用 |
| GPT-4 最佳类别 | rev 9.80%、pwn 7.69%、forensics 6.67%、web 5.26% | crypto/misc 为 0 |
| Claude 最佳类别 | misc 8.33%、crypto 5.77%、rev 3.92%、pwn 2.56% | web 为 0 |
| 开源模型 | Mixtral / LLaMA 全 0 | 100% 失败为「答案错误」 |

## 方法

- **数据**：CSAW Quals/Finals 的 6 类挑战，统一为 `<year>/<event>/<category>/<challenge>`。
- **环境**：Docker + docker-compose；需要服务的挑战自动拉起。
- **Agent**：LLM function calling + 8 类工具，支持多轮交互。
- **判分**：flag 精确匹配；同时统计 5 类失败原因（放弃、轮次超限、连接失败、token 超限、答案错误）。
- **人类对照**：与 CSAW 2022/2023 的人类队伍得分比较。

## 方法学启示

- **开发集/测试集分离**是 CTF 基准的必需项，否则 agent 设计会污染测试分数。
- **失败原因分类**比单一成功率更有信息量：GPT-3.5/Claude 高「放弃率」，开源模型几乎全是「答案错误」。
- **pwn/rev 是二进制相关能力的代理**，但仍是 CTF 题，不是真实二进制闭卷。
- 与 Cybench 的差异：NYU 更早、更大（200 vs 40），但难度上限与人类时间锚点不如 Cybench 精细。

## 局限

- CTF 题目带提示、flag 结构固定，与真实漏洞利用有差距。
- 单类别样本量仍不足以做细粒度能力结论。
- 工具集固定，agent 的探索能力受接口限制。
- 人类对照来自不同年份/赛制，不能直接与 LLM 分数做严格比较。

## 可引用原句

> "The main benchmark dataset contains 200 challenges across 6 CTF categories."

> "A smaller development set of 55 challenges ... can be treated equivalent to a 'train' split."

## 相关页面

- [[nyu-ctf-bench]]
- [[cybench]]
- [[enigma]]
- [[difficulty-levels]]
- [[benchmark-validity]]

## 来源

- `raw/papers/NYU_CTF_Bench_arXiv_2406.05590v3.pdf`
- `raw/github/NYU_CTF_Bench_README.md`
