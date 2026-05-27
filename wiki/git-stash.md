---
title: git stash
tags: [git, stash, temporary]
created: 2026-05-22
source: raw/notes/git/git_doc_merge
---

# git stash

临时保存工作区状态[^1]。

## 使用场景

临时切换到另一个分支时，保存当前工作[^1]。

## 基本命令

```bash
git stash    # 保存当前状态（不需要git add）
git stash pop # 回到原来的工作区
```

## 相关概念

- [[Git基础操作]]
- [[git reset详解]]
- [[Git分支与合并]]

[^1]: raw/notes/git/git_doc_merge