---
title: 可复现性（Reproducibility）
type: concept
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/github/CyberGym_README.md
  - raw/official/UK_AISI_Inspect_CyberGym.txt
  - raw/github/CAGE_docs/how-a-run-works.md
  - raw/github/CAGE_docs/datasets.md
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
tags: [reproducibility, versioning, docker, dataset]
---

# 可复现性（Reproducibility）

## 一句话

同一个基准、同一个模型、同一个 harness，换一台机器、换一个时间，能否得到可比较的结果？网络安全 Agent 评测的可复现性比普通 NLP 基准更难，因为涉及 Docker、网络、靶标、模型端点、随机性。

## 主要威胁

| 威胁 | 例子 | 缓解 |
|---|---|---|
| 数据集版本漂移 | Hugging Face 数据集更新、gated | 固定 commit / 版本号；记录下载日期 |
| 镜像漂移 | Docker 镜像 tag 被覆盖 | 固定 digest（UK AISI 3-B 明确做了） |
| 靶标状态漂移 | 容器未重置、残留 marker | 每 trial 独立栈 + pre/post 快照 |
| 模型端点漂移 | 模型名指向不同后端 | 记录 `served_model`，不只记录标签 |
| 随机性 | Agent 采样、运行间方差 | 多次运行 + Pass@k + 种子 |
| 验证器 bug | UK AISI 曾测错 PoC，旧结果作废 | 版本化 verifier + 回归测试 |
| 环境配置 | 网络是否可达、工具是否预装 | 写清网络/工具/预算，固定镜像 |

来源：[[cybergym-repo-and-site]]、[[uk-aisi-inspect-cybergym]]、[[cage-docs]]。

## CyberGym 的可复现设计

- 数据 ~240GB；仅静态分析可下 ~130GB binary-only；完整 server data ~10TB。
- 提供 subset（10 个任务，5 易 5 难）便于 smoke test。
- 任务生成、提交、验证都有独立脚本。
- 但不同团队自行提交、运行随机，排行榜分数不可直接跨设置比较。

来源：[[cybergym-repo-and-site]]、[[cybergym]]。

## CAGE 的可复现设计

- 每个 trial 独立容器 + 独立靶标 + 独立网络。
- 前后状态快照；`.cage_runs` 是事实来源，Docker 资源可丢弃。
- 终止原因由结构性信号判定，`--resume` 可重放已完成结果。
- 产物包含 `proxy.jsonl`、`scores/*.json`、`state_pre/post`，可事后审计。
- 训练语料导出保留 `served_model` 与 trial 溯源。

来源：[[cage-docs]]。

## UK AISI 的教训

- Inspect Evals 的 CyberGym 实现版本化：`[2-A] 2026-04-03`、`[3-A] 2026-06-29`、`[3-B] 2026-08-03`。
- `[2-A]` 修复「测试了错误 PoC」的 bug，并明确「此前版本结果无效」。
- `[3-A]` 处理超大输出导致的 JSON 解析失败，并明确「结果可能与 2-A 不同」。
- `[3-B]` 把沙箱镜像固定到不可变 digest，防止 registry push 静默改变环境。
- 这展示了「验证器/环境本身也要版本化」的最佳实践。

来源：[[uk-aisi-inspect-cybergym]]。

## 复现检查清单

- [ ] 数据集版本/commit/下载日期已记录
- [ ] Docker 镜像 digest 已固定
- [ ] 模型 endpoint 与 served model 已记录
- [ ] prompt 模板、工具集、预算已公开
- [ ] 是否联网、是否动态分析已声明
- [ ] 每个 trial 独立环境、可重置
- [ ] 验证器版本与回归测试已记录
- [ ] 多次运行 + 方差 + Pass@k
- [ ] 原始轨迹/日志/ PoC 已保留

## 相关页面

- [[benchmark-validity]]
- [[cage]]
- [[cybergym]]
- [[uk-aisi-inspect-cybergym]]
- [[research/methodology-notes]]

## 来源

- `raw/github/CyberGym_README.md`
- `raw/official/UK_AISI_Inspect_CyberGym.txt`
- `raw/github/CAGE_docs/how-a-run-works.md`
- `raw/github/CAGE_docs/datasets.md`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
