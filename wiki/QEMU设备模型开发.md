---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, device-model, AI]
last_compiled: 2026-05-22
---

# QEMU设备模型开发

基于 Linux 内核驱动开发 QEMU 设备模型的方法[^1]。

## 开发流程

1. **分析 Linux 驱动** - 提取 PCI 设备信息、寄存器布局[^1]
2. **生成芯片手册** - 记录寄存器、队列、中断[^1]
3. **创建 QEMU 骨架** - PCI 设备框架[^1]
4. **实现 MMIO** - 寄存器读写[^1]
5. **实现功能逻辑** - DMA、中断模拟[^1]
6. **调试测试** - dmatest 验证[^1]

## 从驱动提取关键信息

| 信息类型 | 位置 |
|----------|------|
| PCI 设备 ID | `pci_device_id` 表 |
| 寄存器偏移 | `#define` 宏 |
| SQE/CQE 结构 | `struct` 定义 |
| 初始化流程 | `probe()` 函数 |

## 常见陷阱

- MSI 用 `msi_notify()` 非 `pci_set_irq()`[^1]
- sq_head/sq_tail 驱动和硬件视角不同[^1]
- BAR0 需注册 dummy 避免 MSI 冲突[^1]

## QOM 结构

```c
struct MyDeviceState {
    DeviceState parent_obj;  /* Must be first */
    /* Properties */
    /* Internal state */
};
```

## 相关概念

- [[QOM]]
- [[MemoryRegion]]
- [[MemoryListener]]
- [[PCIe]]

[^1]: 来源: raw/notes/ai_gen/ai_qemu_dev/如何利用AI做QEMU模型开发.md