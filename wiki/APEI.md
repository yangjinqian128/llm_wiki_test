---
title: APEI
aliases: [ACPI Platform Error Interface]
tags: [acpi, error, ras]
---

# APEI

ACPI平台错误接口，记录和报告硬件错误信息。

## RAS场景

固件优先模式下，固件将RAS错误信息写入APEI表，OS读取处理。

参见[[RAS]]、[[固件优先]]。

## 相关概念

- [[RAS]] - 硬件错误报告机制
- [[固件优先]] - Firmware First处理模式
- [[ACPI表结构解析]] - ACPI表概述
- [[内核调试]] - 错误分析