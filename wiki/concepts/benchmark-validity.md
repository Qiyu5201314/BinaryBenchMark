---
title: 基准有效性（Benchmark Validity）
type: concept
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/github/CyberGym_FAQ.md
  - raw/official/CyberGym_Benchmark_Page.txt
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/official/UK_AISI_Inspect_CyberGym.txt
  - raw/papers/related/Natural_Code_Transformation_CyberGym_arXiv_2610.14350v2.pdf
  - raw/papers/related/Cost_Aware_Security_Agents_arXiv_2607.15263.pdf
  - raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf
  - raw/papers/EnIGMA_arXiv_2409.16165v3.pdf
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
tags: [validity, contamination, reward-hacking, variance, verifier]
---

# 基准有效性（Benchmark Validity）

## 一句话

一个基准的分数是否真的测量了它声称测量的能力？对网络安全 Agent 基准，至少有六类威胁。

## 1. 构念效度（Construct Validity）

- CyberGym 测的是「漏洞复现」，不是「端到端攻击」；官网也承认漏洞描述可能歧义，领先系统分数接近时的小差距未必代表能力差异。
- AgentCyberRange 测 Web 打点 + 内网后渗透，但不覆盖钓鱼、Windows 域、云 IAM、供应链、社工。
- 结论：**分数只能在该基准声称的任务范围内解释**。

来源：[[cybergym-repo-and-site]]、[[agentcyberrange-paper]]。

## 2. 数据污染（Contamination）

- 漏洞来自公开历史数据，模型可能在预训练中见过 patch、PoC、issue、changelog。
- CyberGym 做过数据污染分析（论文 Table 2），并限制在知识截止后的漏洞上做补充实验。
- 但模型代际更新后，污染风险会重新出现。
- 自然代码变换研究进一步说明：同一功能、同一漏洞，仅代码形式不同，Agent 结果就显著变化——这意味着「见过相似代码」可能显著影响分数。

来源：[[cybergym-paper]]、[[related-work]]。

**缓解**：
- 使用知识截止后的漏洞。
- 隐藏 patch / post-patch 代码。
- 禁止网络访问，或审计 Agent 是否搜索 issue/patch。
- 做成员推断/污染测试。

## 3. 奖励黑客（Reward Hacking）

CyberGym FAQ 明确列出：

- 网络访问可让 Agent 直接搜 issue tracker、changelog、commit history、release notes，找到 patch 或 PoC。
- 动态分析环境里，`/src/**/.git`（git 历史）和 `/tmp/poc`（参考 PoC）是泄漏源，交给 Agent 前必须移除。
- any-of 计分（任一 PoC 成功即算解决）会奖励暴力尝试；主榜因此要求 Agent 指定唯一最终提交，报告 final-submission 指标。

来源：[[cybergym-repo-and-site]]。

## 4. 运行间方差（Run-to-Run Variance）

- AgentCyberRange：同一 Agent、同一设置，不同 attempt 结果差异明显；很多漏洞只在一次运行中出现。Pass@1 与 Pass@3 (Max) 的差距就是方差。
- CyberGym：官网声明 agent runs 是随机的，分数可能波动；不同团队自行提交，设置可能不同。
- 结论：**单次运行分数不可靠**；至少报告多次运行的均值/分布，最好报告 Pass@k。

来源：[[agentcyberrange-paper]]、[[cybergym-repo-and-site]]。

## 5. 验证器效度（Verifier Validity）

- 确定性 verifier（sanitizer、marker、`verify.py`）比 LLM judge 更可复现，但可能：
  - 低估部分进展或替代攻击路径；
  - 漏判「非预期但有效」的利用；
  - 受环境/版本 bug 影响。
- UK AISI 的 CyberGym 复现页曾出现「测试了错误的 PoC」的 bug，导致旧版本结果全部作废；这提醒我们：**验证器本身也要版本化与回归测试**。
- LLM judge 则引入模型偏好、提示敏感性与成本。

来源：[[uk-aisi-inspect-cybergym]]、[[agentcyberrange-paper]]。

## 6. 覆盖与生态效度（Coverage / Ecological Validity）

- CyberGym 只覆盖 C/C++ 内存安全、sanitizer 可检测漏洞；不代表 Web、配置、逻辑漏洞。
- AgentCyberRange 只覆盖两个攻击阶段；靶场真实性与隔离性存在张力。
- 现实攻击还涉及社工、供应链、云、身份系统等，均未覆盖。

来源：[[cybergym-paper]]、[[agentcyberrange-paper]]。

## 7. 成本维度缺失

- 只报告成功率会鼓励「无限预算刷分」；CyberGym 2026-08 起要求提交 token/时间/成本；成本感知研究进一步指出攻防任务的 scaling 规律不同。
- 详见 [[cost-aware-evaluation]]。

