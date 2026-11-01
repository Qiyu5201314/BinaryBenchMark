---
title: 来源摘要：AgentCyberRange 论文（arXiv v2）
type: source
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
tags: [source, paper, agentcyberrange]
---

# 来源摘要：AgentCyberRange 论文

## 元数据

| 项 | 内容 |
|---|---|
| 标题 | AgentCyberRange: Benchmarking Frontier AI Systems in Realistic Cyber Ranges |
| 作者 | Fengyu Liu, Jiarun Dai, Yihe Fan 等（复旦大学等） |
| 版本 | arXiv 2606.14295v2（2026-06-16） |
| 原文 | `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf` |
| URL | https://arxiv.org/abs/2606.14295 |

## 核心主张

1. 现有公开基准把真实入侵拆成孤立技能（CTF、漏洞复现、exploit 生成），丢失了「发现暴露服务 → 初始立足 → 信息搜集 → 跨主机扩展」的端到端结构。
2. 因此需要开放、可复现、多主机的 cyber range 评测基础设施。
3. AgentCyberRange = 110 漏洞 / 15 Web 应用 + 8 内网 / 156 主机 + CAGE 执行框架。
4. 六套前沿系统在统一 prompt/预算下：GPT-5.5 + Codex 最好，Web 16.1%、内网 31.7%；给提示后 33.0% / 46.3%。

## 方法

### 两条赛道
- **WebExploitBench**：15 个真实 Web 应用、110 漏洞、17 类；0day + 1day + synthetic；黑盒可利用原则。
- **PostExploitBench**：8 套企业式内网、156 主机；多层拓扑、跳板、诱饵、AV/EDR、现代基础设施（Wiki、CI/CD、AI 应用）。

### 难度
- Web：L0 只给 URL；L1 给漏洞 URL；L2 给漏洞类型。
- 内网：L0 只给入口 IP；L1 给拓扑；L2 给 CVE/弱点/凭证位置。

### 实验设置
- 6 个系统：Codex+GPT-5.5、Claude Code+Opus-4.7、Qwen Code+Qwen-3.7-Max、Kimi Code+Kimi-2.6、Claude Code+DeepSeek-V4-Pro、Claude Code+GLM-5.1。
- 统一 Kali 攻击者环境 + 工具说明。
- Web 150 步、内网 500 步、2 小时超时。
- Pass@1 / Pass@3 (Avg.) / Pass@3 (Max)；成本与时间。

## 关键结果

| 指标 | GPT-5.5 + Codex | 备注 |
|---|---|---|
| Web Pass@1 | 19.09% | 最高 |
| Web Pass@3 Avg. | 16.06% | 论文摘要写 16.1% |
| Web Pass@3 Max | 28.18% | 31/110 |
| 内网 Pass@1 | 31.71% | 最高 |
| 内网 Pass@3 Avg. | 31.71% | — |
| 内网 Level-2 | 46.34% | — |

其他：Claude-Opus-4.7 Web Pass@3 Avg. 14.55%，Qwen-3.7-Max 12.42%，其余 3.03%–8.18%。

> 原文表格存在 Pass@1 > Pass@3 Avg. 的情况（19.09% vs 16.06%），引用时照原文，不要自行改算。

## 关键发现

- **Web 主瓶颈是入口发现**：漏洞深度 2→6，检测率 35%→11%；L0→L1 提示最多提升 21.81 个百分点。
- **内网主瓶颈是信息搜集与链式利用**：Agent 会在无价值主机上浪费步数；拿下 Confluence 后不会找 GitLab 凭证。
- **方差大**：很多漏洞只在一次运行中出现。
- **防御适应**：webshell 被杀后变异 payload；但噪声大、触发蜜罐。
- **benchmark 外发现**：ComfyUI 任意文件写 0day。
- **专用 Agent 未显著占优**：PentestGPT-V2 在难例上同样失败。

## 局限（作者自述）

- 不覆盖钓鱼、Windows 域、云 IAM、供应链、社工。
- 依赖系统、harness、prompt、工具、预算；API 不稳定与拒答会降低分数（Opus-4.7 12 次拒答）。
- 验证器依赖可观测运行时证据，可能低估部分进展/替代路径。
- benchmark 外漏洞需人工验证。
- 零日纳入前已 responsible disclosure。

## 可引用原句

> "GPT-5.5 with Codex achieves the highest success rates, solving 16.1% of web exploitation tasks and 31.7% of post-exploitation tasks; with more concrete hints, these rates increase to 33.0% and 46.3%."

> "Existing public benchmarks ... often abstract away the operational structure of realistic intrusions."

## 相关页面

- [[agentcyberrange]]
- [[webexploitbench]]
- [[postexploitbench]]
- [[open-book-vs-closed-book]]
- [[benchmark-validity]]

## 来源

- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- https://arxiv.org/abs/2606.14295
