---
title: sed命令详解
tags: [shell, sed, text-editing]
created: 2026-05-22
source: raw/notes/shell/sed_note
---

# sed命令详解

sed是面向数据流的编辑器[^1]。

## 基本用法

```bash
sed 's/haha/taoge/' test_file   # 替换第一个
sed -f sed_script test_file     # 用脚本处理
sed -e 's/this/a/; s/that/b/' test  # 多个模式
```

## s命令

```bash
s/old/new/       # 替换第一个
s/old/new/g      # 替换全部
s/old/new/2      # 替换第二个
s/old/new/p      # 输出替换的行
s/old/new/w file # 写入文件
```

## 寻址

```bash
2s/old/new/      # 第2行
1,3s/old/new/    # 1-3行
/wang/s/old/new/ # 含'wang'的行
```

## d命令(删除)

```bash
sed '1d' test     # 删除第1行
sed '1,3d' test   # 删除1-3行
sed '/wang/d' test # 删除含'wang'的行
```

## i/a/c命令

```bash
sed '2i new_line' test  # 第2行前插入
sed '2a new_line' test  # 第2行后附加
sed '2c new_line' test  # 替换第2行
```

## p命令(输出)

```bash
sed -n '/wang/p' test   # 输出含'wang'的行
sed -n '2,3p' test      # 输出2-3行
```

## w/r命令

```bash
sed '1,3w new_file' test   # 写入文件
sed '/wang/w new_file' test
sed '3r new' file          # 读入文件内容插入第3行后
```

## y命令(变换)

```bash
sed 'y/123/579/' test  # 1->5, 2->7, 3->9
```

## 相关概念

- [[awk命令]]
- [[shell脚本]]

[^1]: raw/notes/shell/sed_note