---
title: 来源摘要：AutoPenBench
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf
  - raw/github/AutoPenBench_README.md
tags: [source, benchmark, penetration-testing, milestones]
---

# 来源摘要：AutoPenBench

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | AutoPenBench: Benchmarking Generative Agents for Penetration Testing |
| 作者 / 机构 | Luca Gioacchini, Marco Mellia, Idilio Drago 等（Politecnico di Torino / Università di Torino / NEC Labs Europe） |
| 版本 / 日期 | arXiv v2（2024-10-28） |
| 本地文件 | `raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf`、`raw/github/AutoPenBench_README.md` |
| 原始 URL | https://arxiv.org/abs/2410.03225 ；https://github.com/lucagioacchini/auto-pen-bench |

## 核心主张

1. 现有渗透测试 agent 评测缺乏统一、可复现、可解释的框架。
2. 用 33 个任务（in-vitro + real-world）覆盖从服务发现到提权、取 flag 的完整链条。
3. 用「命令里程碑 + 阶段里程碑」给出部分分，而不只看最终 flag。
4. 对比全自主与半自主（人机协作）两种 agent 架构。

## 关键数字

| 指标 | 数值 | 口径 / 备注 |
|---|---|---|
| 任务数 | 33 | 22 个 in-vitro + 11 个 real-world |
| in-vitro 类别 | 4 | Access Control、Web Security、Network Security、Cryptography |
| real-world 案例 | 11 | 含 Log4j2、Spring4Shell、SambaCry、Heartbleed 等 |
| 全自主 SR | 21% | 27% in-vitro / 9% real-world |
| 半自主 SR | 64% | 59% in-vitro / 73% real-world |
| 阶段数 | 6 | Target Discovery → Infiltration → Vuln Detection → PrivEsc → Flag → Success |

## 方法

- **环境**：每个任务至少一个漏洞容器 + 一个 Kali 工作站容器，虚拟网络隔离。
- **交互**：agent 通过 SSH 登录 Kali，再执行任意 bash 命令；不限制工具集。
- **里程碑**：
  - 命令里程碑（command milestones）：完成某一步所需的命令序列；
  - 阶段里程碑（stage milestones）：把命令里程碑映射到渗透阶段。
- **判分**：最终 flag + 里程碑完成度；可区分「卡在发现」「卡在利用」「卡在提权」。
- **对照**：全自主 agent vs 半自主（允许人类介入）agent；GPT-4o / o1 / Gemini Flash。

## 方法学启示

- **里程碑是长链任务的部分分方案**：比 Cybench 的子任务更贴近渗透流程。
- **in-vitro 与 real-world 分开报告**：两者难度差巨大（27% vs 9%），混在一起会掩盖瓶颈。
- **人机协作基线不可省**：64% vs 21% 说明「全自主」分数低不一定是模型不会，可能是缺人类纠偏。
- **任意 bash 不等于任意能力**：接口开放，但仍受网络拓扑、凭证、工具可用性限制。

## 局限

- 33 个任务规模有限，real-world 仅 11 个。
- 里程碑由人工编写，可能偏向「标准解法」，低估替代路径。
- 任务多为传统服务/Web/CVE，不含二进制闭卷。
- 半自主设置引入人类，不能与全自主分数直接比较。
- 环境是简化企业网络，不等于真实内网。

## 可引用原句

> "The fully autonomous agent performs unsatisfactorily achieving a 21% Success Rate across the benchmark, solving 27% of the simple tasks and only one real-world task."

> "The assisted agent demonstrates substantial improvements, with 64% of SR."

## 相关页面

- [[autopenbench]]
- [[cybench]]
- [[cve-bench]]
- [[difficulty-levels]]
- [[benchmark-validity]]
- [[post-exploitation]]

## 来源

- `raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf`
- `raw/github/AutoPenBench_README.md`
