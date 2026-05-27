---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization]
last_compiled: 2026-05-22
---

# VMID

[[VMID]]是虚拟机标识符[^1]。

## 基本用途

标识不同虚拟机的地址空间[^1]。

[[TLB]]区分不同虚拟机的翻译结果[^1]。

## KVM初始化

```
kvm_arch_init -> 检测vmid
```
[^1]

## RISC-V hgatp

`hgatp`寄存器配置第二级页表基地址[^1]。

包含VMID信息[^1]。

## TLB无效化

带VMID的DVM广播[^1]。

直接无效化相关TLB[^1]。

## 页表同步

guest页表变化需同步到物理[[SMMU]][^2]。

包含页表项和TLB[^2]。

## 相关概念

- [[TLB]] - 地址转换缓存
- [[Stage2翻译]] - 第二级地址翻译
- [[KVM]] - 内核虚拟化
- [[虚拟化]] - 虚拟化技术

[^1]: 来源：raw/notes/virt/linux_kvm_note
[^2]: 来源：raw/notes/virt/vSVA_logic