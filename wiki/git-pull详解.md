---
title: git pull详解
tags: [git, pull, fetch, merge]
created: 2026-05-22
source: raw/notes/git/git_doc_4
---

# git pull详解

git pull = git fetch + git merge[^1]。

## 远程分支机制

```bash
git fetch   # 更新origin/master
git merge origin/master  # 合入本地master
```

本地有完整的远程分支拷贝(origin/master等)，fetch只更新这个拷贝[^1]。

## 分支配置

.git/config:
```
[remote "origin"]
    url = /path/to/repo
    fetch = +refs/heads/*:refs/remotes/origin/*

[branch "master"]
    remote = origin
    merge = refs/heads/master
```

- fetch配置远程到本地的映射[^1]
- branch配置pull时的merge目标[^1]

## 分支操作

```bash
git branch           # 本地分支
git branch -r        # 远程分支
git checkout origin/test -b local_branch  # 创建本地分支跟踪远程
```

## 相关概念

- [[Git基础操作]]
- [[Git分支与合并]]
- [[git remote仓库管理]]

[^1]: raw/notes/git/git_doc_4