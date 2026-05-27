---
title: Git分支与合并
created: 2026-05-22
tags:
  - Git
  - 分支管理
  - 合并
---

# Git分支与合并

## 概述

本文记录Git分支管理、合并和高级操作技巧。

## Git对象模型

### 对象类型

```bash
# 查看对象类型
git cat-file -t <ID>
# 类型：commit, tree, blob...

# 查看对象内容
git cat-file -p <ID>
```

### 引用机制

- **HEAD**：保存当前分支路径，内容如`ref: refs/heads/master`
- **refs/heads/**：存放所有分支，文件内容是最新提交ID
- **refs/tags/**：存放标签

## Reset操作

### --hard

```bash
git reset --hard HEAD^
```

版本库、缓冲区、工作区全部切换到父提交，后续提交丢失（可用reflog恢复）。

### --soft

```bash
git reset --soft HEAD^
```

只切换版本库，回到commit之前的状态。

### 默认模式

```bash
git reset HEAD^
```

版本库和缓冲区切换，回到add之前的状态。

## 恢复操作

```bash
# 查看HEAD历史
git reflog show

# 恢复到指定版本
git reset --hard master@{n}
```

## Checkout操作

```bash
# 检出特定提交（分离HEAD状态）
git checkout <ID>

# 创建新分支
git checkout -b branch_name
```

## Cherry-pick

```bash
# 应用特定提交
git cherry-pick <ID>

# 修改commit签名
git cherry-pick <ID> -e
```

## Pull机制

```
git pull = git fetch + git merge
```

### 流程示例

```
# fetch前
repo A: A-->B-->C-->D
repo B: A-->B-->C (origin/master = master = C)

# fetch后
repo A: A-->B-->C-->D
repo B: A-->B-->C-->D (origin/master = D, master = C)

# merge后
repo B: A-->B-->C-->D (origin/master = master = D)
```

## 分支描述

```bash
# 添加分支描述
git branch --edit-description

# 查看描述
git config -l
```

## Rebase合并提交

```bash
# 合并最近3次提交
git rebase -i HEAD~3
```

参见[[Git基础操作]]。

---

**参考来源**：[git_doc_merge](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/git/git_doc_merge)