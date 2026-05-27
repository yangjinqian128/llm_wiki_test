---
title: git remote仓库管理
tags: [git, remote, repository]
created: 2026-05-22
source: raw/notes/git/git_doc
---

# git remote仓库管理

管理远程仓库地址[^1]。

## 查看远程仓库

```bash
git remote -v
```

## 修改远程仓库

```bash
# 添加正确的地址
git remote add hilt ssh://git.xxx.git

# 删除错误的地址
git remote rm origin

# 重命名
git remote rename hilt origin
```

## 查看分支

```bash
git branch -r    # 显示远程分支
git branch       # 显示本地分支
git checkout branchname  # 切换分支
```

## 相关概念

- [[Git基础操作]]
- [[git pull详解]]
- [[git remote仓库管理]]

[^1]: raw/notes/git/git_doc