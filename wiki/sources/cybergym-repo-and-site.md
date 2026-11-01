---
title: 来源摘要：CyberGym 仓库、官网与提交规范
type: source
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/github/CyberGym_README.md
  - raw/github/CyberGym_FAQ.md
  - raw/github/CyberGym_SUBMISSION.md
  - raw/official/CyberGym_Benchmark_Page.txt
  - raw/official/BerkeleyRDI_CyberGym_Blog.txt
tags: [source, repository, leaderboard, submission]
---

# 来源摘要：CyberGym 仓库、官网与提交规范

## 元数据

| 文件 | 内容 | 原始 URL |
|---|---|---|
| `raw/github/CyberGym_README.md` | 安装、数据、评测服务、防火墙、示例 Agent | https://github.com/sunblaze-ucb/cybergym |
| `raw/github/CyberGym_FAQ.md` | 网络、版本、计分、部署、动态分析 | 同上 |
| `raw/github/CyberGym_SUBMISSION.md` | 榜单提交规范（2026-08-04） | 同上 |
| `raw/official/CyberGym_Benchmark_Page.txt` | 官方基准页、榜单、0day 统计 | https://www.cybergym.io/cybergym/ |
| `raw/official/BerkeleyRDI_CyberGym_Blog.txt` | Berkeley RDI 博客（2025-10-08） | https://rdi.berkeley.edu/blog/cybergym/ |

## 工程细节

### 数据与安装
- 需要 Python + Docker。
- 基准数据约 **240GB**；binary-only 约 **130GB**；完整 server data 约 **10TB**。
- 提供 10 个任务的 subset（5 易 5 难）用于 smoke test。
- 示例 Agent 以 submodule 形式提供。

### 部署安全
- **全部本地部署，不要暴露公网**。
- 提交服务会执行上传 PoC；默认 API key 是公开占位符。
- 绑定地址用 Agent 容器所在 Docker 网络网关，不要 `0.0.0.0`。
- 防火墙：`cybergym-internal` 隔离网络 + Squid 域名白名单；只过滤出站，不解决入站。

### 计分
- FAQ 明确：主榜要求 Agent 指定**唯一最终提交**，报告 final-submission 指标。
- any-of 会随模型变强而奖励暴力尝试。
- 成功 = pre-patch 崩溃 + post-patch 不崩溃。

### 动态分析
- 若给 Agent 可运行环境，必须声明，并在交付前移除泄漏源：`/src/**/.git` 与 `/tmp/poc`。
- 网络访问会引入 reward hacking；建议白名单代理 + 轨迹审计。

### 提交规范（2026-08-04）
- 背景：最强模型在无约束资源下接近饱和；效率与成功率同等重要。
- 必填：`agent_name`、`success_rate`、`link`、`category`、`models[]`（token、成本、时间、请求数）。
- 还需至少 10 个任务的轨迹/日志/PoC，以及所有实例的 `vul_exit_code` / `fix_exit_code`。
- 详细 writeup 需说明 agent scaffold、工具、是否联网、是否动态环境。

## 官方页面关键数字

- 1,507 instances / 188 projects。
- 榜单为 Level 1：给漏洞描述 + 未修补代码，生成 PoC。
- 34 0day / 18 不完整补丁。
- 759 PoC 触发 60 个项目崩溃；17 个不完整补丁（15 项目）；10 个 0day（969 天）。
- 431 个 OSS-Fuzz 项目 / 1,748 可执行文件；GPT-4.1 7 个、GPT-5 22 个 0day。
- 官网警告：运行随机、分数可能波动；描述可能歧义，小差距未必代表能力差异。

## 博客（2025-10-08）补充

- 早期口径：顶尖 Agent 单次约 30%，30 次约 67%。
- Claude-Sonnet-4.5 在 Anthropic System Card 中：单次 28.9%，30 次 66.7%。
- 早期外溢数字：35 0day / 17 不完整补丁。
- 提到 3 个 CVE、6 个已修补漏洞（截至博客发布）。

> 这些数字与论文最终版不完全一致，属于版本差异。引用时注明来源与日期。

## 相关页面

- [[cybergym]]
- [[uk-aisi-inspect-cybergym]]
- [[benchmark-validity]]
- [[evaluation-isolation]]
- [[cost-aware-evaluation]]

## 来源

- `raw/github/CyberGym_README.md`、`raw/github/CyberGym_FAQ.md`、`raw/github/CyberGym_SUBMISSION.md`
- `raw/official/CyberGym_Benchmark_Page.txt`
- `raw/official/BerkeleyRDI_CyberGym_Blog.txt`
