---
title: PLIC中断控制器
created: 2026-05-22
tags:
  - RISC-V
  - PLIC
  - 中断
---

# PLIC中断控制器

## 概述

PLIC(Platform-Level Interrupt Controller)是RISC-V的核外中断控制器，设计简单，只支持线中断。

## 拓扑结构

```
  +------+     +--------+    M-mode irq   +--------------+
  | uart |---->|  plic  |---------------->| riscv hart0  |
  +------+     |        |                 |              |
               |        |    S-mode irq   |              |
          ---->|        |---------------->|              |
               |        |----+            +--------------+
          ---->|        |---+| M-mode irq +--------------+
               |        |--+|+----------->| riscv hart1  |
          ---->|        |-+||  S-mode irq |              |
               +--------+ ||+------------>|              |
                          ||              +--------------+
                          ||  M-mode irq  +--------------+
                          |+------------->| riscv hartN  |
                          |   S-mode irq  |              |
                          +-------------->|              |
                                          +--------------+
```

每个hart有两个输入：M mode外部中断、S mode外部中断。

## PLIC架构

### Gateway

控制外设中断信号能否进入PLIC，每个外设只能pending一个中断。

### Core

PLIC具体处理逻辑，决定中断路由到哪个hart的哪个context。

## 配置寄存器

| 寄存器 | 说明 |
|--------|------|
| priority | 每个中断源的优先级 |
| pending bits | 每个中断源的pending状态 |
| enable | 中断源×target context的使能矩阵 |
| threshold | target context的优先级阈值 |
| claim | 读取中断ID |
| complete | 完成中断处理 |

## 中断流程

1. 外设中断信号进入PLIC
2. Gateway控制中断pending
3. Core根据priority/threshold/enable路由中断
4. hart读取claim获得中断ID
5. 处理完成后写complete，PLIC打开gateway

## Target Context

一个core上的M mode外部中断和S mode外部中断各为一个target context。

参见[[RISC-V特权级模型]]、[[ACLINT中断控制器]]。

---

**参考来源**：[riscv_plic](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/riscv/riscv_plic)