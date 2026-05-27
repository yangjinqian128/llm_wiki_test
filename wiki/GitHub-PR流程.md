---
title: GitHub PR流程
tags: [git, github, pull-request, opensource]
created: 2026-05-22
source: raw/notes/git/github_review_code
---

# GitHub PR流程

开源项目使用GitHub进行代码协作的流程[^1]。

## 开发者流程

```
主线repo master
      ↓ fork
开发者repo
      ↓ git push dev-branch
开发者本地repo (跟踪master，创建dev-branch)
      ↓ git pull request
主线repo master
```

1. Fork主线仓库到开发者GitHub[^1]
2. 本地跟踪master分支[^1]
3. 创建开发分支开发[^1]
4. Push到开发者GitHub[^1]
5. 发起Pull Request[^1]

## 维护者流程

```bash
# 拉取PR到本地review
git pull origin pull/<pull_request_id>/head

# 合入方式
# 1. Create a merge commit - 自动创建merge commit
# 2. Rebase and merge - 只合入patch
```

## 相关概念

- [[Git基础操作]]
- [[git remote仓库管理]]
- [[git send-email]]

[^1]: raw/notes/git/github_review_code