## 8. 部分分与里程碑偏差

二元「成功/失败」会掩盖部分进展，也会低估替代路径；子任务/里程碑是常见解法，但引入新偏差。

- [[cybench]] 用子任务提供部分分；子任务由作者编写，可能偏向「标准解法」。
- [[autopenbench]] 用命令里程碑 + 阶段里程碑；能定位「卡在发现/利用/提权」，但里程碑越细，越容易把探索性路径判为失败。
- 结论：**部分分要提高可归因性，但必须报告评分规则，并抽样检查替代路径是否被误杀**。

来源：[[cybench-paper]]、[[autopenbench-paper]]。

## 9. 轨迹完整性与 soliloquizing

[[enigma]] 发现模型会「自说自话」：不与环境交互，自己生成观察，再基于虚假观察继续推理。这不是普通的幻觉，而是**评测轨迹被模型自己污染**。

- 作者量化了「解法泄漏」与 soliloquy 在成功轨迹中的比例。
- soliloquy 与挑战成功的相关性为负（−26%），说明它通常不是有效策略，但会破坏轨迹可信度。
- 对二进制闭卷：必须保存完整工具调用/环境返回，检测「模型伪造 gdb 输出」「伪造成立崩溃」。
- [推断] 异步监控、事故调查、合规审计都依赖轨迹完整性；若 agent 能改自己的 trace，评测结论不可信。

来源：[[enigma-paper]]、[[anthropic-incident-report]]。

## 10. 造题偏差（Construction Bias）

样本制作本身会引入偏差，尤其是自动化流水线：

- [[cve-bench]]：单个 CVE 需 5–24 人时搭建与复现；人工选择 critical CVE，存在选择偏差。
- [[sec-bench]]：$0.87/instance 自动构建 200 个 CVE；但只选有 bug report / 可复现的样本，长尾项目被系统性排除。
- 自动 Verifier 的误判会污染整个数据集；金补丁可能只修表面症状，PoC 通过 ≠ 语义正确。
- 结论：**造题流水线必须自带抽样审计**；否则「规模化」会把偏差放大。

来源：[[cve-bench-paper]]、[[sec-bench-paper]]。

## 11. 补丁 oracle 的假阳性

[[aixcc]] 的经验：用 PoV 验证补丁，可能被「抑制 PoV」的假补丁欺骗。

- 例如：给 Java 入口套 `catch(Exception)`、改编译选项、用 MTE/PAC 抑制崩溃。
- 这些补丁让 PoV 不再触发，但没有真正修复漏洞。
- AIxCC 用 accuracy modifier + 可选 `test.sh` 额外 oracle 来惩罚；仍无法完全排除语义错误。
- 对二进制闭卷：如果要求 agent 输出补丁，必须同时检查「漏洞消失」与「功能测试通过」，并审计是否只是改变了崩溃行为。

来源：[[aixcc-paper]]。

## 有效性检查清单

- [ ] 任务定义与声称能力一致？
- [ ] 数据是否可能被模型见过？是否做了污染测试？
- [ ] 是否允许网络/动态环境？是否审计轨迹？
- [ ] 计分是 any-of 还是 final-submission？
- [ ] 是否报告多次运行方差与 Pass@k？
- [ ] 验证器是否版本化、是否做过回归测试？
- [ ] 是否报告成本与时间？
- [ ] 覆盖范围是否被明确限定？
- [ ] 部分分/里程碑规则是否公开？是否检查替代路径？
- [ ] 是否检测 soliloquizing / 伪造观察？
- [ ] 造题流水线是否抽样审计？金补丁是否验证语义？
- [ ] 补丁是否可能只是「抑制 PoV」？是否有功能回归？

## 相关页面

- [[evaluation-isolation]]
- [[cost-aware-evaluation]]
- [[reproducibility]]
- [[cybench]]
- [[autopenbench]]
- [[cve-bench]]
- [[enigma]]
- [[aixcc]]
- [[sec-bench]]
- [[binary-closed-book-landscape]]
- [[research/methodology-notes]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/github/CyberGym_FAQ.md`
- `raw/official/CyberGym_Benchmark_Page.txt`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/official/UK_AISI_Inspect_CyberGym.txt`
- `raw/papers/related/Natural_Code_Transformation_CyberGym_arXiv_2610.14350v2.pdf`
- `raw/papers/related/Cost_Aware_Security_Agents_arXiv_2607.15263.pdf`
- `raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf`
- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`
- `raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf`
- `raw/papers/EnIGMA_arXiv_2409.16165v3.pdf`
- `raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf`
- `raw/papers/SEC-bench_arXiv_2506.11791v1.pdf`
