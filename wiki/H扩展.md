---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization, riscv]
last_compiled: 2026-05-22
---

# H扩展

[[H扩展]]是RISC-V虚拟化扩展[^1]。

## 目标

硬件层面创建虚拟机[^1]。

支持各种类型虚拟化[^1]。

## V状态

隐式状态[^1]：
- V=0：CPU U/M/S状态不变，S处于HS状态[^1]
- V=1：U/S变成VU/VS状态[^1]

## V状态改变

1. V状态trap进HS：硬件置V=0[^1]
2. trap进M：硬件置V=0[^1]
3. sret返回：恢复之前V状态[^1]
4. mret返回：恢复之前V状态[^1]

## hstatus.SPV

表示"之前的V状态"[^1]。

sret/mret从SPV得到之前V状态[^1]。

## 新增寄存器

### Hypervisor寄存器

hstatus、hedeleg、hideleg、hvip、hip、hie、hgeip、hgeie、henvcfg、htimedelta、htval、htinst、hgatp[^1]。

### Guest寄存器

vsstatus、vsip、vsie、vstvec、vsscratch、vsepc、vscause、vstval、vsatp[^1]。

## hgatp

第二级页表基地址寄存器[^1]。

## hvip

给VS注入中断[^1]。

## 新增指令

### TLB指令

hfence.vvma、hfence.gvma[^1]。

### 访存指令

hlv.xxx、hsv.xxx[^1]。

带两级地址翻译的访存[^1]。

## 相关概念

- [[Stage2翻译]] - 第二级地址翻译
- [[VMID]] - 虚拟机标识
- [[KVM]] - RISC-V KVM

[^1]: 来源：raw/notes/virt/linux_kvm_note