---
title: Git-Submodule
created: 2026-05-22
tags:
  - Git
  - submodule
  - 仓库管理
---

# Git-Submodule

## 概述

Git submodule允许一个仓库以代码方式完整包含另一个仓库，实现仓库间的模块化管理。

## 术语

- **superproject**：包含子仓库的主仓库
- **submodule/subproject**：被包含的子仓库

## 模型示意

```
                                +-----------+
                                | project_a |
                                +-----------+
                                 /
                                /
    +-----------------------+  /
    | superproject          | /
    |   |                   |/
    |   +----               |
    |   |                  /|
    |   +----             / |
    |   |                /  |
    |   +----- ./project_a  |
    +-----------------------+
```

Superproject只在配置信息中记录子仓库的URL/branch/commit。

## 使用方法

### 添加子仓库

```bash
git submodule add https://github.com/user/repo submodule/repo
```

执行后：
- 创建`.gitmodules`配置文件
- 克隆子仓库到指定目录

### 配置文件

`.gitmodules`内容：

```
[submodule "submodule/repo"]
    path = submodule/repo
    url = https://github.com/user/repo
```

### 查看状态

```bash
git submodule status
# 输出：e78b044... submodule/repo (heads/master)
```

### 更新子仓库

```bash
# 更新到远程最新提交
git submodule update --remote

# 从远程fetch并checkout最新提交
```

### 提交变更

```bash
git add .gitmodules submodule/repo/
git commit -s -m "add repo as a submodule"
```

提交内容只有一行，指向子仓库的具体提交：

```
Subproject commit e78b044...
```

## 应用场景

- **QEMU**：使用submodule管理firmware（如opensbi）
- 大型项目管理第三方依赖

参见[[Git基础操作]]、[[Git分支与合并]]。

---

**参考来源**：[git_submodule的使用](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/git/git_submodule的使用)