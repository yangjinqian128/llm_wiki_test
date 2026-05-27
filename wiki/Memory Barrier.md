---
title: Memory Barrier
tags: [CPU, 内存顺序, 并发]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md
---

# Memory Barrier

强制内存操作顺序的指令，防止CPU微架构优化导致的乱序行为。

## 为什么需要Barrier

CPU微架构引入Store Buffer和Invalid Queue导致乱序：
- Store buffer: Store操作缓存，不同地址store可乱序写入
- Invalid queue: Invalid请求缓存，load可能拿到旧值

多核系统下，乱序导致可见性问题：
- Core A的store顺序与Core B看到的顺序不一致
- 信息传递可能出错

## Barrier类型

### Store Barrier

保证store顺序：
- barrier前store先完成
- barrier后store后完成
- 其他核看到一致顺序

### Load Barrier

保证load顺序：
- 不使用投机load结果
- 清空invalid queue
- 重新执行barrier后load

### Full Barrier

同时保证store和load顺序：
- store barrier + load barrier效果
- 全内存同步

## MP场景

经典消息传递：
```
core 0:                  core 1:

store X1, [addr1]       load X3, [addr2]
store barrier           load barrier
store X2, [addr2]       load X4, [addr1]
```

无barrier问题：
- Core 0: store顺序可能乱序
- Core 1: load顺序可能乱序，拿到旧值

有barrier解决：
- Store barrier保证core 0顺序
- Load barrier保证core 1顺序
- 信息传递正确

## ARM指令

ARM64 barrier指令：
- `DMB`: Data Memory Barrier
- `DSB`: Data Synchronization Barrier
- `DMB ST`: Store barrier
- `DMB LD`: Load barrier

## Linux内核

Linux内存屏障：
- `smp_wmb()`: Write memory barrier
- `smp_rmb()`: Read memory barrier
- `smp_mb()`: Full memory barrier

## 相关概念

- [[Store Buffer]]
- [[Invalid Queue]]
- [[Store Barrier]]
- [[Load Barrier]]
- [[MESI协议]]
- [[ARM内存屏障]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md, Paul's article "Memory Barriers: a Hardware View for Software Hackers"