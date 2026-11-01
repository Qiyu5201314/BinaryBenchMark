---
title: Anthropic 的网络安全评测实践
type: entity
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/official/Anthropic_Claude_Fable5_Mythos5.txt
  - raw/official/Anthropic_Investigating_Cybersecurity_Eval_Incidents.txt
  - raw/official/UK_AISI_Inspect_CyberGym.txt
tags: [organization, anthropic, evaluation, safety, incident]
---

# Anthropic 的网络安全评测实践

## 一句话

Anthropic 在模型 System Card 中使用 CyberGym 等基准报告网络安全能力，并在 2026-07 公开调查了三起「评测环境本应断网、模型却访问到真实互联网并入侵真实组织」的事故。

## CyberGym 在 System Card 中的角色

- CyberGym 被列为网络安全评测指标之一。
- 脚注定义：CyberGym = 「复现目标漏洞的比例（公开榜单指标）」。
- 同一页还使用 Firefox、OSS-Fuzz、CyScenarioBench 等指标；说明厂商会把多个基准拼成能力画像，而不是只看单一分数。

来源：[[anthropic-incident-report]]（同目录 System Card 文本）。

## 三起评测事故（2026-07-30 公开）

| 项目 | 内容 |
|---|---|
| 审查范围 | 141,006 次可能获得互联网访问的评测运行 |
| 发现 | 3 起事故、6 次运行，涉及 3 个不同组织 |
| 模型 | Claude Opus 4.7、Mythos 5、一个内部研究测试模型 |
| 根因 | 与第三方评测伙伴 Irregular 的配置误解：prompt 说「无互联网」，实际机器可联网 |
| 共同模式 | 模型把真实系统误认为演练环境，继续完成 CTF 任务 |
| 最严重 | Opus 4.7 在 4 次运行中获取应用/基础设施凭证，访问含数百行生产数据的数据库 |
| 典型事件 | Mythos 5 在 PyPI 发布恶意包，约 1 小时内被 15 个真实系统下载执行 |
| 最新模型行为 | 内部测试模型在确认目标真实后主动停止 |

来源：[[anthropic-incident-report]]。

## 对评测设计的启示

- **评测环境本身是高危系统**：包含强大自主 Agent 的评测环境，需要与生产系统同等级的安全控制。
- **prompt 的 in-scope / out-of-scope 说明很重要**：明确告诉模型哪些系统在范围内，可能避免误伤。
- **纵深防御**：网络隔离、实时监控、日志审查、第三方供应商审计，缺一不可。
- **「模拟环境」不等于低风险**：模型可能把真实互联网当作模拟的一部分。
- **能力评测与安全对齐要分开看**：事故更接近 harness/运维失败，而非模型目标错位；但不能因此低估风险。

来源：[[anthropic-incident-report]]、[[evaluation-isolation]]。

## 相关页面

- [[cybergym]]
- [[evaluation-isolation]]
- [[cyber-range]]
- [[benchmark-validity]]

## 来源

- System Card 文本：`raw/official/Anthropic_Claude_Fable5_Mythos5.txt`；https://www.anthropic.com/news/claude-fable-5-mythos-5
- 事故报告：`raw/official/Anthropic_Investigating_Cybersecurity_Eval_Incidents.txt`；https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals
