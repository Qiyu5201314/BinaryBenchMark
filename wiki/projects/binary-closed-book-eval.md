---
title: 项目：二进制闭卷评测
type: project
status: draft
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/articles/Fudan_AgentCyberRange_Article.md
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
  - raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf
tags: [project, binary, closed-book, research]
---

# 项目：二进制闭卷评测

> 这是研究项目页，不是已发表结论。`status: draft`。

## 1. 研究目标

> 方法论基线与术语（信息契约 L0–L4、Oracle、八级任务、Level A/B）见 [[binary-closed-book-guide]]。

构建一套面向**二进制闭卷**的 Agent 能力评测：

> 只给 Agent 一个目标二进制（及必要入口），**不给源码、不给漏洞描述、不给补丁**，测量它能否自主完成「侦察 → 漏洞发现 → 利用 → 控制流劫持/代码执行」的完整链路。

与现有工作的区别：

| 工作 | 源码 | 漏洞描述 | 任务 | 是否二进制闭卷 |
|---|---|---|---|---|
| CyberGym L0 | 给 | 不给 | 单点发现 | ❌ 给源码 |
| CyberGym L1–L3 | 给 | 给 | 单点复现 | ❌ |
| AgentCyberRange Web | 不给 | 不给 | Web 打点 | ❌ 目标不是二进制 |
| AgentCyberRange Post | 不给 | 不给 | 内网后渗透 | ❌ 目标不是二进制 |
| **本项目** | **不给** | **不给** | **二进制发现+利用** | ✅ |

## 1.1 2023–2026 前沿对项目的修正

> 详细版图见 [[binary-closed-book-landscape]]。

| 新证据 | 对项目的影响 |
|---|---|
| [[slyp]] 在无源码 COTS Windows 二进制上发现 28 个未知漏洞、16 个 CVE，PoC 67.5% | 「二进制闭卷不可行」已被证伪；但它是 COM 竞态领域专用，不是通用基准 |
| [[veritas]] 在 stripped binary 上 90% recall，静态 witness + 运行时验证 | Oracle 可以做成「声明 + 证据」两层，不必只等最终崩溃 |
| [[vulbinllm]] 在 Juliet 上 CWE F1 74–99% | 检测层可作为任务的低难度档；但合成数据不能代表真实项目 |
| [[sec-bench]] $0.87/instance 自动构建 200 个 CVE | 「造题成本高」有解，但二进制版本成本更高，需要版本矩阵 |
| [[aixcc]] 决赛 54 发现 / 43 修补，$152/任务 | PoV + 补丁的完整链路可复用；要防「抑制 PoV 的假补丁」 |
| [[cybench]] FST + 子任务、[[autopenbench]] 阶段里程碑 | 难度锚与部分分有成熟方案，可直接映射到八级任务谱系 |

对假设的修正：

- **H1 需要限定域**：Slyp 说明前沿 agent 在特定二进制域（COM 竞态）已能发现真实 0day，不能笼统断言「闭卷成功率一定远低于 22%」。应改为「在跨类型、跨架构的通用二进制闭卷上，成功率显著低于开卷」。
- **H2 更有依据**：Veritas 的「证据 grounding」与 EnIGMA 的「工具接口」都说明，**结构化证据/工具**比「更多文字描述」更有价值。
- **H3 仍成立**：Slyp 的收益很大程度来自专用工具；harness 差异在二进制闭卷下更大。
- **H4 仍成立**：二进制搜索空间大，Veritas 的验证成本、Slyp 的长程上下文都指向成本瓶颈。
- **H5 仍成立**：VulBinLLM 只在 Juliet 上验证，正是「合成/变换数据外部效度不足」的例子。

## 2. 研究问题

1. 二进制闭卷下，Agent 的「发现」与「利用」能力如何分离测量？
2. 没有源码/sanitizer 时，如何构建可靠的验证器？
3. 二进制形态（架构、优化级别、符号、保护机制）如何影响难度与结果？
4. 闭卷是否改变了 Agent 的失败模式（探索 vs 利用 vs 链式规划）？
5. 成本-成功率曲线在闭卷下是否与开卷不同？
6. 如何控制污染（公开漏洞、编译器指纹、字符串泄漏）？

## 3. 假设（可证伪）

- H1：在二进制闭卷下，前沿 Agent 的成功率显著低于 CyberGym L1 的 22%，且差距主要来自「定位漏洞」而非「构造输入」。
- H2：给出函数级提示（symbol/反编译片段）带来的提升，大于给出漏洞描述带来的提升。
- H3：harness 差异（Codex vs Claude Code）在闭卷下比开卷下更大，因为探索策略更重要。
- H4：成本-成功率曲线在闭卷下更陡，说明搜索成本是主要瓶颈。
- H5：自然代码变换/编译优化会显著改变成功率，构成基准有效性威胁。

> 这些假设需要实验验证；目前只是研究设计。

## 4. 可复用的现有资产

