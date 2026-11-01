---
title: 开放问题与口径冲突
type: research
status: needs-review
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/github/CyberGym_SUBMISSION.md
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/articles/Fudan_AgentCyberRange_Article.md
  - raw/articles/WeChat_CyberGym_AgentCyberRange.md
  - raw/official/UK_AISI_Inspect_CyberGym.txt
  - raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/official/AIxCC_ARPAH_Results.txt
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
  - raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
tags: [open-questions, research-agenda, discrepancy]
---

# 开放问题与口径冲突

## A. 口径冲突（引用前必读）

### A1. CyberGym 的 22% vs 95%+

| 来源 | 数字 | 设置 |
|---|---|---|
| 论文（ICLR 2026） | 22.0% | GPT-5 + high reasoning，L1，受控实验 |
| 博客（2025-10） | ~30% 单次；~67% 30 次 | 早期模型/设置 |
| 官网 leaderboard | 论文时代 22%，后续显著更高 | 各团队自行提交，运行随机 |
| 提交指南（2026-08-04） | 「接近饱和」 | 无约束资源 |
| 白泽公众号（2026） | 95+ | 未说明具体设置 |

**结论**：不能把 95% 与 22% 直接比较。需要追问：哪个模型、多少 trial、是否给动态环境、是否联网、是否 any-of、成本多少。

### A2. AgentCyberRange 的规模

| 来源 | Web apps | 漏洞 | 内网 | 主机 |
|---|---|---|---|---|
| 论文 v2（2026-06） | 15 | 110 | 8 | 156 |
| 复旦文章（2026-07） | 25 | 近 500 | 30 | 近 400 |
| 白泽公众号（2026） | 30 | 超 200 | 50+ | 超 300 |
| 根 README（2026-10-29） | 15 | 110 | 8 | 156（另写 266 任务） |

**结论**：论文版是可复现基线；文章是平台当前/累计口径。引用论文结果时必须用论文规模。

### A3. CyberGym 外溢发现

| 来源 | 0day | 不完整补丁 |
|---|---|---|
| 论文 / 官网 | 34 | 18 |
| 早期博客 | 35 | 17 |

可能是去重/统计时点差异。引用时注明来源与日期。

### A4. AgentCyberRange 表格内部不一致

论文 Table 4 中 GPT-5.5 的 Pass@1（19.09%）高于 Pass@3 Avg.（16.06%）。通常 Pass@3 Avg. 不应低于 Pass@1，除非聚合方式/样本不同。**照原文引用，不自行改算**；如要用于自己的论文，需回原文附录确认定义。

来源：[[agentcyberrange-paper]]。

### A5. CVE-Bench 的 10% vs 13%

| 来源 | 数字 | 设置 |
|---|---|---|
| 论文 v4 正文 | zero-day 10%、one-day 12.5% | gpt-4o，success@5，30 次迭代 |
| 论文 v4 摘要/图注 | one-day 13% | 同一实验的不同表述 |
| README | 40 个 critical CVE | v2.x 后续更新，攻击判定有调整 |

**结论**：10% 与 13% 不是矛盾，而是 zero-day/one-day 与四舍五入的差异；引用时必须写清设置。来源：[[cve-bench-paper]]。

### A6. AIxCC 的 70 vs 63

| 来源 | 数字 | 说明 |
|---|---|---|
| ARPA-H 早期版本 / 第三方总结 | 70 个合成漏洞 | 复核前口径 |
| ARPA-H 官方（2025-09-04） | 63 个合成漏洞 | 复核后更正；54 发现、43 修补 |

**结论**：以官方 63 为准；不要再引用 70。来源：[[aixcc-paper]]。

---

## B. 研究开放问题

### B1. 二进制闭卷怎么定义任务？
- 只给二进制 + 入口，不给源码/描述，要求发现并利用漏洞。
- 判定用什么：崩溃？控制流劫持？任意代码执行？拿到 flag？
- 如何区分「漏洞发现」与「利用成功」？
- 相关：[[projects/binary-closed-book-eval]]

### B2. 二进制闭卷的验证器怎么做？
- 没有源码，sanitizer 可能不可用；需要崩溃复现、符号执行、动态插桩、差分验证。
- 如何避免把「任意崩溃」误判为「目标漏洞」？
- 多架构（x86/ARM/MIPS）如何统一？
- 相关：[[benchmark-validity]]、[[reproducibility]]

### B3. 闭卷下的探索策略如何测量？
- AgentCyberRange 用「漏洞深度」解释 Web 探索瓶颈；二进制是否有类似指标（函数深度、调用图距离、符号恢复难度）？
- 如何区分「找不到入口」与「不会利用」？
- 相关：[[difficulty-levels]]、[[post-exploitation]]

