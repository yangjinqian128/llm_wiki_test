---
title: RST文档
aliases: [ReStructuredText, Sphinx, rst]
tags: [文档, rst, sphinx]
source: raw/notes/rst_note
---

# RST文档

ReStructuredText 是文档标记语言，Sphinx 是文档生成工具。

## 基本逻辑

- **RST**：书写规则，类似 Markdown
- **Sphinx**：将 RST 转换为各种格式（HTML、PDF 等），组织多文档成书

## 快速开始

```bash
sphinx-quickstart  # 生成配置文档
```

## toctree

目录树定义：

```rst
.. toctree::
   :maxdepth: 2

   文件名1
   文件名2
```

`maxdepth` 表示显示的目录级数。

## 内部链接

定义锚点：

```rst
.. _xxxx:

sub-tile
--------
```

引用锚点：

```rst
:ref:`xxxx`
```

## 注意事项

定义内部链接时，锚点和标题之间需要空行。

## 相关概念

- [[Markdown]] - 另一种文档格式
- [[Git]] - 文档版本管理

---

**参考来源**：[rst_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/rst_note)