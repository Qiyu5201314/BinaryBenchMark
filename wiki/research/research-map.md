---
title: 研究地图：网络安全 Agent 评测
type: research
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/github/CAGE_docs/how-a-run-works.md
  - raw/official/BerkeleyRDI_Frontier_AI_Cybersecurity.txt
  - raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf
  - raw/papers/EnIGMA_arXiv_2409.16165v3.pdf
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
  - raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf
tags: [research-map, evaluation, methodology]
---

# 研究地图：网络安全 Agent 评测

## 五层框架

把「评测一个网络安全 Agent」拆成五层，每层都有独立的研究问题与设计选择。

```text
┌─────────────────────────────────────────────┐
│ 5. 度量（Measurement）                       │
│    成功率 / Pass@k / 成本 / 时间 / 方差       │
├─────────────────────────────────────────────┤
│ 4. 验证（Verification）                      │
│    sanitizer / marker / flag / LLM judge     │
├─────────────────────────────────────────────┤
│ 3. Agent（Harness + Model + Tools）          │
│    Codex / Claude Code / OpenHands / 自研     │
├─────────────────────────────────────────────┤
│ 2. 环境（Environment）                       │
│    源码 / 二进制 / Web / 内网 / 靶场          │
├─────────────────────────────────────────────┤
│ 1. 任务（Task）                              │
│    复现 / 发现 / 打点 / 后渗透 / 闭卷         │
└─────────────────────────────────────────────┘
```

来源：综合 [[cybergym-paper]]、[[agentcyberrange-paper]]、[[cage-docs]]。

## 每层的核心问题

### 1. 任务定义
- 测的是「复现已知漏洞」还是「发现未知漏洞」？
- 单点还是端到端？是否要求横向移动？
- 给多少信息？开卷/闭卷的哪个维度？
- 相关页面：[[open-book-vs-closed-book]]、[[vulnerability-reproduction]]、[[difficulty-levels]]

### 2. 环境
- 源码还是二进制？是否允许动态分析？
- Web / 内网 / 云 / 供应链？
- 真实性与隔离性如何平衡？
- 相关页面：[[cyber-range]]、[[evaluation-isolation]]、[[reproducibility]]

### 3. Agent
- 评测的是模型能力还是系统能力？
- harness、工具集、预算是否固定？
- 是否允许子 Agent、记忆、联网？
- 相关页面：[[agent-harness]]

### 4. 验证
- 确定性 verifier 还是 LLM judge？
- 是否只看最终成功，还是给部分进展？
- 是否抗作弊？是否会被 reward hacking？
- 相关页面：[[benchmark-validity]]

### 5. 度量
- Success / Pass@k / 成本 / 时间 / 方差 / 覆盖率
- 是否报告失败模式与轨迹？
- 相关页面：[[cost-aware-evaluation]]

## 现有工作的层次定位

| 工作 | 任务 | 环境 | Agent | 验证 | 度量 |
|---|---|---|---|---|---|
| [[cybergym]] | 漏洞复现 L0–L3 | C/C++ 源码+二进制 | 多 harness | sanitizer | 成功率、成本 |
| [[agentcyberrange]] | Web 打点 + 后渗透 | 真实 Web + 内网 | 多 harness | marker/verifier/judge | Pass@k、成本、时间 |
| [[cve-bench]] | 真实 Web CVE 利用 | Web 容器 | Cy/T/AutoGPT | 运行时证据 | success@k、成本 |
| [[aixcc]] | 发现 + PoV + 补丁 | 源码 + harness | CRS | PoV + 补丁 | 配对质量、成本 |
| [[cybench]] | 40 个专业 CTF | CTF 服务 | 自研 agent + 4 scaffolds | flag + 子任务 | unguided/subtask、FST |
| [[nyu-ctf-bench]] | 200 个 CTF | CTF 服务 | function calling | flag | 分类成功率、失败原因 |
| [[enigma]] | 390 个 CTF | CTF 服务 | EnIGMA + IAT | flag + 预算 | 成功率、泄漏、soliloquy |
| [[autopenbench]] | 33 个渗透任务 | 靶场 + 真实 CVE | 全自主/半自主 | flag + 里程碑 | SR、阶段完成 |
| [[sec-bench]] | PoC + 补丁 | 源码仓库 | SWE-agent/OpenHands/Aider | PoC + 金补丁 | 成功率、构建成本 |
| [[slyp]] | COM 竞态 + PoC | COTS 二进制 | ReAct + MCP 工具 | debugger PoC | F1、PoC 率、真实 0day |
| [[veritas]] | OOB 推理 | stripped binary | 三阶段 agent | 静态 witness + 运行时 | recall、FP、成本 |
| [[vulbinllm]] | CWE 检测 | stripped binary | LLM + memory | CWE 分类 | Acc/F1 |
| [[cage]] | 通用执行框架 | 可插拔 | 多 harness | 可插拔 scorer | 可插拔 |
| [[uk-aisi-inspect-cybergym]] | CyberGym 复现 | Docker Compose | Inspect solver | 同 CyberGym | 成功率 |

