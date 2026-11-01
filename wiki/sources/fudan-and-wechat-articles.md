---
title: 来源摘要：复旦文章与白泽公众号
type: source
status: stable
created: 2026-10-29
updated: 2026-10-29
sources:
  - raw/articles/Fudan_AgentCyberRange_Article.md
  - raw/articles/WeChat_CyberGym_AgentCyberRange.md
  - raw/articles/WeChat_CyberGym_AgentCyberRange.txt
tags: [source, article, narrative, fudan]
---

# 来源摘要：复旦文章与白泽公众号

## 元数据

| 文件 | 作者/来源 | 日期 | 原文 |
|---|---|---|---|
| `raw/articles/Fudan_AgentCyberRange_Article.md` | 朱育灏，复旦大学系统软件与安全实验室 | 2026-07-10 | https://security.fudan.edu.cn/news/content/2026-08-20-004/ |
| `raw/articles/WeChat_CyberGym_AgentCyberRange.md` | 复旦白泽战队 | 2026 | https://mp.weixin.qq.com/s/giumeYfpF0tmQ6CKK5KiCg |

> 复旦官网页面抓取时返回 504，Markdown 为据此前抓取正文整理；引用以原文链接为准。

## 核心叙事

1. **CyberGym 是「开卷考」**：给漏洞描述和代码，考察能否生成 PoC。
2. **真实攻击是「闭卷实战」**：只给目标，Agent 要自己完成信息搜集、入口打点、内网渗透、防御对抗。
3. **因此需要补「闭卷考」**：黑盒环境，只给入口，考察端到端自主攻击。
4. **结论**：Agent 已有一定攻击能力，但远未到让黑客失业；信息搜集不全、长攻击链停滞、攻击痕迹明显。

## 关键数字（文章口径）

| 指标 | 复旦文章 | 白泽公众号 |
|---|---|---|
| Web 应用 | 25 | 30 |
| 漏洞 | 近 500 个 0day/1day | 超 200 个 0day/1day |
| 内网 | 30 套 | 50+ |
| 靶机 | 近 400 | 超 300 |
| 最强系统 | GPT-5.5 + Codex | 同 |
| 完成率 | 不到 1/3 | — |
| 提示后 | 31.7% → 46.3% | — |

> 这些数字与论文（15 apps / 110 vulns / 8 ranges / 156 hosts）差异很大。**不要混用**。见 [[agentcyberrange]] 的「规模口径差异」。

## 白泽公众号补充信息

- CyberGym 分数被国内外不少模型/团队推到 **95 以上**。
- AgentCyberRange 发布后收到 **96 家单位**试用申请；OpenAI、Qwen、Kimi 等在跟进。
- 失败案例：Agent 拿下 Confluence 后没有继续在 Wiki 里找 GitLab 凭证，导致后续代码审计与 0day 利用无法展开。
- 强调「面向下一步攻击路径的判断」是人类专家与当前 Agent 的关键差距。

## 可引用原句

> "CyberGym 更像一场'网络安全开卷考'：给出漏洞描述和代码，考察智能体能否生成 PoC。但真实网络攻击更像'闭卷实战'。"

> "智能体已经拿下了 Confluence，却没有继续在 Wiki 里寻找 GitLab 凭证。"

> "当前性能最好的 AI 智能体（GPT-5.5 + Codex）仅完成了不到三分之一的任务。"

## 使用注意

- 这是**团队对外叙事/解读**，不是同行评审论文；数字与论文口径不同。
- 用于理解「为什么需要闭卷评测」很有价值；用于引用具体规模/结果时，优先论文。
- 公众号的 95+ 分说法与 CyberGym 提交指南「接近饱和」一致，但与论文 22% 不矛盾——设置不同。

## 相关页面

- [[fudan-sslab]]
- [[agentcyberrange]]
- [[open-book-vs-closed-book]]
- [[cybergym]]
- [[benchmark-validity]]

## 来源

- `raw/articles/Fudan_AgentCyberRange_Article.md`
- `raw/articles/WeChat_CyberGym_AgentCyberRange.md`
- `raw/articles/WeChat_CyberGym_AgentCyberRange.txt`
