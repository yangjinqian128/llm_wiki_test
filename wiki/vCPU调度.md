---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization]
last_compiled: 2026-05-22
---

# vCPU调度

[[vCPU调度]]分析KVM虚拟化场景下vCPU上下线逻辑[^1]。

## 前提条件

分析禁止内核抢占的Linux系统[^1]。

## 调度时机

线程调度只发生在[^1]：
1. 内核态显示让出CPU[^1]
2. 内核态返回用户态的调度点[^1]

## vCPU退出原因

1. **指令trap**：触发trap到KVM模拟[^1]
2. **host中断**：中断打断guest执行[^1]

## KVM让出CPU点

1. [[WFI]]/[[WFE]]模拟[^1]
2. ioctl run进入guest前检查调度[^1]

```
xfer_to_guest_mode_handle_work -> schedule()
```
[^1]

## vCPU上线时机

1. vCPU初次运行[^1]
2. 被调度后再次执行[^1]
3. KVM显式wakeup[^1]

## vCPU下线时机

1. KVM主动调度让出CPU[^1]
2. KVM退出到qemu时被调度走[^1]

## kvm_vcpu_kick

使vCPU至少下线一下[^1]。

更新资源后kick vCPU重新加载[^1]。

## kick触发点

- vCPU power off[^1]
- vCPU suspend[^1]
- kvm_vm_ioctl_irq_line[^1]
- PMU模拟[^1]
- 中断注入[^1]
- GICD_CTLR[^1]
- vSGI直通[^1]

## API

- kvm_vcpu_kick[^1]
- kvm_vcpu_wake_up[^1]
- kvm_kick_many_cpus[^1]

## 相关概念

- [[KVM]] - 内核虚拟化
- [[虚拟化]] - 虚拟化技术
- [[进程管理]] - 进程调度

[^1]: 来源：raw/notes/virt/虚拟机被调度的基本逻辑