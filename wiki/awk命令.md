---
title: awk命令
tags: [shell, awk, text-processing]
created: 2026-05-22
source: raw/notes/shell/awk_note
---

# awk命令

文本处理工具[^1]。

## 基本用法

```bash
awk 'FS=: {printf "%-20s %-20s\n", $1, $2}' /etc/passwd
```

- FS指定字段分隔符[^1]
- $1, $2表示字段[^1]
- printf格式化输出[^1]

## 相关概念

- [[sed命令详解]]
- [[shell脚本]]

[^1]: raw/notes/shell/awk_note