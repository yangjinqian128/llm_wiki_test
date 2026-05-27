---
title: Load Barrier
tags: [CPU, 内存顺序, Barrier]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md
---

# Load Barrier

强制load操作按顺序执行的内存屏障，保证load不使用投机结果，重新执行后续load。

## 作用

Load可投机执行：
- 提前执行load指令
- 使用旧值（其他核已更新但本核还未处理invalid请求）
- 导致乱序行为

Load barrier保证：
- Barrier前的load使用正确值
- 不使用投机得到的load结果
- 重新执行barrier后的load指令

## 与Invalid Queue

Invalid queue存在时，load barrier需：
1. 清空invalid queue中的请求
2. 然后执行barrier后的load

清空invalid queue：
- 处理其他核发来的invalid cache请求
- 更新本核cache状态
- 保证后续load得到正确值

## 实现逻辑

Load barrier执行：
1. 清空invalid queue（处理其他核的invalid请求）
2. 不使用投机load结果
3. 重新执行barrier后的load指令
4. 使用正确的数据执行后续指令

## MP场景

经典消息传递场景：
```
core 0:                  core 1:

store X1, [addr1]       load X3, [addr2]
store barrier
store X2, [addr2]       load barrier
                        load X4, [addr1]
```

Core 0:
- Store addr1和addr2
- Store barrier保证顺序

Core 1:
- 先load addr2（检测X2值）
- Load barrier清空invalid queue，重新执行后续load
- 再load addr1（得到正确的X1值）

若无load barrier:
- Core 1第二条load可能乱序执行
- Load addr1拿到旧值（X1而非正确值）
- 信息传递错误

## 与Store Barrier配合

Store barrier + Load barrier:
- Store barrier保证本核store顺序
- Load barrier保证本核load顺序
- 配合保证跨核消息传递正确

## 相关概念

- [[Store Barrier]]
- [[Memory Barrier]]
- [[Invalid Queue]]
- [[Store Buffer]]
- [[MESI协议]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md