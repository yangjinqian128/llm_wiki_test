---
title: Store Buffer
tags: [CPU, 微架构, 内存系统]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md
---

# Store Buffer

CPU内部的存储缓冲区，用于缓存待写入内存的store操作，提高store指令性能。

## 为什么需要Store Buffer

Store指令执行时写入cache或内存是慢速操作：
- 若处理器等待写完成，会阻塞流水线
- Store buffer缓存store操作，允许处理器继续执行
- Store异步写入存储器

## 状态分类

Store buffer每个位置有三种状态：
1. **no-complete**: store指令刚进入，地址和数据还在计算中
2. **complete**: store地址和数据都准备好，处于投机状态
3. **retired**: store已提交，处于架构状态，必须成功写入存储器

状态1/2是处理器内部状态，可能被flush掉；状态3是外部可见状态，必须完成。

## 与LSU的关系

Load-Store Unit包含：
- **Load Queue**: 无序容器，缓存load操作
- **Store Queue**: FIFO，缓存store操作
- **Store Buffer**: FIFO，缓存已提交的store

Store指令流程：
1. 投机执行时：地址和数据存到store queue/store buffer
2. 提交时：store进入retired状态（store buffer 2）
3. 异步写入：从store buffer写入cache/内存

## RAW违例检测

Store执行时需检查是否有访问相同地址的更年轻load已执行：
- 若存在，则检测到RAW违例
- load及其后续指令需被flush掉

Load取数据时不使用store buffer中比它年轻的store的输出。

## 多核一致性

多核系统下store buffer导致乱序行为：
- Core A的store进入store buffer
- 其他core可能看不到最新数据
- 需要通过MESI协议保证一致性

Store barrier保证前后两个store的顺序：
- 关闭store queue到store buffer通路
- 等待store buffer里的请求完成
- 所谓完成指完成了MESI协议的多核一致性操作

## 页表同步问题

当页表VA->PA映射变化时：
- 使用TLB无效化指令做同步
- 需flush store buffer里处于retired状态的对应store操作
- 否则后续store访问会出错

## 相关概念

- [[Load-Store单元]]
- [[LSU]]
- [[CPU-Flush]]
- [[Memory Barrier]]
- [[MESI协议]]
- [[WAR违例]]
- [[RAW违例]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md