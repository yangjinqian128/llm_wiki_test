---
title: SMP屏障配对
aliases: [SMP barrier pairing]
tags: [并发编程, 内存模型]
---

# SMP屏障配对

SMP 场景下内存屏障必须配对使用才能保证正确性。

## 配对规则

写屏障与读屏障配对：
```
CPU 1                  CPU 2
WRITE_ONCE(a, 1);
<smp_wmb()>            x = READ_ONCE(b);
WRITE_ONCE(b, 2);      <smp_rmb()>
                       y = READ_ONCE(a);
```

若读到 b=2，则 a 必为 1。

## 常见配对

- 写屏障 + 读屏障
- 写屏障 + 地址依赖(隐式)
- ACQUIRE + RELEASE
- 通用屏障 + 通用屏障

## 写屏障一侧

```
STORE A = 1
STORE B = 2
<smp_wmb()>
STORE C = 3
```

其他组件看到 {A, B} 在 {C} 之前。

## 读屏障一侧

```
LOAD B
<smp_rmb()>
LOAD A
```

若 B 是最新值，则 A 也是最新值。

## 不配对后果

写侧屏障无法保证读侧顺序：
```
CPU 1                  CPU 2
WRITE_ONCE(a, 1);
<smp_wmb()>
WRITE_ONCE(b, 2);      x = READ_ONCE(b);
                       y = READ_ONCE(a);  // 可能读到 a=0!
```

读侧无屏障，可能乱序读到旧值。

## 经典场景

Message Passing，发布-订阅模式都需要配对屏障。

## 相关概念

- [[内存屏障]]: 排序机制
- [[ACQUIRE语义]]: 替代方案
- [[RELEASE语义]]: 替代方案

[^1]: 来源: raw/notes/linux/Linux内核memory-barrier文档学习