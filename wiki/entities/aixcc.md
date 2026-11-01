---
title: AIxCC（DARPA AI Cyber Challenge）
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/github/AIxCC_FuzzingBrain_README.md
  - raw/official/AIxCC_ARPAH_Results.txt
  - raw/official/AIxCC_Archive.txt
  - raw/official/AIxCC_TeamAtlanta_Blog.txt
tags: [entity, aixcc, cyber-reasoning-system, vulnerability-discovery, patching]
---

# AIxCC（DARPA AI Cyber Challenge）

## 一句话

2023–2025 年由 DARPA/ARPA-H 举办的自动漏洞发现与修复竞赛：参赛 CRS 在真实开源 C/Java 项目中自主生成 PoV 与补丁，2025-08 在 DEF CON 33 决赛。

**评测范式定位**：源码开卷 + 自动修复，不是二进制闭卷；但它把「发现 → PoV → 补丁」的完整链路做成了可评测对象。

> [推断] 它是二进制闭卷最重要的工程参照之一：PoV、sanitizer、补丁回归都可复用；但输入契约不同，不能直接当二进制分数。

## 规模与结果（决赛）

| 维度 | 数值 | 说明 |
|---|---|---|
| 决赛队伍 | 7 | 均承诺开源；ARPA-H 公告时 5 个已发布 |
| 代码量 | 5,400 万行 | 跨真实开源项目 |
| 合成漏洞 | 63 | ARPA-H 复核后口径；早期稿写 70 |
| 被发现 | 54 | 所有队伍合计 |
| 被修补 | 43 | 所有队伍合计 |
| 真实漏洞 | 18 | 6 个 C、12 个 Java；11 个补丁 |
| 单任务成本 | 约 $152 | 平均 |
| 冠军 | Team Atlanta | $4M；Trail of Bits $3M；Theori $1.5M |

来源：[[aixcc-paper]]。

## 任务与判分

- **输入**：目标源码 + fuzz harness；不是二进制。
- **输出**：PoV（sanitizer 触发输入）+ 补丁 diff + bug report。
- **判分**：发现、PoV、补丁、漏洞-补丁配对、报告质量；用 accuracy modifier 惩罚低质量提交。
- **关键设计**：PoV 作为补丁的 oracle；补丁必须在 patched 程序上让 PoV 不再触发。
- **漏洞**：既有组织方注入的合成漏洞，也有真实 0day；真实漏洞需 responsible disclosure。

## FuzzingBrain（第 4 名）

- 架构：coverage-guided fuzzing + LLM「可疑点（SP）推理」。
- 结果：28 个漏洞（含 6 个 0day），修补 14 个。
- 原则：所有报告必须由真实构建的 sanitizer 验证，避免幻觉。
- 开源：`raw/github/AIxCC_FuzzingBrain_README.md`。

## 对二进制闭卷的启示

1. **Oracle 可以复用**：PoV 验证、sanitizer、补丁回归测试，都能迁移到二进制样本。
2. **补丁验证不等于语义正确**：Team Atlanta 明确讨论「抑制 PoV 的假补丁」（加 catch、改编译选项），二进制闭卷同样要防。
3. **多方法集成有效**：coverage-guided、directed、concolic、LLM agent 正交组合，比单一路线稳。
4. **成本可接受**：$152/任务说明自动漏洞发现已进入「可批量跑」的阶段。
5. **但闭卷程度不够**：给源码与 harness，测的是「代码理解 + 修复」，不是「只看二进制」。

> [推断] 二进制闭卷可以直接借用 AIxCC 的 CRS 组件（fuzzer、PoV 验证、补丁生成），但必须替换输入契约：去掉源码与 harness，只留二进制与入口。

## 局限

- 源码级，不是二进制级；不能直接回答「逆向税」。
- 合成漏洞占比高，真实感有限。
- 竞赛评分规则复杂，跨届不可比。
- PoV 通过 ≠ 补丁语义正确；仍需测试/人工复核。
- 竞赛环境与生产环境差异大。

## 相关页面

- [[slyp]]
- [[sec-bench]]
- [[vulnerability-reproduction]]
- [[benchmark-validity]]
- [[reproducibility]]

## 来源

- 论文：`raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf`；https://arxiv.org/abs/2509.07225
- 官方结果：`raw/official/AIxCC_ARPAH_Results.txt`；https://arpah.gov/news-and-events/arpa-h-darpa-challenge-showcases-ais-power-secure-americas-health-care
- 竞赛归档：`raw/official/AIxCC_Archive.txt`；https://archive.aicyberchallenge.com/
- Team Atlanta：`raw/official/AIxCC_TeamAtlanta_Blog.txt`；https://team-atlanta.github.io/blog/post-afc/
