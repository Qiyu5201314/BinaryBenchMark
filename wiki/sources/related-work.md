---
title: 来源摘要：两篇相关研究
type: source
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/papers/related/Cost_Aware_Security_Agents_arXiv_2607.15263.pdf
  - raw/papers/related/Natural_Code_Transformation_CyberGym_arXiv_2610.14350v2.pdf
tags: [source, related-work, cost, robustness, code-transformation]
---

# 来源摘要：两篇相关研究

这两篇不是 CyberGym/AgentCyberRange 官方论文，而是围绕「如何更全面地评测安全 Agent」的方法/分析研究。引用时注意区分。

---

## 1. Beyond Success Rate: Cost-Aware Evaluation of Offensive and Defensive Security Agents

| 项 | 内容 |
|---|---|
| 作者 | Paul Kassianik, Blaine Nelson, Yaron Singer |
| 版本 | arXiv 2607.15263 |
| 本地文件 | `raw/papers/related/Cost_Aware_Security_Agents_arXiv_2607.15263.pdf` |

### 核心主张

- 安全 Agent 评测常给宽松预算，测峰值攻击能力；但真实运营中每一步都有成本。
- 应在**固定成本水平**比较模型，并分解 **inference spend** 与 **tool spend**。
- 攻击与防御的 scaling 规律不同：
  - **攻击型 CTF**：增加 test-time compute 通常有效；开源权重模型在成本上有竞争力。
  - **防御型 SOC 调查**：不完全随推理预算 scaling；更依赖有纪律的工具使用、遥测导航、选择性 enrichment。
- 建议安全 Agent 基准同时测经济效率与运营适配性。

### 对本研究的用处

- 为「二进制闭卷评测」的成本指标设计提供依据。
- 提醒：闭卷任务搜索空间更大，成本可能比成功率更能区分系统。
- 可借鉴「cost-success frontier」而不是单点分数。

相关页面：[[cost-aware-evaluation]]

---

## 2. Natural Code Transformation Confuses Agents in Vulnerability Analysis

| 项 | 内容 |
|---|---|
| 作者 | Jie Zhu, Ronik Bhaskar, Heather Zheng, Ben Y. Zhao, Kexin Pei（University of Chicago） |
| 版本 | arXiv 2610.14350v2 |
| 本地文件 | `raw/papers/related/Natural_Code_Transformation_CyberGym_arXiv_2610.14350v2.pdf` |

### 核心主张

- 一个 surprising finding：功能相同、漏洞相同的真实代码提交，Agent 的漏洞分析结果差异巨大。
- 这说明 Agent 对**代码形式**高度敏感，而不只是对漏洞语义敏感。
- 作者挖掘真实开发中反复出现的、保持功能不变的代码变换，构建语料，测试其对 Agent 漏洞分析的影响。
- 即使单一变换也能「混淆」前沿 Agent（如 Codex + GPT-6 Sol），降低漏洞发现成功率。
- 进一步研究变换组合、自适应攻击者能否逆转。

### 对本研究的用处

- **基准有效性威胁**：如果代码形式变化就能显著改变结果，那么基准分数可能测量的是「模式匹配」而非「漏洞理解」。
- **防御思路**：自然代码变换可以作为延缓攻击者的防御手段。
- **数据增强**：构建二进制闭卷基准时，可考虑代码/二进制形态的多样性，避免单一编译/混淆配置主导结果。

相关页面：[[benchmark-validity]]、[[research/open-questions]]

---

## 相关页面

- [[cost-aware-evaluation]]
- [[benchmark-validity]]
- [[research/methodology-notes]]

## 来源

- `raw/papers/related/Cost_Aware_Security_Agents_arXiv_2607.15263.pdf`
- `raw/papers/related/Natural_Code_Transformation_CyberGym_arXiv_2610.14350v2.pdf`
