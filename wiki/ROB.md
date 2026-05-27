---
title: ROB
aliases: [Reorder Buffer, 重排序缓冲]
tags: [hardware, cpu, rob]
source: raw/notes/cpu硬件/超标量处理器ROB的理解
---

# ROB

超标量处理器内部的重排序缓冲，FIFO队列结构，维护指令年龄顺序。

## 基本结构

```
young ← allocate                commit → old
+--------+--------+--------+--------+--------+
| insn_6 | insn_5 | insn_4 | insn_3 | insn_2 | insn_1 |
+--------+--------+--------+--------+--------+
```

- 指令顺序进入ROB（包括投机执行指令）
- 指令按顺序退休离开ROB

## 并行操作部件

| 部件 | 操作 |
|------|------|
| rename | allocate位置写入指令 |
| commit | 检测并退休指令 |
| flush | 更新allocate点（丢弃年轻指令） |

## Flush处理

分支预测错误时：
```
原: [insn_6 | insn_5 | insn_4 | ...]
Flush后: [       |       | insn_4 | ...]
新指令: [insn_8 | insn_7 | insn_4 | ...]
```

## 指令状态

- **allocate到commit**：处理器内执行的投机指令
- **commit到retired**：已退休，外部可见
- **commit位置**：待提交检查点

## 相关概念

- [[寄存器重命名]] - ROB保存映射信息
- [[CPU Flush]] - 更新allocate点
- [[超标量处理器]] - ROB应用场景
- [[指令发射]] - 从ROB分发指令