---
title: OpenSBI设备驱动
aliases: [opensbi driver, OpenSBI设备模型]
tags: [OpenSBI, RISC-V, 驱动模型]
source: raw/notes/opensbi_设备驱动模型
---

# OpenSBI设备驱动

OpenSBI 设备驱动框架，支持多平台多设备。

## 设计要素

| 要素 | 说明 |
|------|------|
| 设备结构 | 描述硬件设备 |
| 驱动结构 | 描述驱动实现 |
| 发现机制 | 从 DTS 检测设备 |
| 匹配机制 | compatible 字段匹配 |
| 公共接口 | 统一对外 API |

## 模型示意

```
DTS ←── device struct ←─┐
           ↑           │
       match           │
           ↓           │
    driver struct ─────┘
           ↓
    common interfaces
```

## Platform 结构

全局结构体收集平台所有设备初始化入口和配置参数。

## 串口驱动示例

### 初始化流程

```
fdt_serial_init
  +-> 扫描 DTS stdout-path
  +-> 查找 fdt_serial_drivers 表
  +-> compatible 字段匹配驱动
  +-> 调用驱动 init 函数
  +-> 注册到 sbi_console_set_device
```

### 公共接口

```c
sbi_getc  +-> console_dev->console_getc
sbi_putc  +-> console_dev->console_putc
```

## 驱动注册

所有串口驱动静态放入 `fdt_serial_drivers` 表。

## 相关概念

- [[OpenSBI]] - OpenSBI 概述
- [[设备驱动]] - Linux 驱动模型
- [[DTS]] - 设备树
- [[RISC-V]] - RISC-V 架构

---

**参考来源**：[opensbi_设备驱动模型](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/opensbi_设备驱动模型)