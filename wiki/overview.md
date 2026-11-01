---
title: 领域总览与阅读地图
type: overview
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/articles/Fudan_AgentCyberRange_Article.md
  - raw/official/BerkeleyRDI_Frontier_AI_Cybersecurity.txt
  - raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
tags: [overview, reading-map, synthesis]
---

# 领域总览与阅读地图

## 这个 Wiki 在回答什么

> AI Agent 到底有多强的网络安全能力？我们该如何**可信地测量**它？尤其是：当只给一个二进制、不给源码和漏洞信息时，它还能不能自主发现并利用漏洞？

## 两条主线

### 主线 A：开卷式漏洞复现（CyberGym）
给漏洞描述 + 源码，要求生成 PoC；sanitizer 双版本验证。规模大、客观、便宜，但只测单点复现。

### 主线 B：闭卷式端到端攻击（AgentCyberRange）
只给入口 URL/IP，要求自主发现攻击面、利用、横向移动；用 marker/verifier 判分。真实、端到端，但成本高、方差大、规模小。

两者不是替代关系，而是覆盖不同能力层。见 [[comparisons/cybergym-vs-agentcyberrange]]。

### 主线 C：二进制闭卷（2023–2026 前沿）
只给 stripped binary（或 COTS 二进制），不给源码/漏洞描述，要求检测、推理、触发或生成 PoC。[[vulbinllm]]、[[veritas]] 做检测/推理，[[slyp]] 做到 debugger-verified PoC；[[sec-bench]]、[[aixcc]] 提供自动造题与 Oracle 方法。仍缺统一、公开、跨架构的基准。见 [[binary-closed-book-landscape]]。

## 三个核心对象

| 对象 | 角色 | 一句话 |
|---|---|---|
| [[cybergym]] | 基准 | 1,507 个真实漏洞的 PoC 复现 |
| [[agentcyberrange]] | 基准 + 靶场 | Web 打点 + 内网后渗透的闭卷评测 |
| [[cage]] | 基础设施 | 跑 Agent × benchmark 的通用执行框架 |

## 五个核心张力

1. **真实 vs 可控**：越真实的靶场越难复现、越难隔离。
2. **信息 vs 难度**：给多少提示，决定了测的是「发现」还是「利用」。
3. **单点 vs 端到端**：复现一个漏洞 ≠ 打穿一个内网。
4. **成功率 vs 成本**：无限预算刷分没有运营意义。
5. **能力测量 vs 评测安全**：评测环境本身可能被逃逸（Anthropic 事故）。

对应页面：[[open-book-vs-closed-book]]、[[difficulty-levels]]、[[cost-aware-evaluation]]、[[evaluation-isolation]]、[[benchmark-validity]]。

## 当前判断（含不确定性）

- CyberGym 论文的 22% 是受控设置下的结果；排行榜 95%+ 是另一套设置。**不能直接比较**。
- AgentCyberRange 论文的规模（110/15/8/156）与团队文章（近 500/25/30/近 400）是不同口径。**引用必须注明**。
- 二进制闭卷（只给二进制、不给源码/描述）已从「完全空白」推进到「领域专用可用」：[[slyp]] 在 COTS Windows 二进制上发现 28 个 0day、16 个 CVE；[[veritas]] 在 stripped binary 上 90% recall。但仍没有统一、公开、跨架构的基准。这是研究机会，也是本库的项目页：[[projects/binary-closed-book-eval]]。
- AIxCC 决赛官方口径是 63 个合成漏洞、54 发现、43 修补（早期 70 已更正）；CVE-Bench 是 40 个 CVE、zero-day 10% / one-day 12.5–13%。
- 评测环境隔离不是工程细节，而是安全底线；Anthropic 的三起事故证明「模拟环境」也可能造成真实伤害。

## 阅读路径

### 路径 1：快速了解（30 分钟）
1. [[cybergym]] → [[agentcyberrange]] → [[comparisons/cybergym-vs-agentcyberrange]]
2. [[open-book-vs-closed-book]]
3. [[benchmark-validity]]

### 路径 2：做研究（半天）
1. [[binary-closed-book-guide]]（作者总纲：信息契约、Oracle、任务谱系）
2. [[research/research-map]]
3. [[research/open-questions]]
4. [[research/methodology-notes]]
5. [[projects/binary-closed-book-eval]]
6. 回读 [[cybergym-paper]]、[[agentcyberrange-paper]]

### 路径 3：做工程（半天）
1. [[cage]] → [[cage-docs]]
2. [[cybergym-repo-and-site]]
3. [[evaluation-isolation]]
4. [[reproducibility]]
5. [[uk-aisi-inspect-cybergym]]

### 路径 4：看行业叙事
1. [[fudan-sslab]] → [[fudan-and-wechat-articles]]
2. [[anthropic]]
3. [[berkeley-frontier-ai-report]]

### 路径 5：看 2023–2026 前沿（半天）
1. [[binary-closed-book-landscape]]（先看版图与空白）
2. [[cve-bench]] → [[autopenbench]]（真实闭卷端到端）
3. [[slyp]] → [[veritas]] → [[vulbinllm]]（二进制检测/推理/利用）
4. [[aixcc]] → [[sec-bench]]（Oracle 与自动造题）
5. [[cybench]] → [[nyu-ctf-bench]] → [[enigma]]（CTF 与工具接口）

## 相关页面

- [[index]]
- [[log]]
- [[research/research-map]]
- [[binary-closed-book-landscape]]
- [[binary-closed-book-eval]]

## 来源

- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/articles/Fudan_AgentCyberRange_Article.md`
- `raw/official/BerkeleyRDI_Frontier_AI_Cybersecurity.txt`
- `raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf`
- `raw/papers/Cybench_arXiv_2408.08926v4.pdf`
- `raw/papers/Slyp_arXiv_2605.05000v1.pdf`
- `raw/papers/Veritas_arXiv_2605.15097v3.pdf`
- `raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf`
- `raw/papers/SEC-bench_arXiv_2506.11791v1.pdf`
