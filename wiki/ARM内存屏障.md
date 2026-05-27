---
title: ARM内存屏障
created: 2026-05-22
tags:
  - ARM
  - 内存序
  - barrier
  - 并发
---

# ARM内存屏障

## 概述

内存屏障指令强制约束内存访问顺序，是对弱内存序处理器基本内存序的补充。

## 经典案例

```
    core 1           core 2

    store data       check flag
    set flag         get stored data
```

问题：core1的store data和set flag可能乱序执行，导致core2看到flag置位但数据未准备好。

解决：在store data和set flag之间添加内存屏障。

## ARM64内存屏障指令

### 指令类型

| 指令 | 功能 |
|------|------|
| DMB | 只约束访存指令前后关系 |
| DSB | 在DMB基础上约束其他指令 |
| ISB | 最强屏障，清空流水线 |

### 作用域参数

| 参数 | 作用域 |
|------|--------|
| ISH | Inner Shareable域 |
| OSH | Outer Shareable域 |
| SY | 全系统 |

### 方向参数

| 参数 | 方向 |
|------|------|
| LD | 只对之前的load起作用 |
| ST | 只对之前的store起作用 |

## 作用域示意图

```
  +-----------------------------------------------------+ system
  | +-------------------------------------------+ outer |
  | | +---------------------------------+ inner |       |
  | | | +-----+   +-----+       +-----+ |       |       |
  | | | | CPU |   | CPU |  ...  | CPU | |       |       |
  | | | +--+--+   +--+--+       +--+--+ |       |       |
  | | +----+---------+-------------+----+       |       |
  | +--------------------+----------------------+       |
  +-----------------------------------------------------+
```

- Inner域：各CPU核心
- Outer域：CPU + DMA + Device
- System域：所有计算单元

## DSB vs ISB

- **DSB**：拦不住指令fetch和decode，特定情况下拦不住page table walk
- **ISB**：flush流水线上已fetch和decode的指令

参见[[RISC-V内存模型]]、[[内存可见性]]。

---

**参考来源**：[arm64_memory_order](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/ARM/arm64_memory_order)