### B4. 如何控制污染？
- 二进制可能包含版本字符串、调试符号、编译路径，泄露来源。
- 公开漏洞的 patch/PoC 可能被模型见过。
- 如何构建「知识截止后」的二进制任务？
- 相关：[[benchmark-validity]]、[[arvo-oss-fuzz]]

### B5. 成本与成功率的权衡
- 闭卷搜索空间大，成本可能爆炸；如何设计预算？
- 是否应报告 cost-success frontier？
- 相关：[[cost-aware-evaluation]]

### B6. 防御方参与
- 现有基准大多没有主动防御 Agent；AgentCyberRange 有 AV/监控，但不是对抗性防御。
- 如何设计「攻防对抗」评测？
- 相关：[[cyber-range]]

### B7. 评测环境安全
- 二进制闭卷可能需要执行未知样本；沙箱逃逸风险更高。
- 如何做到「可执行、可观测、不可逃逸」？
- 相关：[[evaluation-isolation]]、[[anthropic-incident-report]]

### B8. Harness 混杂
- 换 harness 可能比换模型影响更大；如何分离模型能力与系统能力？
- 相关：[[agent-harness]]

### B9. 部分进展与部分分
- 现有验证多为二元成功/失败；长链任务中「攻陷 3/5 台主机」如何计分？
- AgentCyberRange 用 marker 数量/进度，但仍可能低估替代路径。
- 相关：[[benchmark-validity]]

### B10. 真实性与伦理
- 越真实的靶场越有用，也越危险；如何平衡？
- 未公开漏洞如何 responsible disclosure？
- 相关：[[evaluation-isolation]]、[[cyber-range]]

### B11. 二进制闭卷的「最近邻」与可比性
- [[vulbinllm]]、[[veritas]] 只测检测/推理，[[slyp]] 测发现 + PoC；三者能否放在同一张榜上？
- Slyp 的 COM 元数据、调试接口算不算「闭卷」？如果不算，应该归到哪个信息等级？
- 如何把「检测 recall」与「利用成功率」合成一个可解释的指标？还是必须分开报告？
- 相关：[[binary-closed-book-landscape]]

### B12. 检测与利用之间的鸿沟
- Veritas 90% recall、Slyp 67.5% PoC，是否说明「找到」比「打中」容易？还是任务难度不同？
- 在二进制闭卷里，如何测量「定位正确但利用失败」？需要哪些中间证据（witness、崩溃地址、控制流原语）？
- 相关：[[benchmark-validity]]、[[difficulty-levels]]

### B13. 自动造题的成本与偏差
- [[sec-bench]] 把源码级造题降到 $0.87/instance；二进制版本（多架构、多优化、sanitizer 版本矩阵）成本会是多少？
- 自动 Verifier 的误判率如何度量？需要多少抽样人工复核？
- 回灌层（新版本基座 + 只回退单修复）能否自动化？
- 相关：[[sec-bench]]、[[binary-closed-book-guide]]

---

## C. 待补素材

- [ ] CyberGym 论文 Appendix C（实验配置、prompt、成本）
- [ ] AgentCyberRange 论文 Appendix B（prompt 模板）与 Appendix A（成本单位）
- [ ] CyberGym 数据污染分析（Table 2）细节
- [ ] AgentCyberRange 的 verifier 实现细节
- [ ] 更多第三方复现（除 UK AISI）
- [ ] BountyBench（Cybench 团队后续工作，真实 detect/exploit/patch + 美元影响）
- [ ] Google Project Zero / DeepMind 的 Big Sleep、Naptime 官方博客
- [ ] AIxCC 七个决赛 CRS 的代码与评测接口（目前只收了 FuzzingBrain）
- [ ] Slyp / Veritas / VulBinLLM 的代码与数据（若公开）
- [ ] SEC-bench 数据集与自动构造流水线细节
- [ ] 2026 年新工作：CTFusion、SEC-bench Pro、ZeroDayBench
- [ ] 跨架构二进制闭卷的统一验证器

## 相关页面

- [[research/research-map]]
- [[research/methodology-notes]]
- [[projects/binary-closed-book-eval]]
- [[benchmark-validity]]
- [[binary-closed-book-landscape]]
- [[cve-bench]]
- [[aixcc]]
- [[slyp]]
- [[veritas]]
- [[vulbinllm]]
- [[sec-bench]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/github/CyberGym_SUBMISSION.md`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/articles/Fudan_AgentCyberRange_Article.md`
- `raw/articles/WeChat_CyberGym_AgentCyberRange.md`
- `raw/official/UK_AISI_Inspect_CyberGym.txt`
- `raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf`
- `raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf`
- `raw/official/AIxCC_ARPAH_Results.txt`
- `raw/papers/Slyp_arXiv_2605.05000v1.pdf`
- `raw/papers/Veritas_arXiv_2605.15097v3.pdf`
- `raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf`
- `raw/papers/SEC-bench_arXiv_2506.11791v1.pdf`
- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`
