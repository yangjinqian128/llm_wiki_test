---
title: git am冲突解决
tags: [git, conflict, patch]
created: 2026-05-22
source: raw/notes/git/git_am_conflict_resolv
---

# git am冲突解决

合入patch时冲突的解决技巧[^1]。

## 解决步骤

```bash
# 1. 尝试直接合入
git am patch

# 2. 有冲突时使用reject方式
cd code_root/
git apply --reject patch

# 3. 在.rej文件中找到冲突diff段，手动修改代码

# 4. 添加修改的文件
git add related_files

# 5. 继续am操作（自动打上commit log）
git am --resolved
```

## git apply说明

- 只看到文件，把patch的diff段拆出来合入[^1]
- 只合入当前目录下的diff段[^1]
- 在代码根目录执行最方便[^1]
- `--reject`把冲突段保存在.rej文件[^1]

## 相关概念

- [[git patch制作]]
- [[Git基础操作]]
- [[git rebase合并提交]]

[^1]: raw/notes/git/git_am_conflict_resolv