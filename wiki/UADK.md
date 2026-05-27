---
title: UADK
aliases: [uadk, UACCE/UADK, 加速器框架]
tags: [加速器, UACCE, UADK, SVA]
source: raw/notes/uacce_summary
---

# UADK

User-space Accelerator Development Kit，基于 UACCE 的加速器软件栈。

## 整体方案

| 层级 | 内容 |
|------|------|
| 内核 | UACCE 驱动、加速器驱动、SVA/vSVA 补丁、QEMU 改动 |
| UADK | libwd 基础库、libwd_comp/libwd_crypto 算法库、驱动库 |
| Engine | UADK-openssl engine |

## 内核进展

- UACCE 驱动、加速器驱动、SVA 补丁已上传主线（v5.14-rc5）
- PCIe SMMU stall mode pci quirk 补丁待上传
- vSVA/QEMU 改动社区进展缓慢，block 在 iommu 用户态接口

## UADK 库

https://github.com/Linaro/uadk

特性：

- 兼容 no-sva 接口
- 补齐 1620/1630 算法
- 内存池、环境变量、polling 线程优化

## UADK-openssl Engine

https://github.com/Linaro/openssl-uadk

特性：

- 兼容 KAE no-sva 特性
- 补齐 1620/1630 算法

## 应用拓展

- Ceph 加速器优化
- DPDK 加速器集成

## 相关概念

- [[UACCE]] - 内核加速器框架
- [[UACCE fork]] - fork 功能设计
- [[SVA]] - Shared Virtual Addressing
- [[PASID]] - Process Address Space ID

---

**参考来源**：[uacce_summary](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/uacce_summary)