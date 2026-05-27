---
title: Checkpoint恢复
tags: [CPU, 微架构, 寄存器重命名]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器rename基本逻辑.md
---

# Checkpoint恢复

寄存器重命名资源恢复方法，分支预测时保存状态，失败时直接恢复。

## 原理

投机执行分支指令时：
- 分支指令rename时保存当时的map表
- map表保存到checkpoint
- 分支预测失败时用checkpoint恢复map

示意图：
```
      +-------------------------------------------------------------------------+
ROB  |      D       |      C       |      B       |      jump    |             |
      |  a5->p6,p1   |   a0->p4,p3  |   a2->p2,p5  |   a0->p0,p7  |             |
      +-------------------------------------------------------------------------+
         ->--/
      input_insn

         map3 <---------  map2  <-------  map1  <---------  map
              --------->        ------->        --------->
```

每个跳转指令rename时保存map到checkpoint。

## 优点

- 恢复速度快：直接拷贝checkpoint到map
- 实现简单：无需反向遍历ROB
- 适合深度流水线：减少恢复延迟

## 缺点

- 硬件开销大：每个跳转都需保存map
- 存储需求大：map表可能很大
- checkpoint数量受限制：硬件资源有限

## 与Walk恢复对比

Walk恢复：
- 根据ROB信息反向一步步回退
- 无需额外checkpoint存储
- 恢复速度慢，需遍历多条指令

Checkpoint恢复：
- 一次拷贝完成恢复
- 需额外存储checkpoint
- 恢复速度快

## 实现要点

- checkpoint在分支rename时创建
- checkpoint保存完整map状态
- 分支确认正确时释放checkpoint
- 分支预测失败时用checkpoint恢复map

## 相关概念

- [[寄存器重命名]]
- [[Walk恢复]]
- [[投机执行]]
- [[ROB]]
- [[CPU-Flush]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器rename基本逻辑.md