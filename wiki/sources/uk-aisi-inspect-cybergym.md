---
title: 来源摘要：UK AISI Inspect Evals 的 CyberGym 复现
type: source
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/official/UK_AISI_Inspect_CyberGym.txt
tags: [source, reproduction, inspect-evals, uk-aisi]
---

# 来源摘要：UK AISI Inspect Evals 的 CyberGym 复现

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | CyberGym: Evaluating AI Agents' Real-World Cybersecurity Capabilities at Scale – Inspect Evals |
| 维护 | UK AI Safety Institute（AISI）Inspect Evals |
| 版本 | [3-B] 2026-08-03；[3-A] 2026-06-29；[2-A] 2026-04-03 |
| 本地文件 | `raw/official/UK_AISI_Inspect_CyberGym.txt` |
| URL | https://ukgovernmentbeis.github.io/inspect_evals/evals/cybergym/index.html |

## 为什么重要

这是 CyberGym 的**第三方独立复现**，暴露了很多论文不会写的工程细节与坑。

## 沙箱架构

Docker Compose 四个容器：

| 容器 | 作用 |
|---|---|
| `default` | solver 工作区 |
| `cybergym-vulnerable` | 未修补程序 |
| `cybergym-fixed` | 修补后程序 |
| `cybergym` | 路由 solver/scorer 的提交 |

另有两个网络：`cg_public`（solver 可见）、`cg_private`（vuln/fixed 内部）。

## 任务与样本

- Sample ID 格式：`cybergym_{dataset}_{task_num} ({variant})`，如 `cybergym_arvo_47101 (level1)`。
- `dataset` 为 `arvo` 或 `oss_fuzz`。
- L0/L1 最难（信息最少），L3 最易（信息最多）。
- 需要把 CyberGym 数据目录软链到 `src/inspect_evals/cybergym/tasks`，否则从 Hugging Face 下载（完整 236GB）。
- 用 `--message-limit 202` 对齐论文的 100 次 agent iteration。

## 结果

| 模型 | Reproduced | New Vuln | 时间 | 样本 |
|---|---|---|---|---|
| gpt-4.1-2025-04-14 | 11.61% | 3.12% | 11h 3m 26s | 1507/1507（level1） |

- 论文报告 gpt-4.1 在 level1 的成功率是 9.4%（最好 agent）。
- 复现实验的 solver 沙箱**无互联网**，但预装了部分安全工具。
- 结果差异可能来自 harness、工具、消息限制、镜像版本。

## 版本变更（可复现性教训）

### [2-A] 2026-04-03
- 修复「测试了错误 PoC」的 bug；**此前版本结果全部无效**。

### [3-A] 2026-06-29
- 处理超大 executor 输出（>10MB）导致 JSON 截断/解析失败的问题。
- 无法解析的 executor 响应现在会抛错，而不是算作「未复现」。
- 结果可能与 2-A 不同。

### [3-B] 2026-08-03
- 把沙箱镜像引用固定为不可变 tag/digest，防止 registry push 静默改变评测环境。

## 启示

- 复现一个基准不只是「跑一下」；镜像、消息限制、工具、网络、验证器都会影响结果。
- **验证器 bug 会让整批结果作废**，必须版本化 + 回归测试。
- 第三方复现是检验基准鲁棒性的重要手段。

## 相关页面

- [[cybergym]]
- [[reproducibility]]
- [[benchmark-validity]]
- [[agent-harness]]

## 来源

- `raw/official/UK_AISI_Inspect_CyberGym.txt`
- https://ukgovernmentbeis.github.io/inspect_evals/evals/cybergym/index.html
