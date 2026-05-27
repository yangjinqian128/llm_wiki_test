---
title: git patch制作
tags: [git, patch, format-patch]
created: 2026-05-22
source: raw/notes/git/git_doc
---

# git patch制作

生成代码补丁文件[^1]。

## format-patch命令

```bash
git format-patch -s -1   # 最近1次提交
git format-patch -s -2   # 最近2次提交
```

- `-s`: 加上签名项[^1]
- `-1`: 对最近一次提交生成patch

## 修改patch subject

```bash
git format-patch -s -2 --subject-prefix="PATCH RFC"
# 生成 [PATCH RFC 0/2], [PATCH RFC 1/2], [PATCH RFC 2/2]
```

## 克隆优化

```bash
git clone git://xxx.git --reference /path/to/kernel.git
```

重用已有仓库，提高下载速度[^1]。

## 相关概念

- [[git rebase合并提交]]
- [[git send-email]]
- [[git am冲突解决]]

[^1]: raw/notes/git/git_doc