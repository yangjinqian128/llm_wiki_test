---
title: git checkout详解
tags: [git, checkout, HEAD]
created: 2026-05-22
source: raw/notes/git/git_doc_2
---

# git checkout详解

检出提交或分支[^1]。

## 检出特定提交

```bash
git checkout ID(C)
```

- 进入no branch状态[^1]
- HEAD指向具体ID而非分支[^1]
- 可查看代码做验证，但不能提交修改[^1]

## 创建分支

```bash
git checkout -b branch_name
```

## 分离头指针

```
           /-- master
A-->B-->C-->D
    \
     E  # checkout B后提交E
```

checkout master后E不可见，用reflog可找回[^1]。

## checkout改变HEAD内容

checkout改变HEAD指向，reset改变refs/heads内容[^1]。

## 相关概念

- [[Git基础操作]]
- [[git reset详解]]
- [[Git分支与合并]]

[^1]: raw/notes/git/git_doc_2