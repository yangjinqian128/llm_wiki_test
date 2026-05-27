---
title: DMA
aliases: [Direct Memory Access]
tags: [hardware, dma, io]
---

# DMA

直接内存访问，设备直接读写内存无需CPU参与。

## DMA缓冲区

- [[一致性DMA映射]] - coherent DMA
- [[dma-buf]] - DMA缓冲区共享框架

## IOMMU保护

[[IOMMU]]为DMA提供地址翻译和权限保护。

## 相关概念

- [[IOMMU]] - DMA地址翻译与保护
- [[dma-buf]] - DMA缓冲区框架
- [[VFIO]] - 用户态DMA访问
- [[SVA]] - 共享虚拟地址DMA