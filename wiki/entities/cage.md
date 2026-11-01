---
title: CAGE
type: entity
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/github/CAGE_README.md
  - raw/github/CAGE_docs/index.md
  - raw/github/CAGE_docs/how-a-run-works.md
  - raw/github/CAGE_docs/agent-cage-managed-CN.md
  - raw/github/CAGE_docs/agent-serve-mode-CN.md
  - raw/github/CAGE_docs/datasets.md
  - raw/github/CAGE_docs/training-corpus.md
tags: [infrastructure, evaluation-harness, docker, agent-evaluation]
---

# CAGE（Cybersecurity Agent Gym & Evaluation）

## 一句话

不是基准，也不是 Agent，而是**评测执行基础设施**：把已安装的 AI 编码 Agent（Codex、Claude Code、Qwen、Kimi、自研 Agent）放进独立 Docker 容器，对可插拔 benchmark 跑 trial，拦截每一次 LLM 调用，做前后状态快照并自动判分。

来源：[[cage-docs]]。

## 核心设计：一个等式

```text
cage run = Framework ( Benchmark , Config )
           └ Layer 1 ┘ └ Layer 2 ┘ └ Layer 3 ┘
           运行机制       评什么        怎么跑
```

- **Layer 1 `cage/`**：容器、代理、靶标、判分、快照、续跑；**不知道任何 benchmark 名字**。
- **Layer 2 `examples/<name>/`**：samples、prompts、targets、scorer。
- **Layer 3 你**：实验 YAML + CLI 参数。

这条「框架零 benchmark 名」的不变量，是它能同时服务 Web 渗透、CTF、CVE 复现、红队任务的原因。

## 两种接入模式

| 维度 | Benchmark-only / serve（推荐） | CAGE-managed |
|---|---|---|
| 谁运行 Agent | 外部进程 | CAGE `cage run` |
| 集成成本 | 几乎为零（HTTP API / SDK） | Dockerfile + `agent.yml` + proxy 约定 |
| 轨迹（逐次 LLM/工具调用） | **不采集**，只有最终判定 | **全量采集** |
| 快照 / 断点续跑 | 不支持 | 支持 |
| 跨 Agent 可比性 | 较弱 | 强（运行时标准化） |
| 适合 | 成熟 Agent / 黑盒 Agent / 自助打榜 | 终端型 Agent（Codex、Claude Code） |

**判断口诀**：Agent 已有成熟日志和 UI → serve；想让 CAGE 替你记录轨迹 → CAGE-managed。

来源：[[cage-docs]]。

## 一次 trial 的生命周期

1. **SETUP**：启动靶标栈 → Agent 容器接入靶标网络 → 快照 pre-state → 重置 workspace → benchmark 准备文件 → 注入靶标地址 → 启动提交服务。
2. **EXECUTE**：渲染 prompt → 启动容器内 LLM 代理 → 启动实时成功监控 → 运行 Agent CLI → 停止代理并收集日志。
3. **SCORE**：收集 Agent 输出 → 从靶标收集实时证据 → 靶标 post-mortem → 写 `.traj` → 快照 post-state → scorer 判分。
4. **CLEANUP**：拆提交服务、网络、靶标栈。

四个阶段对应公平评测的四件事：干净可复现的环境、被观测的运行、基于证据的判定、不泄漏资源。来源：[[cage-docs]]。

## 关键机制

### 容器内 LLM 代理
- 代理运行在 Agent 容器内，Agent 的模型端点指向它。
- 记录每一次请求/响应到 `proxy/proxy.jsonl`，这是**事实来源**，比 Agent stdout 更可信。
- 可翻译 Anthropic ↔ OpenAI 协议、重建 tool calls、执行轮次/token/成本预算。

### 靶标与网络
- 每个 trial 独立靶标栈与独立 Agent 容器；并行 trial 不共享 workspace 或靶标。
- 两种暴露方式：alias（按 hostname 访问）或 network（在子网内发现主机）。
- serve 模式强调：不要让宿主机直接访问靶标，不要给 Agent docker socket；每个并发任务用独立 Agent 容器，避免网络串扰。

### 判分
- benchmark 提供 `Scorer`；同一 scorer 在三个位置复用：trial 内实时监控、trial 结束判分、离线 `cage score`。
- 判分优先信任 trial 期间写入的 **live-success verdict**，而不是事后从文本里解析。
- 信号有两类：确定性 verifier（靶标侧/输出检查）与 LLM judge。

### 终止与续跑
- `termination_reason` 只由**结构性信号**判定（退出码、代理 HTTP 状态、预算计数），不扫描 Agent 自然语言。
- `--resume` 重放已完成结果，只重跑你指定的基础设施失败；预算耗尽类终止默认不重跑。

### 产物
```text
.cage_runs/<agent>:<model>:<lifecycle>/<run_id>/
  dashboard.json
  results.csv
  trials/<sample>/
    meta.json
    prompt.txt
    task_output.json
    proxy/proxy.jsonl
    scores/<benchmark>.json
    state_pre/ state_post/
    runtime/live_success.json
```

来源：[[cage-docs]]。

## 训练语料导出

`cage corpus` 把完成的 run 转成训练数据（ms-swift `messages + loss` 格式）：

```bash
cage corpus runs
cage corpus export --run <run-id> --out sft.jsonl
```

- 一行 = 一条完整对话，不是逐轮样本。
- 只拒绝 benchmark 定义的「污染/不可学习」数据；其余过滤默认关闭。
- 每条 row 可回溯到 `.cage_runs` 的具体 trial。
- 路径脱敏保留容器路径 `/home/agent/workspace`，只脱敏宿主机路径。

这部分对「用评测轨迹生产训练数据」的研究很关键。来源：[[cage-docs]]。

## 自带 benchmark

CAGE 本身不带数据，以下为示例包：

| Benchmark | 评什么 |
|---|---|
| AgentPentestBench | [[webexploitbench]] + [[postexploitbench]] |
| CVEBench | 在 live target 上利用已知 CVE |
| NYU CTF | CTF 任务 |
| Cybench | 40 个 CTF 任务，23 个有 live service |
| AutoPenBench | 自动化渗透测试 |
| HackWorld | Web CTF |
| StrongREJECT | 安全/拒答行为（无 live target） |

来源：[[cage-docs]]。

## 相关页面

- [[agentcyberrange]]：CAGE 服务的基准项目
- [[agent-harness]]
- [[evaluation-isolation]]
- [[reproducibility]]
- [[benchmark-validity]]

## 来源

- README：`raw/github/CAGE_README.md`；https://github.com/AgentCyberRange/CAGE
- 文档：`raw/github/CAGE_docs/index.md`、`how-a-run-works.md`、`agent-cage-managed-CN.md`、`agent-serve-mode-CN.md`、`datasets.md`、`training-corpus.md`
