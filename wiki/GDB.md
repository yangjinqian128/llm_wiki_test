---
type: knowledge-node
tags: [core-concept, auto-generated, debug-tool, development]
last_compiled: 2026-05-22
---

GDB是GNU调试器，用于调试程序的执行流程和状态。

基本使用[^1]：
- `gdb program`：启动调试
- `set args`：设置程序参数
- `b function`：设置断点
- `r`：运行程序
- `bt`：查看调用栈
- `c`：继续执行

断点类型[^2]：
- `b file:line`：文件+行号
- `b function`：函数名
- `b file:function`：文件+函数
- `b xxx if cond`：条件断点

单步执行[^3]：
- `n`：单步(不进入函数)
- `s`：单步(进入函数)
- `finish`：退出函数
- `u`：执行完循环

查看数据[^4]：
- `p variable`：打印变量
- `p *pointer`：打印指针指向数据
- `p array@size`：打印数组
- `p/x`：十六进制格式

watchpoint[^5]：
- `watch expr`：监控表达式变化
- 变化时程序暂停，显示新旧值
- `watch p`监控指针变量
- `watch *p`监控指针指向地址

配置文件[^6]：
- .gdbinit可预置调试命令
- 放置于home目录或项目目录

[[调试技巧]]的核心工具。

相关概念包括[[调试技巧]]、[[断点]]、[[核心转储]]。

[^1]: 来源于原始素材 raw/notes/gdb_note 第12-60行。
[^2]: 来源于原始素材 raw/notes/gdb_note 第77-84行。
[^3]: 来源于原始素材 raw/notes/gdb_note 第90-92行。
[^4]: 来源于原始素材 raw/notes/gdb_note 第94-96行。
[^5]: 来源于原始素材 raw/notes/gdb_note 第98-100行。
[^6]: 来源于原始素材 raw/notes/gdb_note 第66-75行。