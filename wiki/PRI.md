---
type: knowledge-node
tags: [core-concept, auto-generated, pci-iommu]
last_compiled: 2026-05-22
---

# PRI

[[PRI]]（Page Request Interface）依赖 [[ATS]]，允许设备向 [[IOMMU]] 发送缺页请求[^1]。

## 协议流程

1. 设备发送 Page Request Message（PRM）[^1]
2. [[IOMMU]] 申请物理页并建立页表[^1]
3. [[IOMMU]] 返回 Page Request Group Response（PRGRM）[^1]
4. 设备再发 [[ATS]] 请求获取 PA[^1]

## PRI Capability

定义在 PF 中[^1]：

- **Status Register**：response failure、stopped 等状态[^1]
- **Control Register**：enable、reset 控制[^1]
- **Outstanding Cap**：最大并发请求数[^1]
- **Outstanding Allocation**：实际使用的并发数[^1]

## 消息类型

- **PRM**：一组消息组成 group，last 标记结束[^1]
- **Stop Marker**：设备通知不再使用 PASID[^1]
- **PRGRM**：以 group 为单位返回处理结果[^1]

## 注意事项

收到 PRGRM 后需再发 [[ATS]] 请求[^1]。

PRI 翻译建立的页表可能变动[^1]。

[^1]: 来源：raw/notes/pci/PRI_analysis