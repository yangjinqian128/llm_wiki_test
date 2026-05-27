---
title: git分支描述
tags: [git, branch, description]
created: 2026-05-22
source: raw/notes/git/git_doc_for_lost
---

# git分支描述

为分支添加说明信息[^1]。

## 添加描述

```bash
git branch --edit-description
```

说明文字被添加到.git/config[^1]。

## 查看描述

```bash
git config -l
```

如果有说明会显示[^1]。

## 相关概念

- [[Git基础操作]]
- [[Git分支与合并]]

[^1]: raw/notes/git/git_doc_for_lost