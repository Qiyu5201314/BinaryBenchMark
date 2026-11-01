---
title: 二进制闭卷前沿版图（2023–2026）
type: comparison
status: draft
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/papers/NYU_CTF_Bench_arXiv_2406.05590v3.pdf
  - raw/papers/EnIGMA_arXiv_2409.16165v3.pdf
  - raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf
  - raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
tags: [comparison, landscape, binary-closed-book, frontier]
---

# 二进制闭卷前沿版图（2023–2026）

> 本页是综合判断，不是任何单篇论文的结论。数字均带来源与口径，引用前请回对应 source 页。

## 1. 一张表看清位置

| 工作 | 信息契约 | 任务 | 判分 | 规模 | 最强结果 | 与「二进制闭卷」的距离 |
|---|---|---|---|---|---|---|
| [[cybergym]] | 源码 + 漏洞描述（L1） | PoC 复现 | 差分崩溃 + sanitizer | 1,507 漏洞 / 188 项目 | 22.0% | 开卷；不是二进制 |
| [[agentcyberrange]] | 仅入口 URL/IP | Web 打点 + 内网后渗透 | 运行时 marker | 110 漏洞 / 15 应用 / 8 内网 | 16.1% / 31.7% | 闭卷，但不是二进制 |
| [[cve-bench]] | 目标 URL + 账号（zero-day） | 真实 Web CVE 利用 | 运行时证据（8 类结果） | 40 CVE | 10% / 12.5–13% | 闭卷，但不是二进制 |
| [[aixcc]] | 源码 + fuzz harness | 发现 + PoV + 补丁 | PoV + 补丁 + 配对 | 63 合成漏洞 / 5,400 万行 | 54 发现 / 43 修补 | 开卷；但是完整链路 |
| [[cybench]] | CTF 题面 + 起始文件 | 40 个 CTF 任务 | flag + 子任务 | 40 任务 | 17.5% unguided | 含 pwn/rev，但有题面 |
| [[nyu-ctf-bench]] | CTF 题面 + 文件 | 200 个 CTF 任务 | flag | 200 任务 | GPT-4 rev 9.80% / pwn 7.69% | 含 pwn/rev，但有题面 |
| [[enigma]] | CTF 题面 + 工具接口 | 390 个 CTF 任务 | flag + 预算 | 390 挑战 | NYU 13.5% | 证明工具接口是变量 |
| [[autopenbench]] | 目标网络 + 任意 bash | 33 个渗透任务 | flag + 里程碑 | 33 任务 | 21% / 64% | 闭卷，但不是二进制 |
| [[sec-bench]] | 源码仓库 | PoC + 补丁 | PoC + 金补丁 | 200 CVE | 18.0% / 34.0% | 造题流水线可迁移 |
| [[vulbinllm]] | stripped binary | CWE 检测 | CWE 分类 | Juliet 20,000+ | F1 74.5–99.5% | 只检测，不触发/利用 |
| [[veritas]] | stripped binary | OOB 漏洞推理 | 静态 witness + 运行时 oracle | 20 真实案例 | 90% recall | 只检测，不利用 |
| [[slyp]] | COTS 二进制 + COM 元数据 | 竞态发现 + PoC | debugger-verified PoC | 20 对象 / 40 案例 | 0.973 F1；67.5% PoC | **最接近**，但领域专用 |

## 2. 四个梯队

### 梯队 A：真实闭卷端到端（但不是二进制）

代表：[[agentcyberrange]]、[[cve-bench]]、[[autopenbench]]。

共同点：只给入口或目标网络，要求 agent 自己发现攻击面并完成利用；判分用运行时证据。

对二进制闭卷的价值：
- 「闭卷」的操作化定义（信息契约、提示梯度）可以直接借用；
- 多出口判据、里程碑/部分分、zero-day/one-day 对照都可迁移；
- 但目标形态是 Web/服务/内网，不是单个二进制。

### 梯队 B：二进制闭卷的检测/推理（但没有统一基准）

代表：[[vulbinllm]]、[[veritas]]、[[slyp]]。

- VulBinLLM：stripped binary 的 CWE 分类；证明「反编译 + LLM」在合成数据上可行。
- Veritas：把「定位」升级为「可验证的声明」；静态 witness + 运行时 oracle，90% recall。
- Slyp：端到端发现 + debugger-verified PoC；真实生产 28 个漏洞、16 个 CVE。

**关键判断**：这三篇合起来说明「二进制闭卷」在技术上已经可行，但**评测上仍没有统一基准**：
- VulBinLLM/Veritas 只测检测，不测利用；
- Slyp 只测 Windows COM 竞态，且给了 COM 元数据与调试接口；
- 没有工作同时满足「stripped binary + 仅入口 + 发现 + 利用 + 可归因 Oracle + 公开排行榜」。

