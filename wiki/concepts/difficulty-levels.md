---
title: 难度分级与提示等级
type: concept
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/github/CAGE_README.md
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf
  - raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf
  - raw/notes/二进制闭卷测试指南.md
tags: [difficulty, information-asymmetry, prompt-level, pass-at-k]
---

# 难度分级与提示等级

## 核心思想

同一个任务，通过控制**给 Agent 多少信息**来制造难度梯度。这样不仅能测「能不能做」，还能诊断「卡在哪一步」。

## CyberGym：L0–L3

| 等级 | 额外信息 | 诊断目的 |
|---|---|---|
| L0 | 无描述，只给代码 | 开放式发现能力 |
| L1 | 漏洞文字描述 | 主任务：报告 → PoC |
| L2 | + 崩溃栈 | 能否利用位置信息 |
| L3 | + 补丁 diff / post-patch 代码 | patch-to-exploit（one-day） |

关键结果：L0 仅 3.5%；L2/L3 显著高于 L1；说明信息量是强变量。

来源：[[cybergym-paper]]。

## AgentCyberRange：两条赛道各自的 L0–L2

| 等级 | Web | 内网 |
|---|---|---|
| L0 | 只给 URL | 只给入口 IP |
| L1 | + 哪些 URL 有漏洞 | + 内网拓扑 |
| L2 | + 漏洞类型 | + 具体 CVE/弱点/凭证位置 |

关键结果：
- Web：L0→L1 提升最大（入口发现是瓶颈），L1→L2 提升较小且不一致。
- 内网：L1 拓扑提示收益有限，L2 具体提示收益更明显（最高 46.34%）。

来源：[[agentcyberrange-paper]]。

## CAGE 的工程实现

- prompt level 是 trial 的启动参数：`--prompt-level l0|l1|l2`。
- serve 模式下，提示档位在 `launch` 时绑定到具体实例，不同实例可跑不同档位而无需重启。
- 档位由发起 launch 的一方决定；严格公平评测应统一由运维方驱动 launch，或固定默认值。

来源：[[cage-docs]]。

## Cybench：FST 作为难度锚

- 用人类队伍首次解出时间（First Solve Time, FST）标注任务难度，范围 2 分钟–24 小时 54 分。
- 无子任务提示时，FST > 11 分钟的任务基本解不出；FST ≤ 11 分钟的多数尝试成功。
- 子任务提供部分分，但子任务由作者编写，可能偏向标准解法。

来源：[[cybench-paper]]。

## CVE-Bench：zero-day / one-day

- zero-day：只给目标 URL 与一般目标；one-day：额外给漏洞描述。
- 结果：10% vs 12.5–13%（success@5），直接估计「给描述」的信息增益。
- 与 CyberGym L0/L1 的思路一致，但目标是真实 Web 应用。

来源：[[cve-bench-paper]]。

## AutoPenBench：阶段里程碑

- 6 个阶段：Target Discovery → Infiltration → Vulnerability Detection → Privilege Escalation → Flag Capturing → Success。
- 每个任务有命令里程碑与阶段里程碑；可区分「卡在发现」与「卡在利用/提权」。
- in-vitro 27% vs real-world 9%，说明难度分层必须分开报告。

来源：[[autopenbench-paper]]。

## 二进制闭卷：六个正交维度

| 维度 | 档位 |
|---|---|
| 信息 | 有符号 → stripped → 无任何提示 |
| 编译 | O0 → O2/O3/LTO → 静态链接/多编译器 |
| 架构 | x86-64 → ARM64 → MIPS/RTOS |
| 规模 | 单函数 → 单二进制 → 多二进制 → 固件 |
| 防护 | 无 → NX/Canary/PIE → CET/CFG/ASLR |
| 对抗 | 无 → 反调试/加壳 → 蜜罐/监控 |

**不要算一个难度总分**：用正交矩阵做消融。最有价值的配对实验是「给符号 vs 不给符号」，差值就是**逆向税**，可分离「不会逆向」与「不会漏洞推理」。

来源：[[binary-closed-book-guide]]。

## 指标：Pass@k

- **Pass@1**：单次尝试成功率。
- **Pass@k (Avg.)**：k 次独立尝试的平均成功率。
- **Pass@k (Max)**：k 次中任意一次成功即算解决。
- 三者的差距反映**运行间方差**；差距越大，说明 Agent 越不稳定。

AgentCyberRange 同时报告三者；CyberGym 主榜要求 final-submission，避免 any-of 指标奖励暴力尝试。

来源：[[agentcyberrange-paper]]、[[cybergym-repo-and-site]]。

## 设计陷阱

- **跨基准比较等级无意义**：CyberGym L2 与 AgentCyberRange L2 的信息量完全不同。
- **等级不是线性难度**：增加信息不一定单调提升，尤其内网 L1 拓扑提示可能帮助有限。
- **提示会改变任务性质**：L2 给漏洞类型后，任务更接近「已知漏洞利用」，不再是「发现」。
- **必须报告等级**：不写等级的分数不可比。

来源：[[agentcyberrange-paper]]、[[research/methodology-notes]]。

## 相关页面

- [[open-book-vs-closed-book]]
- [[vulnerability-reproduction]]
- [[post-exploitation]]
- [[cybench]]
- [[cve-bench]]
- [[autopenbench]]
- [[binary-closed-book-guide]]
- [[binary-closed-book-landscape]]
- [[benchmark-validity]]
- [[research/methodology-notes]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/github/CAGE_README.md`
- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`
- `raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf`
- `raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf`
- `raw/notes/二进制闭卷测试指南.md`
