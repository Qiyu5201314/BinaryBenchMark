---
title: AutoPenBench
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf
  - raw/github/AutoPenBench_README.md
tags: [entity, benchmark, penetration-testing, milestones]
---

# AutoPenBench

## 一句话

面向生成式 Agent 的渗透测试基准：33 个任务（22 个 in-vitro + 11 个真实 CVE），用命令/阶段里程碑给出部分分；全自主 21%，半自主 64%。

**评测范式定位**：闭卷渗透（只给网络/目标与任务），目标不是二进制，而是服务与 Web/内网。

## 规模与设置

| 维度 | 数值 | 说明 |
|---|---|---|
| 任务 | 33 | 22 in-vitro + 11 real-world |
| in-vitro 类别 | 4 | Access Control、Web、Network、Crypto |
| real-world | 11 | Log4j2、Spring4Shell、SambaCry、Heartbleed 等 |
| 阶段 | 6 | Discovery → Infiltration → Vuln Detection → PrivEsc → Flag → Success |
| 交互 | 任意 bash | 通过 Kali 工作站 SSH |
| 判分 | flag + 里程碑 | 可分阶段归因 |

## 主要结果

| 设置 | 成功率 | 说明 |
|---|---|---|
| 全自主 | 21% | 27% in-vitro / 9% real-world |
| 半自主 | 64% | 59% in-vitro / 73% real-world |

## 为什么重要

- **里程碑部分分**：比「是否拿到 flag」更能定位瓶颈。
- **in-vitro / real-world 分开**：难度差距大，混报会误导。
- **人机协作基线**：64% vs 21% 说明全自主分数低可能来自缺人类纠偏。
- **任意 bash 接口**：接近真实渗透的工具自由度。

## 局限

- 规模有限（33 题），real-world 仅 11 个。
- 里程碑由人工编写，偏向标准解法。
- 环境是简化企业网络，不等于真实内网。
- 半自主引入人类，不能与全自主直接比较。
- 不含二进制闭卷。

## 对二进制闭卷的启示

- 里程碑设计可直接映射到八级任务谱系：理解 → 定位 → 可达 → 触发 → 归因 → 利用。
- 「全自主 vs 半自主」是重要的能力分层：二进制闭卷也应报告「有人类提示」与「完全无人」的差值。
- in-vitro / real-world 分层报告，可避免用合成题的高分掩盖真实场景的低分。

## 相关页面

- [[autopenbench]]
- [[cybench]]
- [[cve-bench]]
- [[difficulty-levels]]
- [[benchmark-validity]]
- [[post-exploitation]]

## 来源

- [[autopenbench-paper]]
- `raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf`；https://arxiv.org/abs/2410.03225
- `raw/github/AutoPenBench_README.md`；https://github.com/lucagioacchini/auto-pen-bench