### 梯队 C：源码级完整链路（提供 Oracle 与造题方法）

代表：[[aixcc]]、[[sec-bench]]。

- AIxCC：PoV 作为补丁 oracle；多 fuzzer/多 agent 集成；7 个 CRS 开源。
- SEC-bench：自动构建「PoC + 金补丁」，$0.87/实例；证明造题可以流水线化。

对二进制闭卷的价值：**Oracle 与样本工程**。但输入契约不同（源码 + harness），不能直接当二进制分数。

### 梯队 D：CTF（二进制能力的代理）

代表：[[cybench]]、[[nyu-ctf-bench]]、[[enigma]]。

- Cybench：FST 难度锚 + 子任务；证明 scaffold 是混杂变量。
- NYU：200 题规模 + 开发集/测试集分离 + 失败原因分类。
- EnIGMA：交互式工具（gdb/nc）显著提升 pwn/rev；提出 soliloquizing。

对二进制闭卷的价值：**难度设计、部分分、工具接口、污染检测**。但 CTF 题面本身是提示，不是闭卷。

## 3. 六个可复用机制

| 机制 | 来源 | 怎么用到二进制闭卷 |
|---|---|---|
| FST 难度锚 | [[cybench]] | 用人类专家首次解出时间校准题目难度，替代主观标签 |
| 子任务 / 里程碑 | [[cybench]]、[[autopenbench]] | 把八级任务谱系做成阶段分，失败可归因 |
| 多出口判据 | [[cve-bench]] | 崩溃 / 控制流劫持 / 任意写 / 权限证据均可作为分层成功 |
| PoV 作为 oracle | [[aixcc]] | 用 PoV 验证补丁与利用；注意「抑制 PoV 的假补丁」 |
| 自动造题 | [[sec-bench]] | Preprocessor → Verifier → Evaluator 流水线；二进制需加版本矩阵 |
| 工具接口作为变量 | [[enigma]]、[[slyp]] | 给/不给 gdb、COM 检查、反编译工具，作为独立难度维度 |

## 4. 仍未解决的空白

1. **没有统一的二进制闭卷基准**：只给 stripped binary + 入口，要求发现并利用，且 Oracle 可归因。
2. **没有跨架构的 Oracle**：x86-64 之外的 ARM64/MIPS/RTOS 如何统一判分。
3. **没有「逆向税」的系统测量**：给符号 vs 不给符号的配对消融，公开工作里仍缺。
4. **没有抗污染的二进制语料**：公开 CVE 的 patch/PoC 可能被模型见过；需要回灌层与多变体。
5. **没有成本-成功率前沿**：二进制搜索空间大，token/时间成本可能爆炸。
6. **没有真实 COTS 的公开评测协议**：Slyp 是单团队结果，缺少可复现的排行榜。

## 5. 对本项目的建议

- **架构**：借鉴 [[slyp]] 的三类工具接口（探索 / 元数据 / 调试），但把「COM 元数据」换成通用的二进制元数据（导入表、节区、调用图）。
- **验证**：借鉴 [[veritas]] 的「静态 witness + 运行时 oracle」，把「声明」作为中间评分对象，而不只看最终崩溃。
- **样本**：借鉴 [[sec-bench]] 的自动化流水线 + [[binary-closed-book-guide]] 的 Level A/B 版本矩阵。
- **难度**：借鉴 [[cybench]] 的 FST + 子任务，以及 [[autopenbench]] 的阶段里程碑。
- **Oracle**：借鉴 [[aixcc]] 的 PoV 验证，但增加「补丁语义正确性」检查，防止假补丁。
- **报告**：必须同时报 harness、工具集、预算、Pass@k、方差、成本，否则分数不可比。

> [推断] 如果只能选一个「最接近的目标」，那是 Slyp 的形态（二进制 + 工具接口 + debugger-verified PoC）加上 Veritas 的 grounding（静态 witness + 运行时验证），再用 SEC-bench 的方式自动化造题。三者组合，正好覆盖「发现 → 证明 → 规模化」。

## 相关页面

- [[benchmark-landscape]]
- [[cybergym-vs-agentcyberrange]]
- [[open-book-vs-closed-book]]
- [[benchmark-validity]]
- [[difficulty-levels]]
- [[agent-harness]]
- [[binary-closed-book-eval]]
- [[research/open-questions]]

## 来源

见各 source 页；原始文件在 `raw/papers/`、`raw/github/`、`raw/official/`。
