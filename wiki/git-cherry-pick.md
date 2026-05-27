---
title: git cherry-pick
tags: [git, cherry-pick, commit]
created: 2026-05-22
source: raw/notes/git/git_doc_3
---

# git cherry-pick

把指定commit向当前HEAD提交[^1]。

## 使用示例

```bash
# 假设: A-->B-->C-->D-->E
git checkout C        # HEAD指向C
git cherry-pick E     # 把E向C提交

# 结果:
#           /-- master
# A-->B-->C-->D-->E
#       \
#        E <--HEAD (分离头指针状态)
```

## 修改author

```bash
git cherry-pick id -e  # 修改commit签名内容
```

## 相关概念

- [[Git基础操作]]
- [[Git分支与合并]]
- [[git rebase合并提交]]

[^1]: raw/notes/git/git_doc_3