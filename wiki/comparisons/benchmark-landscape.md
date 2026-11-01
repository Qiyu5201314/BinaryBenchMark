---
title: 网络安全 Agent 基准版图
type: comparison
status: stable
created: 2026-10-29
updated: 2026-11-01
sources:
  - raw/github/CAGE_README.md
  - raw/papers/CyberGym_arXiv_2506.02548v3.pdf
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
  - raw/papers/CVE-Bench_arXiv_2503.17332v4.pdf
  - raw/papers/Cybench_arXiv_2408.08926v4.pdf
  - raw/papers/NYU_CTF_Bench_arXiv_2406.05590v3.pdf
  - raw/papers/EnIGMA_arXiv_2409.16165v3.pdf
  - raw/papers/AutoPenBench_arXiv_2410.03225v2.pdf
  - raw/papers/AIxCC_FuzzingBrain_arXiv_2509.07225v1.pdf
  - raw/papers/SEC-bench_arXiv_2506.11791v1.pdf
  - raw/papers/Slyp_arXiv_2605.05000v1.pdf
  - raw/papers/Veritas_arXiv_2605.15097v3.pdf
  - raw/papers/VulBinLLM_arXiv_2505.22010v1.pdf
  - raw/official/Anthropic_Investigating_Cybersecurity_Eval_Incidents.txt
  - raw/official/BerkeleyRDI_Frontier_AI_Cybersecurity.txt
  - raw/official/AIxCC_ARPAH_Results.txt
tags: [comparison, landscape, benchmark]
---

# 网络安全 Agent 基准版图

> 本页只收录本地素材中明确提到的基准；未读原文的只做定位，不展开细节。

## 一览表

| 基准 | 任务 | 目标形态 | 信息给多少 | 验证 | 定位 |
|---|---|---|---|---|---|
| [[cybergym]] | 漏洞 PoC 复现 | C/C++ 源码 + 二进制 | 开卷（描述+代码） | sanitizer 双版本 | 大规模、客观、单点能力 |
| [[agentcyberrange]] | Web 打点 + 内网后渗透 | 真实 Web 应用 + 企业内网 | 闭卷（入口） | marker / verifier / LLM judge | 端到端、真实、成本高 |
| [[cve-bench]] | 真实 Web CVE 利用 | Web 应用容器 | zero-day / one-day | 运行时证据（8 类结果） | 真实 CVE、多出口判据 |
| [[aixcc]] | 发现 + PoV + 补丁 | 源码 + fuzz harness | 开卷 | PoV + 补丁 + 配对 | 完整漏洞修复链路 |
| [[cybench]] | 40 个专业 CTF | CTF 服务 | 题面 + 起始文件 | flag + 子任务 | 专业 CTF、FST 难度锚 |
| [[nyu-ctf-bench]] | 200 个 CTF | CTF 服务 | 题面 + 文件 | flag | 规模、开发集/测试集 |
| [[enigma]] | CTF Agent + 工具接口 | CTF 服务 | 题面 + 工具 | flag + 预算 | ACI / harness 研究 |
| [[autopenbench]] | 33 个渗透任务 | 靶场 + 真实 CVE | 目标网络 + 任意 bash | flag + 里程碑 | 端到端渗透、部分分 |
| [[sec-bench]] | PoC + 补丁 | 源码仓库 | 源码 | PoC + 金补丁 | 自动造题流水线 |
| [[slyp]] | COM 竞态发现 + PoC | COTS Windows 二进制 | 二进制 + COM 元数据 | debugger-verified PoC | 二进制闭卷最接近 |
| [[veritas]] | OOB 漏洞推理 | stripped binary | 二进制 | 静态 witness + 运行时 oracle | 二进制推理 / grounding |
| [[vulbinllm]] | CWE 检测 | stripped binary | 二进制 | CWE 分类 | 二进制检测基线 |

来源：[[cage-docs]]、[[anthropic-incident-report]]、[[binary-closed-book-landscape]]。

> 旧版表中提到的 HackWorld、StrongREJECT、ExploitBench 未在本地展开：StrongREJECT 是安全对齐而非能力基准，ExploitBench 只在 Anthropic System Card 中出现。暂不列入主表；待补素材见 [[research/open-questions]]。

## 定位图（概念坐标）

```text
                 端到端 / 长链
                      ▲
                      │
        AgentCyberRange│
                      │
     AutoPenBench      │
                      │
  ────────────────────┼────────────────────▶ 信息越少 / 越闭卷
     CVEBench         │        CyberGym L0
     Cybench/NYU CTF  │        CyberGym L1-L3
     HackWorld        │
                      │
                 单点 / 技能
```

> [推断] 这张图是概念定位，不是严格测量。纵轴是任务粒度，横轴是信息量；实际基准可能同时落在多个位置。

## 覆盖空白

Berkeley RDI 报告指出：没有任何现有基准覆盖完整攻击链（侦察、恶意软件、漏洞发现、利用、横向移动、数据外泄）。AgentCyberRange 补了 Web + 内网两段，但仍不覆盖钓鱼、云、供应链、社工。

> [推断] 对「二进制闭卷」而言，2023–2026 的前沿把空白缩小了，但没有填上：
> - [[vulbinllm]]、[[veritas]] 证明 stripped binary 上可以做检测/推理，但不做利用；
> - [[slyp]] 做到「二进制 + debugger-verified PoC」，但只覆盖 Windows COM 竞态，且给了 COM 元数据；
> - [[cve-bench]]、[[autopenbench]] 做到闭卷端到端，但目标是 Web/网络，不是单个二进制；
> - [[aixcc]]、[[sec-bench]] 提供 Oracle 与自动造题方法，但输入是源码。
>
> 因此「只给 stripped binary + 只给入口 + 不给漏洞信息 + 要求发现并利用 + 可归因 Oracle」的组合，仍没有成熟、公开、可复现的统一基准。

详见 [[binary-closed-book-landscape]]、[[projects/binary-closed-book-eval]]、[[research/open-questions]]。

## 相关页面

- [[cybergym]]
- [[agentcyberrange]]
- [[binary-closed-book-landscape]]
- [[open-book-vs-closed-book]]
- [[benchmark-validity]]
- [[research/research-map]]

## 来源

- `raw/github/CAGE_README.md`
- `raw/papers/CyberGym_arXiv_2506.02548v3.pdf`
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
- `raw/official/Anthropic_Investigating_Cybersecurity_Eval_Incidents.txt`
- `raw/official/BerkeleyRDI_Frontier_AI_Cybersecurity.txt`
