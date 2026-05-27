---
title: Store Barrier
tags: [CPU, 内存顺序, Barrier]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md
---

# Store Barrier

强制store操作按顺序完成的内存屏障指令，保证store操作对其他核可见的顺序。

## 作用

Store buffer存在导致不同地址store可乱序写入：
- Store A和Store B在store buffer
- 可能B先写入cache，A后写入
- 其他核看到的顺序与程序顺序不同

Store barrier保证：
- barrier前的store先完成
- barrier后的store后完成
- 其他核看到一致顺序

## 实现逻辑

Store barrier执行：
1. 检测到store barrier
2. 关闭store queue到store buffer通路
3. 持续等待store buffer里的请求完成
4. Store barrier提交
5. 恢复core向store buffer的请求通路

"请求完成"含义：
- 在多核系统下，指完成MESI一致性操作
- 例如：对share状态cache line写
- 向其他share该cache line的core发invalid消息
- 接收到所有应答

## 多核一致性

示意图：
```
+-------------+     +-------------+     +-------------+
| core 0      |     | core 1      |     | core 2      |
|             |     |             |     |             |
| store queue |     | store queue |     | store queue |
+-------------+     +-------------+     +-------------+
       v                   v                   v
+-------------+     +-------------+     +-------------+
| store buffer|     | store buffer|     | store buffer|
| A B C       |     |             |     |             |
+-------------+     +-------------+     +-------------+
        \
         -- invalid -------->---->-------
                   \                 \
   cache lines         cache lines         cache lines
        <---- ack --                    /
        <---------- ack ----------------
```

Core 0上store barrier：
- 关闭store queue到store buffer通路
- 把store buffer里的A、B、C完成
- 完成指其他core也看到一致数据

## 局限性

Store barrier保证本核两个store顺序，但不保证其他核看到顺序：
- 其他核上的指令执行是乱序的
- 其他核的load可能乱序执行
- 需要load barrier配合

经典MP场景：
```
core 0:                  core 1:

store X1, [addr1]       load X3, [addr2]
store barrier
store X2, [addr2]       load X4, [addr1]
```

Core 0的store barrier保证两个store顺序，但core 1第二条load可能乱序执行拿到addr1旧值。

需core 1两load间加load barrier。

## 相关概念

- [[Store Buffer]]
- [[Load Barrier]]
- [[Memory Barrier]]
- [[MESI协议]]
- [[Invalid Queue]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md