| 资产 | 用途 |
|---|---|
| [[binary-closed-book-guide]] | 作者自撰总纲：信息契约 L0–L4、Oracle 七武器、八级任务、Level A/B |
| [[cybergym]] | 任务构建流程、sanitizer 验证、难度分级、成本上报 |
| [[arvo-oss-fuzz]] | 漏洞语料与可复现构建 |
| [[agentcyberrange]] | 闭卷叙事、L0–L2 提示设计、marker 验证 |
| [[cage]] | 执行框架：容器、代理、快照、续跑、轨迹导出 |
| [[uk-aisi-inspect-cybergym]] | 第三方复现经验、镜像固定、验证器版本化 |
| [[related-work]] | 成本指标、代码变换鲁棒性 |
| [[slyp]] | 二进制闭卷端到端参照：三类工具接口、ReAct、debugger-verified PoC |
| [[veritas]] | Oracle 设计：静态 witness + 运行时验证，90% recall |
| [[sec-bench]] | 自动造题流水线：Preprocessor → Verifier → Evaluator |
| [[aixcc]] | PoV/补丁验证、多 fuzzer/多 agent 集成、CRS 开源组件 |
| [[cybench]] / [[autopenbench]] | FST 难度锚、子任务、阶段里程碑 |

## 5. 初步设计

### 5.1 任务
- 输入：目标二进制 + 运行入口（stdin/文件/网络端口），可选架构信息。
- 不给：源码、漏洞描述、patch、参考 PoC、调试符号（除非作为难度变量）。
- 输出：PoC/输入 + 漏洞报告；要求指定唯一最终提交。
- 成功：在目标版本触发预定义影响（崩溃/控制流劫持/任意写/代码执行），且在修补版本不触发（若能获得）。
- 难度：控制符号、保护机制（ASLR/NX/Canary/CFI）、优化级别、架构。

### 5.2 环境
- Docker 隔离；每 trial 独立实例；禁止外网；镜像 digest 固定。
- 动态分析工具可选（gdb、rr、angr、qemu），作为变量而非默认。
- 交付前移除泄漏源（构建路径、符号、参考输入）。

### 5.3 验证
- 首选确定性验证：崩溃签名、控制流劫持、目标内存写。
- 差分验证：pre/post 版本行为对比。
- 若无法获得 patch，则由人工/规则定义「目标影响」。
- 验证器版本化 + 回归测试。

### 5.4 指标
- Success Rate、Pass@1/Pass@3 Avg./Max。
- 发现率 vs 利用率（分离两个阶段）。
- 成本（token/USD）、时间、工具调用次数。
- 失败模式分类：未定位 / 定位未利用 / 利用不稳定 / 环境失败。
- 方差。

## 6. 里程碑

- [ ] M0：复现 [[slyp]] / [[veritas]] 的最小闭环（1 个二进制 + 探索/调试工具 + debugger-verified PoC）。
- [ ] M1：完成 10 个二进制任务的手工构建与验证器原型。
- [ ] M2：跑通 3 个 Agent × 2 个 harness 的 baseline。
- [ ] M3：完成难度消融（符号/保护/架构）。
- [ ] M4：完成污染与 reward hacking 审计（含 soliloquizing、假补丁）。
- [ ] M4.5：参考 [[sec-bench]] 做自动造题流水线原型（10 题规模）。
- [ ] M5：扩展到 100+ 任务，公开数据与轨迹。
- [ ] M6：论文写作。

## 7. 开放决策

- 是否要求「利用成功」还是只要求「崩溃」？
- 是否把「检测/定位」与「利用」拆成两个榜单？还是用阶段分合成？
- 是否允许动态分析工具？允许哪些（objdump / Ghidra / gdb / rr / angr / Frida）？
- 是否允许 COM 式元数据（导入表、节区、调用图、RTTI）？这算功能信息还是身份信息？
- 如何处理无 patch 的漏洞？
- 是否要求补丁，还是只要求 PoC？
- 自动造题流水线是否作为一等公民（参考 [[sec-bench]]）？
- 是否包含真实世界固件/协议？
- 数据许可与 responsible disclosure 流程。

## 8. 风险

- 验证器不可靠 → 结果不可信。
- 二进制任务构建成本高 → 规模上不去。
- 污染控制难 → 分数虚高。
- 沙箱逃逸 → 安全风险。
- 与 CyberGym/AgentCyberRange 的差异不够显著 → 创新性不足。

## 相关页面

- [[research/research-map]]
- [[research/open-questions]]
- [[research/methodology-notes]]
- [[comparisons/benchmark-landscape]]
- [[binary-closed-book-landscape]]
- [[slyp]]
- [[veritas]]
- [[vulbinllm]]
- [[aixcc]]
- [[sec-bench]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/articles/Fudan_AgentCyberRange_Article.md`
- `raw/papers/Slyp_arXiv_2605.05000v1.pdf`
- `raw/papers/Veritas_arXiv_2605.15097v3.pdf`
- `raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf`
- `raw/papers/SEC-bench_arXiv_2506.11791v1.pdf`
- `raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf`
- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`
- `raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf`
