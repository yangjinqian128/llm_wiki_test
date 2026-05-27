---
title: Tmux
aliases: [tmux, 终端复用器]
tags: [终端, tmux, 开发工具]
source: raw/notes/tmux_note
---

# Tmux

终端复用工具，支持多窗口、多会话，适合远程开发场景。

## 核心概念

| 概念 | 说明 |
|------|------|
| Session | 会话，可持久化 |
| Window | 窗口，一个会话可有多个窗口 |
| Pane | 面板，一个窗口可分割为多个面板 |

## Pane 操作

| 操作 | 说明 |
|------|------|
| `Ctrl+B+%` | 左右分割 |
| `Ctrl+B+"` | 上下分割 |
| `Ctrl+B+z` | 全屏/恢复当前面板 |

## Window 操作

| 操作 | 说明 |
|------|------|
| `Ctrl+B+c` | 创建新窗口 |

## Session 管理

| 命令 | 说明 |
|------|------|
| `tmux ls` | 列出会话 |
| `tmux a -t name` | 连接会话 |
| `tmux new -s name` | 创建命名会话 |

## 复制日志到文件

```bash
Ctrl+B+:
capture-pane -S -3000    # 复制3000行到buffer

Ctrl+B+:
save-buffer /path/file   # 保存buffer到文件
```

## Bash 配置

Tmux 默认使用 `.bash_profile`，若需 `.bashrc` 配置：

```bash
# 在 .bash_profile 中添加
source ~/.bashrc
```

解决 ls 等命令颜色输出问题。

## 相关概念

- [[Vim]] - 编辑器
- [[SSH]] - 远程连接
- [[终端]] - 伪终端概念

---

**参考来源**：[tmux_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/tmux_note)