---
title: WAR违例
tags: [CPU, LSU, 内存顺序]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md
---

# WAR违例

Load/store指令间的地址依赖关系，WAR对相同地址是真依赖，不能乱序执行。

## 定义

WAR (Write After Read) 对相同地址：
- 先读后写
- 若先执行write再执行read，逻辑错误
- WAR是真依赖，不能消除

示例：
```
A:  load x0, (x1)	
    ...
B:  store x2, (x3)
```

若x1/x3值相同：
- A读地址，B写相同地址
- 若B先执行，A读到错误值
- 必须顺序执行A、B

## LSU处理

Store是顺序执行，load可投机执行：
- WAR不会发生违例（store按顺序执行）
- Load不能使用store buffer中比它年轻（标号更大）的store的输出

Load取数据时：
- 搜索处理器内部buffer (store buffer)
- 不使用store buffer中比load年轻的store
- 若store比load新，load从cache取数据

## 编号机制

处理器对LSU里的load/store做编号：
- 编号表示年龄顺序
- Load取数据时不用标号比load大的store的输出

示意图：
```
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+  old
  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
         ^              ^           ^        ^  \-------------/
     allocate          B           A      commit  retired
```

A是load，B是store且比A年轻。A取数据时不使用B的输出。

## 与寄存器WAR区别

寄存器WAR依赖：
- 假依赖，可通过寄存器重命名消除
- 例如：`add r3, r0, r2; add r0, r5, r4`
- 第二条add的r0输出可重命名为r7

Load/store地址WAR：
- 真依赖，不能消除
- 必须顺序执行

## 相关概念

- [[Load-Store单元]]
- [[Store Buffer]]
- [[RAW违例]]
- [[寄存器重命名]]
- [[流水线冒险]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md