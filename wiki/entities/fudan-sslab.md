---
title: 复旦大学系统软件与安全实验室
type: entity
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/articles/Fudan_AgentCyberRange_Article.md
  - raw/articles/WeChat_CyberGym_AgentCyberRange.md
  - raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf
tags: [organization, fudan, agentcyberrange]
---

# 复旦大学系统软件与安全实验室

## 一句话

[[agentcyberrange]] 的提出者。核心主张：CyberGym 是「网络安全开卷考」，真实攻击是「闭卷实战」，因此需要只给入口、让 Agent 自主完成端到端攻击的评测。

## 主要观点

- 榜单高分不等于真实攻击能力。CyberGym 给出漏洞描述和代码，考察的是复现能力。
- 真实攻击只掌握有限信息（例如仅知 IP），需要自主发现攻击面、完成利用、再横向移动。
- 网络攻击是**开放式、状态持续演化**的长程任务：前一步的权限、凭证、网络可达性会改变后续攻击空间。
- 因此要补「闭卷考」：黑盒环境中只给目标入口，考察端到端自主攻击。

来源：[[fudan-and-wechat-articles]]。

## 相关工作

- [[agentcyberrange]]：评测平台与论文。
- [[webexploitbench]]、[[postexploitbench]]：两条赛道。
- [[cage]]：执行框架。

## 值得注意的表述

- 复旦文章：GPT-5.5 + Codex 完成不到 1/3 任务；给提示后 31.7% → 46.3%。
- 白泽公众号：CyberGym 分数被推到 95 以上，但认为这更像开卷考；AgentCyberRange 已有 96 家单位申请试用，OpenAI、Qwen、Kimi 等在跟进。

> [推断] 这些是团队对外叙事，用于论证「闭卷评测」的必要性；引用其数字时仍需与论文口径区分。详见 [[research/open-questions]]。

## 相关页面

- [[open-book-vs-closed-book]]
- [[cyber-range]]
- [[post-exploitation]]
- [[comparisons/cybergym-vs-agentcyberrange]]

## 来源

- `raw/articles/Fudan_AgentCyberRange_Article.md`；https://security.fudan.edu.cn/news/content/2026-08-20-004/
- `raw/articles/WeChat_CyberGym_AgentCyberRange.md`；https://mp.weixin.qq.com/s/giumeYfpF0tmQ6CKK5KiCg
- `raw/papers/AgentCyberRange_arXiv_2606.14295v2.pdf`
