---
title: 来源摘要：CAGE 文档
type: source
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/github/CAGE_README.md
  - raw/github/CAGE_docs/index.md
  - raw/github/CAGE_docs/how-a-run-works.md
  - raw/github/CAGE_docs/datasets.md
  - raw/github/CAGE_docs/agent-cage-managed-CN.md
  - raw/github/CAGE_docs/agent-serve-mode-CN.md
  - raw/github/CAGE_docs/training-corpus.md
  - raw/github/CAGE_docs/cli-design.md
  - raw/github/CAGE_docs/deployment.md
  - raw/github/CAGE_docs/targets-check.md
tags: [source, documentation, cage]
---

# 来源摘要：CAGE 文档

## 元数据

| 项 | 内容 |
|---|---|
| 项目 | CAGE (Cybersecurity Agent Gym & Evaluation) |
| 仓库 | https://github.com/AgentCyberRange/CAGE |
| 本地文件 | `raw/github/CAGE_README.md`、`raw/github/CAGE_docs/*.md` |
| 定位 | 评测执行基础设施，不是 benchmark，不是 agent |

## 核心概念

### run equation
```text
cage run = Framework ( Benchmark , Config )
```
- Layer 1 `cage/`：容器、代理、靶标、判分、快照、续跑。
- Layer 2 `examples/<name>/`：samples、prompts、targets、scorer。
- Layer 3 用户：实验 YAML + CLI 参数。
- 框架不出现任何 benchmark 名字。

### 两种模式
- **serve / benchmark-only**：外部 Agent 通过 HTTP API / SDK 驱动 `list → launch → attack → submit → close`；无轨迹、零集成。
- **CAGE-managed**：CAGE 运行 Agent，容器内代理拦截每次 LLM 调用，采集完整轨迹。

### trial 生命周期
SETUP → EXECUTE → SCORE → CLEANUP，共 19 步；benchmark 只负责其中少数步骤（prepare_trial、build_prompt、on_agent_finish、Scorer.gather、on_trial_complete、Scorer.score）。

### 关键工程点
- 容器内代理记录 `proxy.jsonl`，是事实来源。
- 每个 trial 独立靶标栈 + 独立 Agent 容器。
- 前后状态快照；`.cage_runs` 是审计证据。
- 判分：同一 scorer 三处复用（实时监控、trial 结束、离线 `cage score`）。
- 终止原因只由结构性信号判定；`--resume` 安全重放。
- 产物：`meta.json`、`prompt.txt`、`task_output.json`、`proxy/proxy.jsonl`、`scores/*.json`、`state_pre/post`、`runtime/live_success.json`。

### 数据集
- CAGE 不带数据；每个 benchmark 自己带 dataset。
- 通过 git submodule 拉工具 + 子集；Hugging Face 拉完整集。
- WebExploitBench 15 apps / 110 vulns；PostExploitBench 8 ranges / 156 hosts。

### 训练语料
- `cage corpus export` 导出 ms-swift `messages + loss` 格式。
- 一行一条完整对话；保留推理与工具调用；路径脱敏保留容器路径。
- 只拒绝 benchmark 定义的污染门；其他过滤默认关闭。

### 自带 benchmark
AgentPentestBench、CVEBench、NYU CTF、Cybench、AutoPenBench、HackWorld、StrongREJECT。

## 可引用原句

> "CAGE is infrastructure — not a benchmark, not an agent."

> "The framework holds zero benchmark names."

> "proxy.jsonl — not the agent's stdout — is the source of truth for what the model was asked and what it answered."

## 相关页面

- [[cage]]
- [[agentcyberrange]]
- [[agent-harness]]
- [[reproducibility]]
- [[evaluation-isolation]]

## 来源

- `raw/github/CAGE_README.md`
- `raw/github/CAGE_docs/index.md`
- `raw/github/CAGE_docs/how-a-run-works.md`
- `raw/github/CAGE_docs/datasets.md`
- `raw/github/CAGE_docs/agent-cage-managed-CN.md`
- `raw/github/CAGE_docs/agent-serve-mode-CN.md`
- `raw/github/CAGE_docs/training-corpus.md`
