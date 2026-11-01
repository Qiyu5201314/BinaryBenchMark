---
title: NYU CTF Bench
type: entity
status: stable
created: 2026-11-01
updated: 2026-11-01
sources:
  - raw/papers/NYU_CTF_Bench_arXiv_2406.05590v3.pdf
  - raw/github/NYU_CTF_Bench_README.md
tags: [entity, benchmark, ctf, pwn, reverse-engineering]
---

# NYU CTF Bench

## 一句话

NYU 提出的 200 题 CTF Agent 基准：来自 CSAW 2017–2023，覆盖 6 类，含 55 题开发集；用统一 function-calling 框架评测 5 个 LLM。

**评测范式定位**：闭卷 CTF，pwn/rev 类别是二进制能力的代理，但题面与 flag 仍提供信息。

## 规模与设置

| 维度 | 数值 | 说明 |
|---|---|---|
| 挑战 | 200 | 6 类：crypto、forensics、pwn、rev、web、misc |
| 开发集 | 55 | 可当 train split，降低 agent 设计污染 |
| 模型 | 5 | GPT-3.5、GPT-4、Claude、Mixtral、LLaMA |
| 工具 | 8 | 含 pwntools、Ghidra 等，按类别启用 |
| 判分 | flag 精确匹配 | 同时统计 5 类失败原因 |

## 主要结果

| 模型 | 最佳类别表现 | 备注 |
|---|---|---|
| GPT-4 | rev 9.80%、pwn 7.69%、forensics 6.67%、web 5.26% | crypto/misc 为 0 |
| Claude | misc 8.33%、crypto 5.77%、rev 3.92%、pwn 2.56% | web 为 0 |
| GPT-3.5 | misc 12.5%、rev 5.88%、pwn 2.56% | crypto/web 为 0 |
| Mixtral / LLaMA | 全 0 | 100% 失败为答案错误 |

## 为什么重要

- **规模大、开源、可 docker 化**：适合做 agent 开发与回归测试。
- **开发集/测试集分离**：是 CTF 基准的必需设计。
- **失败原因分类**：能区分「放弃」「轮次超限」「token 超限」「答案错误」，比单一成功率有信息量。
- **pwn/rev 子集**：可用于观察二进制相关能力的早期水平。

## 局限

- CTF 题带提示，flag 结构固定，不等于真实漏洞利用。
- 单类别样本量不足以做细粒度结论。
- 工具集固定，探索能力受接口限制。
- 人类对照跨年份/赛制，不能严格比较。
- 不是二进制闭卷。

## 对二进制闭卷的启示

- 开发集/测试集分离必须保留：二进制样本制作也应区分「公开开发集」与「私有测试集」。
- 失败原因分类可直接借用：未定位 / 定位未触发 / 触发未利用 / 环境失败。
- 200 题规模说明「CTF 式闭卷」可以规模化，但二进制闭卷的构造成本更高，需要自动化（见 [[sec-bench]]）。

## 相关页面

- [[nyu-ctf-bench]]
- [[cybench]]
- [[enigma]]
- [[difficulty-levels]]
- [[benchmark-validity]]

## 来源

- [[nyu-ctf-bench-paper]]
- `raw/papers/NYU_CTF_Bench_arXiv_2406.05590v3.pdf`；https://arxiv.org/abs/2406.05590
- `raw/github/NYU_CTF_Bench_README.md`；https://github.com/NYU-LLM-CTF/NYU_CTF_Bench
