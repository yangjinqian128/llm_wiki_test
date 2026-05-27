---
type: knowledge-node
tags: [core-concept, auto-generated, iommu, virtualization, memory-management]
last_compiled: 2026-05-22
---

# SVA (Shared Virtual Addressing)

SVA（Shared Virtual Addressing）是 Linux 的一个特性，可以使设备直接使用进程里的虚拟地址[^1]。

## 核心功能

如果把设备操作映射到用户态，就可以在用户态让设备直接使用进程虚拟地址。设备可以使用进程虚拟地址做 DMA，如果设备功能强大（比如可以执行一段指令），设备甚至可以直接执行相应进程虚拟地址上的指令[^1]。

## 使用场景

最直观的使用场景是在用户态做 DMA[^1]：

```
用户态进程 malloc 一段内存，得到 va
→ 把 va 直接配置给设备的 DMA
→ 启动 DMA 向 va 对应的虚拟地址写/读数据
```

需要先把设备 DMA 相关的寄存器 mmap 到用户态，这样 DMA 操作在用户态就可以完成[^1]。

## 硬件基础

SVA 特性需要硬件支持[^1]：

1. **IOMMU/SMMU**：设备发起缺页的支持
2. **PASID**：PCI 设备和平台设备都需要支持

### ARM64 架构

在 ARM64 下，IOMMU 指的是 [[SMMU]]。设备有平台设备和 PCI 设备两种[^1]。

硬件关系：
- MMU 和 SMMU 使用相同的进程页表
- SMMU 使用 STE 表和 CD 表管理页表
  - STE 表：区分设备
  - CD 表：区分同一个设备上分配给不同进程使用的硬件资源

### PCI 设备缺页

需要 ATS、PRI 硬件特性支持[^1]：
- **ATS**（Address Translation Services）：设备中缓存 va 对应的 pa
- **PRI**（Page Request Interface）：设备发起缺页请求

### 平台设备缺页

平台设备无法用 PRI 发起缺页请求。[[SMMU]] 用 stall 模式支持平台设备的缺页[^1]：
- 内存访问请求到达 SMMU 后，如果没有 va 到 pa 的映射
- 硬件给 SMMU 的 event queue 里写信息
- Event queue 中断处理里做缺页处理
- 软件发送 RESUME CMD 继续 stall 的请求

## 数据结构

关键数据结构[^1]：

- `iommu_bond`：一个绑定关系的抽象
- `io_mm`：绑定了外设资源的进程地址空间
- `iommu_sva`：对外暴露的接口结构

## 关键接口

- `iommu_dev_enable_feature`：准备和 SVA 相关的 SMMU 管理结构[^1]
- `iommu_sva_bind_device`：将设备和 mm 绑定[^1]
- `iommu_sva_unbind_device`：解除绑定[^1]
- `iommu_sva_set_ops`：设置 mm_exit 回调[^1]
- `iommu_sva_get_pasid`：获取 PASID 数值[^1]

## 性能影响

基于 [[UACCE]] 和 ZIP 压缩解压缩引擎测试[^1]：

1. ZIP 性能从 1900MB/s 提升到 5000+MB/s
2. 大量 SMMU TLB miss 不会造成系统性能下降
3. IO 流程中有设备缺页，性能会下降到没有缺页流程的 1/6

## 相关主题

- [[SMMU]]
- [[IOMMU]]
- [[PASID]]
- [[UACCE]]
- [[虚拟化]]

---

[^1]: 来源于原始素材 raw/notes/sva_note。