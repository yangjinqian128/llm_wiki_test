---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization]
last_compiled: 2026-05-22
---

# eventfd

[[eventfd]]是VFIO中断路由的关键机制[^1]。

## 中断路由流程

VF中断到达[[VFIO]] MSI/MSI-X部分[^1]。

通过[[eventfd]]发送给[[KVM]] ITS[^1]。

## 建立关系

QEMU VFIO代码预先建立eventfd关系[^1]。

VF中断 → VFIO → eventfd → [[KVM]] ITS[^1]

## SR-IOV应用

VF直通场景使用eventfd建立中断路径[^1]。

## 相关概念

- [[VFIO]] - 设备直通框架
- [[SR-IOV]] - 单根IO虚拟化
- [[KVM]] - 内核虚拟化

[^1]: 来源：raw/notes/virt/sr-iov_note