---
title: 来源摘要：DARPA AIxCC 与 FuzzingBrain
type: source
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/github/AIxCC_FuzzingBrain_README.md
  - raw/official/AIxCC_ARPAH_Results.txt
  - raw/official/AIxCC_Archive.txt
  - raw/official/AIxCC_TeamAtlanta_Blog.txt
tags: [source, aixcc, cyber-reasoning-system, vulnerability-discovery, patching]
---

# 来源摘要：DARPA AIxCC 与 FuzzingBrain

## 元数据

| 项 | 内容 |
|---|---|
| 主体 | DARPA / ARPA-H AI Cyber Challenge（AIxCC，2023–2025）；决赛于 DEF CON 33（2025-08） |
| 论文 | All You Need Is A Fuzzing Brain（arXiv 2509.07225，2025-09-08） |
| 本地文件 | `raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf`、`raw/github/AIxCC_FuzzingBrain_README.md`、`raw/official/AIxCC_*.txt` |
| 原始 URL | https://arxiv.org/abs/2509.07225 ；https://archive.aicyberchallenge.com/ ；https://team-atlanta.github.io/blog/post-afc/ |

## 核心主张

1. AIxCC 要求参赛系统（CRS）在真实开源 C/Java 项目上**自主发现漏洞、生成 PoV、生成补丁**。
2. 决赛评分优先看「快速打补丁」与「漏洞-补丁配对质量」，而不仅是发现数量。
3. 七个决赛队伍的 CRS 均承诺开源（ARPA-H 公告时 5 个已发布），形成可复用的「自动漏洞发现+修复」基础设施。
4. FuzzingBrain 用 coverage-guided fuzzing + LLM「可疑点推理」组合，强调所有发现必须被真实构建的 sanitizer 验证。

## 关键数字

| 指标 | 数值 | 口径 / 备注 |
|---|---|---|
| 决赛队伍 | 7 | 均承诺开源；公告时 5 个已发布 |
| 冠军 | Team Atlanta | $4M；Trail of Bits $3M；Theori $1.5M |
| 决赛代码量 | 5,400 万行 | 跨多个真实项目 |
| 合成漏洞 | 63 个 | ARPA-H 更正：早期稿写 70，复核后为 63 |
| 被发现 | 54 个 | 所有队伍合计 |
| 被修补 | 43 个 | 所有队伍合计 |
| 真实漏洞 | 18 个 | 6 个 C、12 个 Java；11 个补丁 |
| 单任务成本 | 约 $152 | 平均 |
| FuzzingBrain 名次 | 第 4 | 28 个漏洞（含 6 个 0day），修补 14 个 |
| PoV 验证增益 | 0.9999 vs 0.9044 | Team Atlanta：PoV 验证后的 accuracy modifier vs Theori 44.4% 准确率 |

## 方法

- **AIxCC 任务**：给目标源码与 fuzz harness，要求生成 PoV（sanitizer 触发输入）与 diff 补丁。
- **评分**：发现、PoV、补丁、配对、bug report 质量；用 accuracy modifier 惩罚低质量/误报提交。
- **FuzzingBrain**：把代码库分区 → LLM 推理可疑点（SP）→ 构造 PoV → sanitizer 验证 → 生成补丁。
- **Team Atlanta（Atlantis）**：多 fuzzer 集成（coverage-guided、directed、concolic）+ 多 LLM agent 集成；PoV 作为补丁 oracle；可选 test.sh 作为额外 oracle。
- **补丁验证**：把 PoV 重新跑在 patched 程序上；但「PoV 不再触发」不等于语义正确（可能只是 mitigation）。

## 方法学启示

- **AIxCC 是「开卷」的**：给源码与 harness，不是二进制闭卷；但它把「发现 → PoV → 补丁」的完整链路做成了可评测对象。
- **PoV 作为 oracle 是双刃剑**：它能稳定验证补丁，但也可能被「抑制 PoV」的假补丁欺骗（如加 catch、改编译选项）。
- **多 agent/多 fuzzer 集成有效**：Team Atlanta 明确用正交方法集成，并用 oracle 筛掉幻觉。
- **成本可接受**：$152/任务，远低于人工漏洞挖掘，但仍是「有预算的搜索」。
- **开源 CRS 是重要资产**：可直接复用其 harness、PoV 验证、补丁生成组件。

## 局限

- 任务基于源码与 fuzz harness，不是「只给二进制 + 入口」。
- 合成漏洞由组织方注入，真实感有限；真实漏洞只占少数。
- 评分规则复杂，不同届/不同轮的分数不可直接比较。
- PoV 验证无法保证补丁语义正确；需要额外测试/人工复核。
- 竞赛环境与生产环境差异大（构建、依赖、时间限制）。

## 可引用原句

> "Competitors' systems discovered 54 unique synthetic vulnerabilities in the Final Competition's 63 challenges. Of those, they patched 43."

> "All seven finalist teams' Cyber Reasoning Systems (CRSs) will be made available as open-source software."

## 相关页面

- [[aixcc]]
- [[slyp]]
- [[sec-bench]]
- [[vulnerability-reproduction]]
- [[benchmark-validity]]
- [[reproducibility]]

## 来源

- `raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf`
- `raw/github/AIxCC_FuzzingBrain_README.md`
- `raw/official/AIxCC_ARPAH_Results.txt`
- `raw/official/AIxCC_Archive.txt`
- `raw/official/AIxCC_TeamAtlanta_Blog.txt`
