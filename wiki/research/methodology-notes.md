---
title: 评测方法笔记
type: research
status: draft
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/github/CAGE_docs/how-a-run-works.md
  - raw/official/UK_AISI_Inspect_CyberGym.txt
tags: [methodology, benchmark-design, checklist]
---

# 评测方法笔记

> 这是 LLM 基于现有素材整理的设计笔记，不是任何原文的结论。用于指导「二进制闭卷评测」的设计。

## 1. 设计一个基准的步骤

1. **定义构念**：到底测什么能力？漏洞发现、利用、链式规划、隐蔽性？
2. **定义任务**：输入/输出/成功条件；单点还是端到端。
3. **构建数据**：真实来源、版本、许可、去重、污染控制。
4. **设计环境**：源码/二进制/Web/内网；隔离、可重置、可复现。
5. **选择验证器**：确定性优先；必要时 LLM judge；版本化 + 回归测试。
6. **定义指标**：成功率、Pass@k、成本、时间、方差、部分进展。
7. **做基线**：至少 3 个模型 × 2 个 harness，报告完整设置。
8. **做消融**：难度等级、提示、工具、预算、网络。
9. **做有效性检查**：污染测试、reward hacking 审计、跨环境复现。
10. **公开**：数据、prompt、轨迹、成本、失败案例。

## 2. 任务设计模板

```yaml
task:
  name:
  goal:            # 发现 / 复现 / 利用 / 端到端
  input:
    source:        # source | binary | none
    description:   # none | text | stack | patch
    target:        # URL | IP | binary path
  output:
    artifact:      # PoC | exploit | shell | report
    final_answer:  # 唯一最终提交？
  success:
    verifier:      # sanitizer | marker | flag | judge
    condition:     # crash | no-crash | marker present | judge pass
  budget:
    steps:
    time:
    cost:
  difficulty:
    level:         # L0/L1/L2/...
    hints:         # 给什么信息
```

## 3. 指标模板

| 指标               | 定义               | 备注                             |
| ---------------- | ---------------- | ------------------------------ |
| Success Rate     | 成功任务 / 总任务       | 必须写 final-submission 还是 any-of |
| Pass@1           | 单次运行             | 最接近真实使用                        |
| Pass@k (Avg.)    | k 次平均            | 反映稳定性                          |
| Pass@k (Max)     | k 次任一成功          | 上限能力，可能奖励暴力                    |
| Cost / Task      | token / USD      | 分 input/cache/output           |
| Time / Task      | 墙钟               | 含工具执行                          |
| Variance         | 多次运行标准差          | 比单点更有信息                        |
| Partial Progress | 阶段性 marker / 子目标 | 长链任务建议                         |

## 4. 偏误与陷阱

### 4.1 信息泄漏
- 源码、git 历史、patch、参考 PoC、构建路径、版本字符串。
- 二进制任务尤其要检查：调试符号、`.comment`、字符串、编译时间、路径。
- 动态环境交付前移除 `/src/**/.git`、`/tmp/poc`。

来源：[[cybergym-repo-and-site]]。

### 4.2 验证器偏差
- 只看「崩溃」可能把非目标崩溃算成功。
- 只看「最终成功」会低估部分进展。
- LLM judge 可能偏好某种报告格式。
- 验证器 bug 会让整批结果作废（UK AISI 2-A）。

来源：[[uk-aisi-inspect-cybergym]]、[[benchmark-validity]]。

### 4.3 Harness 混杂
- 固定模型换 harness，分数可能大变；固定 harness 换模型，也可能被工具集限制。
- 至少报告：模型、harness、工具、预算、prompt。

来源：[[agent-harness]]。

### 4.4 方差与预算
- 单次运行不可靠；预算越长成功率越高，但成本也越高。
- 报告 cost-success frontier，而不是单点。

来源：[[cost-aware-evaluation]]。

### 4.5 污染
- 公开漏洞可能被模型见过。
- 用知识截止后数据、做污染测试、隐藏 patch。
- 自然代码变换研究表明：仅代码形式变化就能显著改变结果，提示「模式匹配」风险。

来源：[[related-work]]。

## 5. 二进制闭卷的实验矩阵（建议）

| 变量 | 水平 |
|---|---|
| 信息 | 二进制+入口 / 二进制+描述 / 源码+描述 |
| 架构 | x86_64 / ARM64 / MIPS（至少 2 种） |
| 漏洞类型 | 栈溢出 / 堆溢出 / UAF / 格式化字符串 / 整数溢出 |
| 难度 | 无符号 / 部分符号 / 全符号；无保护 / 部分保护 / 全保护 |
| Agent | Codex / Claude Code / 自研 |
| 预算 | 50 / 150 / 500 步 |
| 验证 | 崩溃 / 控制流劫持 / 任意代码执行 |
| 防御 | 无 / ASLR / NX / Canary / CFI |

> [推断] 这个矩阵用于分离「二进制分析难度」与「漏洞利用难度」，避免把所有失败都归因于「模型不行」。

## 6. 报告清单

- [ ] 构念与任务定义
- [ ] 数据来源、版本、许可、去重
- [ ] 环境（镜像 digest、架构、保护机制）
- [ ] Agent（模型、harness、工具、预算）
- [ ] Prompt 模板与难度等级
- [ ] 验证器与回归测试
- [ ] 指标（成功率、Pass@k、成本、时间、方差）
- [ ] 失败模式分类
- [ ] 污染与 reward hacking 审计
- [ ] 伦理与 responsible disclosure

## 相关页面

- [[research/research-map]]
- [[research/open-questions]]
- [[benchmark-validity]]
- [[reproducibility]]
- [[projects/binary-closed-book-eval]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/github/CAGE_docs/how-a-run-works.md`
- `raw/official/UK_AISI_Inspect_CyberGym.txt`
