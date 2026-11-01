---
title: Agent Harness（智能体外壳）
type: concept
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/github/CAGE_README.md
  - raw/github/CAGE_docs/agent-cage-managed-CN.md
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/papers/EnIGMA_arXiv_2409.16165v3.pdf
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
tags: [agent-harness, scaffold, confound, evaluation]
---

# Agent Harness（智能体外壳）

## 定义

Harness 是包裹基础模型的「外壳」：负责系统提示、工具定义、命令执行、上下文管理、重试/压缩、子 Agent 编排等。模型能力 ≠ 系统能力；同一个模型换 harness，分数可能显著变化。

## 证据

### CyberGym
- 用同一 OpenHands 框架评测 11 个模型，Claude-Sonnet-4 最高 17.9%。
- 换成不同 Agent（OpenHands、Codex CLI、EnIGMA、Cybench agent）但同一 GPT-4.1，四者成功率相近；**成功并集 18.4%**，接近单个最好结果的两倍，说明能力互补、成功重叠小。
- 行为差异明显：OpenHands 更常用 Bash 命令链；CTF 专用 Agent 更常写 Python 脚本。
- 专用 SWE-bench 模型在 CyberGym 上 ≤2%，说明「修 bug」能力不能直接迁移到「漏洞复现」。

来源：[[cybergym-paper]]、[[cybergym-repo-and-site]]。

### AgentCyberRange
- 配对方式：Codex + GPT-5.5、Claude Code + Opus-4.7、Qwen Code + Qwen-3.7-Max、Kimi Code + Kimi-2.6；无原生 harness 的模型统一用 Claude Code。
- 统一攻击者环境（Kali 工具 + 使用说明）、统一 prompt、统一预算。
- 结果：GPT-5.5 + Codex 最好；专用渗透 Agent PentestGPT-V2 在难例上同样失败。

来源：[[agentcyberrange-paper]]。

### CAGE
- 明确把 harness 作为评测变量：`agent × model × sample × prompt level × pass@k` 矩阵。
- CAGE-managed 模式拦截每次 LLM 调用，保证同模型跨 harness 的可比性；serve 模式放弃轨迹，换取零集成。
- 这说明「评测一个 Agent」和「评测一个模型」是不同问题，提交时应注明类别（model-focused vs agent-focused）。

来源：[[cage-docs]]、[[cybergym-repo-and-site]]。

### Cybench

- 固定模型（GPT-4o、Claude 3.5 Sonnet），比较 4 种 scaffold：structured bash、action-only、pseudoterminal、web search。
- 结果：scaffold 的影响**因模型而异**；Claude 3.5 在 pseudoterminal + web search 下更好，GPT-4o 反而更差。
- 含义：不存在「通用最优 scaffold」；跨模型比较必须固定 scaffold，或报告每个模型的最佳 scaffold。

来源：[[cybench-paper]]。

### EnIGMA

- 给 agent 增加 **Interactive Agent Tools（IATs）**：gdb、服务器连接等交互式终端程序。
- 消融（Claude 3.5，四基准聚合）：完整 29.5% → 去掉 debugger/connect 27.4%（-2.1pp）→ 去掉 demonstrations 23.3%（-6.2pp）。
- 工具收益按类别不同：crypto/pwn/rev 提升，web 反而下降（缺少合适的 Web 交互工具）。
- 含义：**工具接口设计（ACI）本身是能力的一部分**，不是中立的「管道」。

来源：[[enigma-paper]]。

### Slyp

- 三个 MCP 工具接口：二进制探索、COM 检查、动态调试。
- 生产 coding agent（Codex / Claude Code）在默认工具下几乎无法验证 PoC；加全部二进制工具后仍有差距（Slyp 最多 +0.208 F1）。
- 含义：二进制闭卷里，「给不给 gdb / 反编译 / 符号恢复工具」必须作为独立难度维度，而不是默认配置。

来源：[[slyp-paper]]。

## 设计含义

- 报告分数时必须写清 **模型 + harness + 工具集 + 预算 + prompt 等级**。
- 比较模型能力时，应固定 harness；比较系统能力时，应允许各自最佳 harness。
- 不要假设「换更强的模型」一定提升；工具使用、上下文管理、拒答行为都可能成为瓶颈。
- 专用 Agent 不一定优于通用 Agent；瓶颈可能在信息搜集与链式规划，而非编排。

## 相关页面

- [[cybergym]]
- [[agentcyberrange]]
- [[cage]]
- [[cybench]]
- [[enigma]]
- [[slyp]]
- [[benchmark-validity]]
- [[cost-aware-evaluation]]
- [[binary-closed-book-landscape]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/github/CAGE_README.md`
- `raw/github/CAGE_docs/agent-cage-managed-CN.md`
- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`
- `raw/papers/EnIGMA_arXiv_2409.16165v3.pdf`
- `raw/papers/Slyp_arXiv_2605.05000v1.pdf`
