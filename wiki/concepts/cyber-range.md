---
title: 网络靶场（Cyber Range）
type: concept
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/github/PostExploitBench_README.md
  - raw/github/CAGE_docs/how-a-run-works.md
  - raw/official/Anthropic_Investigating_Cybersecurity_Eval_Incidents.txt
tags: [cyber-range, environment, realism, isolation]
---

# 网络靶场（Cyber Range）

## 定义

一套隔离的、可复现的虚拟网络环境，包含入口服务、内部主机、网络分段、支撑服务与诱饵，用于模拟真实企业网络，让 Agent 在其中执行攻击或防御任务。

在 [[agentcyberrange]] 中，8 套企业式内网就是 cyber range；在 [[cage]] 中，每个 trial 都会启动独立的靶标栈。

## 组成

| 组件 | 作用 |
|---|---|
| 入口服务 | 暴露给 Agent 的第一个攻击面（Web 应用、VPN、API） |
| 内部网络 | 多网段，限制可达性，迫使跳板 |
| 跳板机 / pivot host | 连接不同网段的节点 |
| 漏洞主机 | 可被利用的目标 |
| 支撑服务 | 数据库、Wiki、CI/CD、AI 应用等现代基础设施 |
| 诱饵（decoy） | 无利用价值的主机/服务，测试优先级判断 |
| 防御组件 | AV/EDR、监控运营者，测试免杀与隐蔽 |
| 验证器 | marker、verifier、日志，用于自动判分 |

来源：[[agentcyberrange-paper]]、[[postexploitbench]]。

## 核心张力：真实 vs 隔离 vs 可复现

- **越真实**：越能观察端到端能力，但环境越复杂、越难复现、越难隔离。
- **越隔离**：越安全，但可能降低生态效度（Agent 行为与真实网络不同）。
- **越可复现**：越适合科学比较，但可能牺牲动态性。

Anthropic 的三起事故说明：**「模拟环境」不等于低风险**。如果网络隔离配置错误，Agent 可能把真实互联网当作演练的一部分，入侵真实组织。详见 [[evaluation-isolation]]。

## 工程要求

- 每个 trial 独立靶标栈、独立网络，避免并行污染。
- 靶标可重置、可销毁、可重建。
- Agent 容器不能挂 docker socket；不能让宿主机进程直接访问靶标。
- 需要实时监控与日志，便于事后审计。
- 靶标构建与评测分离：`cage run` 不建靶，需先 `cage benchmark build`。

来源：[[cage-docs]]、[[evaluation-isolation]]。

## 相关页面

- [[agentcyberrange]]
- [[postexploitbench]]
- [[post-exploitation]]
- [[evaluation-isolation]]
- [[reproducibility]]

## 来源

- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/github/PostExploitBench_README.md`
- `raw/github/CAGE_docs/how-a-run-works.md`
- `raw/official/Anthropic_Investigating_Cybersecurity_Eval_Incidents.txt`
