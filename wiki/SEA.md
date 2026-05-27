---
title: SEA
aliases: [Synchronous External Abort]
tags: [arm64, exception, error]
---

# SEA

同步外部中止，由当前指令执行直接触发。

## 特性

- 异常返回地址指向触发异常的指令
- 常见原因：内存错误、设备访问错误、页表遍历错误

## 固件优先模式

参见[[固件优先]]、[[RAS]]。

## 内核处理

参见[[RAS]]中的Linux内核处理流程。

## 相关概念

- [[RAS]] - RAS扩展
- [[固件优先]] - Firmware First模式
- [[内核调试]] - crash分析