## 空白矩阵

| 组合 | 是否已有成熟基准 |
|---|---|
| 源码 + 漏洞描述 + 单点复现 | ✅ CyberGym |
| 源码 + 无描述 + 单点发现 | ✅ CyberGym L0（部分） |
| 源码 + 无漏洞信息 + 发现 + 补丁 | ✅ AIxCC / SEC-bench |
| Web + 无漏洞信息 + 单点利用 | ✅ CVE-Bench |
| Web + 无漏洞信息 + 端到端打点 | ✅ AgentCyberRange Web L0 |
| 内网 + 无拓扑 + 端到端后渗透 | ✅ AgentCyberRange Post L0 |
| 渗透 + 无漏洞信息 + 端到端 | ✅ AutoPenBench / AgentCyberRange |
| **二进制 + 无源码 + 无漏洞信息 + 端到端利用** | ⚠️ 仅领域专用（[[slyp]]：Windows COM 竞态） |
| 二进制 + 无源码 + 无漏洞信息 + 单点检测 | ⚠️ 有（[[vulbinllm]]、[[veritas]]），但只检测、不利用 |
| 跨架构（ARM/MIPS）+ 闭卷 | ❌ |
| 有防御方（主动响应）的闭卷 | ⚠️ AgentCyberRange 有 AV/监控，但不是对抗性防御 Agent |

> [推断] 2023–2026 的前沿把「二进制闭卷」从「完全空白」推进到「领域专用可用」：
> - [[vulbinllm]]、[[veritas]] 证明 stripped binary 上可以做检测/推理；
> - [[slyp]] 证明可以做端到端发现 + debugger-verified PoC；
> - 但还没有跨漏洞类型、跨架构、公开可复现的统一基准。
>
> 「二进制闭卷」不是简单地把源码换成二进制，而是同时改变：
> - 信息（无源码、无描述）
> - 分析难度（反编译、符号恢复、动态调试）
> - 验证（崩溃/控制流劫持/利用成功）
> - 环境（架构、libc、sanitizer 可用性）
> 这会带来新的构念效度与可复现性问题。

## 相关页面

- [[research/open-questions]]
- [[research/methodology-notes]]
- [[projects/binary-closed-book-eval]]
- [[comparisons/benchmark-landscape]]
- [[comparisons/binary-closed-book-landscape]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/github/CAGE_docs/how-a-run-works.md`
- `raw/official/BerkeleyRDI_Frontier_AI_Cybersecurity.txt`
- `raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf`
- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`
- `raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf`
- `raw/papers/EnIGMA_arXiv_2409.16165v3.pdf`
- `raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf`
- `raw/papers/SEC-bench_arXiv_2506.11791v1.pdf`
- `raw/papers/Slyp_arXiv_2605.05000v1.pdf`
- `raw/papers/Veritas_arXiv_2605.15097v3.pdf`
- `raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf`
