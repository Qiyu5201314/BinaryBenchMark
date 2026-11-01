---
title: 成本感知评测（Cost-Aware Evaluation）
type: concept
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/github/CyberGym_SUBMISSION.md
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/papers/related/Cost_Aware_Security_Agents_arXiv_2607.15263.pdf
tags: [cost, efficiency, metrics, evaluation]
---

# 成本感知评测（Cost-Aware Evaluation）

## 为什么只看成功率不够

- 安全评测常给宽松预算，测「峰值能力」；但真实运营中，每一步推理、每次工具调用、每次遥测查询都有成本。
- 只报告成功率会奖励「无限预算刷分」，无法回答「这个模型在可接受成本下是否实用」。
- CyberGym 提交指南（2026-08-04）明确：最强模型在无约束资源下已接近饱和，**效率现在与成功率同等重要**。

来源：[[cybergym-repo-and-site]]、[[related-work]]。

## CyberGym 要求的成本字段

提交时必须报告（按模型）：

| 字段 | 含义 |
|---|---|
| `input_tokens` | 每任务平均非缓存输入 token |
| `cache_read_tokens` | 平均缓存读取 token |
| `cache_creation_tokens` | 平均缓存写入 token |
| `output_tokens` | 平均输出 token |
| `est_usd_cost` | 平均估计 USD 成本（可选，本地/未定价填 null） |
| `time_cost_sec` | 平均墙钟时间 |
| `llm_requests` | 平均模型请求次数 |

并要求提交至少 10 个任务的轨迹、日志、PoC，以及所有实例的 `vul_exit_code` / `fix_exit_code`。

来源：[[cybergym-repo-and-site]]。

## AgentCyberRange 的成本报告

论文表格同时给出 **Cost (M)**（百万 token？按表中 14.84/12.90 的量级，更像百万 token 或某种成本单位）与 **Time (min)**。引用时需回原文确认单位，不要臆测。

> `status: needs-review` — 表格列名是 `Cost (M)`，具体单位需查论文正文/附录确认。

来源：[[agentcyberrange-paper]]。

## 成本感知研究的核心发现

《Beyond Success Rate: Cost-Aware Evaluation of Offensive and Defensive Security Agents》：

- 在固定成本水平比较模型，而不是只报 best-case。
- 把性能分解为 **inference spend** 与 **tool spend**。
- **攻防 scaling 规律不同**：
  - 攻击型 CTF：增加 test-time compute 通常有效；开源权重模型在成本上有竞争力。
  - 防御型 SOC 调查：不完全随推理预算 scaling；更依赖有纪律的工具使用、遥测导航、选择性 enrichment。
- 结论：安全 Agent 基准应同时测经济效率与运营适配性。

来源：[[related-work]]。

## 建议报告的最小集合

- Success rate + Pass@k + 运行次数。
- 每任务 token（分 input/cache/output）。
- 每任务 USD 成本或本地算力成本。
- 每任务墙钟时间。
- 工具调用次数 / 类型分布。
- 成本-成功率曲线（cost-success frontier），而不是单点。

## 相关页面

- [[benchmark-validity]]
- [[cybergym]]
- [[agentcyberrange]]
- [[research/methodology-notes]]

## 来源

- `raw/github/CyberGym_SUBMISSION.md`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/papers/related/Cost_Aware_Security_Agents_arXiv_2607.15263.pdf`
