---
title: RAW违例
tags: [CPU, LSU, 内存顺序]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md
---

# RAW违例

Load指令投机执行时，访问了后续store指令将写入的地址，需flush后续指令。

## 定义

RAW (Read After Write) 对相同地址：
- Store指令写入地址
- Load指令读取地址
- 若load先于store执行，load拿到旧值

在LSU中，store是顺序执行，load可投机执行。若load投机执行但实际应等store：
- 检测到RAW违例
- Load及其依赖指令需flush

## 示例

```
A:  store x2, (x3)
    ...
B:  load x0, (x1)	
C:  add x4, x0, x5
```

若x1/x3值相同：
- B处load投机执行
- A处store执行时检测到RAW违例
- B、C及后续指令需flush

示意图：
```
  flush  <--------------+
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+  old
  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
         ^           ^  ^           ^        ^  \-------------/
      allocate       C  B           A      commit  retired
```

## 检测时机

Store执行时检查：
- 是否有访问相同地址的更年轻load已执行
- 若存在，检测到RAW违例

## Flush时机

两种选择：
- **立即flush**: 及时释放硬件资源，需额外检测电路
- **等到load提交**: 实现简单，但性能差（等待期间处理器空转）

立即flush需从CPU各部件找出flush指令，实现复杂。

等到load提交时，比load老的指令都已退休，流水线里都是需flush指令，flush逻辑简单。

## 恢复

RAW违例恢复：
- Flush掉load及后续指令
- 恢复硬件资源（ROB、物理寄存器、issue queue等）
- 重新执行load（等store完成后再执行）

## 相关概念

- [[Load-Store单元]]
- [[Store Buffer]]
- [[CPU-Flush]]
- [[WAR违例]]
- [[投机执行]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md