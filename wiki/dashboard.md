---
title: 工作台
type: dashboard
status: stable
created: 2026-10-29
updated: 2026-10-29
sources: []
tags: [dashboard, obsidian]
---

# 工作台

> 需要安装 **Dataview** 社区插件。没有安装时，下面的查询会显示为代码块。
> 日常入口：先看「待处理」，再看「最近更新」，最后处理「孤立页」。

## 待处理（draft / needs-review）

```dataview
TABLE type AS "类型", status AS "状态", updated AS "更新"
FROM "wiki"
WHERE (status = "draft" OR status = "needs-review")
  AND !contains(file.path, "_templates")
SORT updated DESC
```

## 最近更新

```dataview
TABLE type AS "类型", status AS "状态", file.mtime AS "修改时间"
FROM "wiki"
WHERE type != "index" AND type != "log" AND type != "dashboard"
  AND !contains(file.path, "_templates")
SORT file.mtime DESC
LIMIT 15
```

## 按类型统计

```dataview
TABLE length(rows) AS "页面数"
FROM "wiki"
WHERE type AND !contains(file.path, "_templates")
GROUP BY type
```

## 孤立页（无入链）

```dataview
LIST
FROM "wiki"
WHERE length(file.inlinks) = 0
  AND type != "index"
  AND type != "log"
  AND type != "dashboard"
  AND !contains(file.path, "_templates")
SORT file.name ASC
```

## 研究页与项目页

```dataview
LIST
FROM "wiki/research" OR "wiki/projects"
SORT file.name ASC
```

## 来源清单

```dataview
TABLE tags AS "标签", updated AS "更新"
FROM "wiki/sources"
SORT file.name ASC
```

## 快速操作

- 新建来源摘要：用 `_templates/source.md`，放到 `wiki/sources/`。
- 新建实体：用 `_templates/entity.md`，放到 `wiki/entities/`。
- 新建概念：用 `_templates/concept.md`，放到 `wiki/concepts/`。
- 每日记录：用 `_templates/daily.md`，建议放到 `wiki/journal/`。
- 新增素材后：让 Agent 按 `AGENTS.md` 执行 Ingest。

## 相关页面

- [[index]]
- [[overview]]
- [[open-questions]]
- [[log]]
