---
title: git reset详解
tags: [git, reset, HEAD]
created: 2026-05-22
source: raw/notes/git/git_doc_2
---

# git reset详解

git reset的三个模式[^1]。

## --hard模式

```bash
git reset --hard HEAD^
```

- 版本库、缓存区、工作区全部切到父提交[^1]
- D版本被丢弃（不显示）[^1]

## --soft模式

```bash
git reset --soft HEAD^
```

- 只把版本库切到父提交[^1]
- 回到git add/commit之前的状态[^1]

## 默认模式

```bash
git reset HEAD^
git reset HEAD      # 取消git add
git reset -- file   # 取消git add file
```

- 版本库、缓存区切到父提交[^1]
- 回到编辑后git add之前[^1]

## 恢复丢失提交

```bash
git reflog show
git reset --hard master@{n}
```

查看.git/logs/HEAD或用reflog找回[^1]。

## 相关概念

- [[Git基础操作]]
- [[git checkout详解]]
- [[Git分支与合并]]

[^1]: raw/notes/git/git_doc_2