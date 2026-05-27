---
title: git rebase合并提交
tags: [git, rebase, commit]
created: 2026-05-22
source: raw/notes/git/git_doc
---

# git rebase合并提交

将多个中间版本合并成一个提交[^1]。

## 合合最近N次提交

```bash
git rebase -i HEAD~3
```

3表示把最近3次提交合并成一次[^1]。

## 修改commit message

```bash
git commit --amend
```

重写最近一次提交的log message[^1]。

## 相关概念

- [[Git基础操作]]
- [[git patch制作]]
- [[Git分支与合并]]

[^1]: raw/notes/git/git_doc