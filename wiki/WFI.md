---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization, arm]
last_compiled: 2026-05-22
---

# WFI

[[WFI]]（Wait For Interrupt）是ARM等待指令[^1]。

## 虚拟化模拟

guest执行WFI触发trap到[[KVM]][^1]。

KVM模拟WFI逻辑[^1]。

## vCPU让出CPU

WFI模拟时KVM可能主动让出CPU[^1]。

vCPU线程被调度出去[^1]。

## WFx逻辑

KVM中WFI/WFE的基本逻辑[^1]。

## vCPU下线点

WFI模拟是KVM让出CPU的点之一[^1]。

## 相关概念

- [[vCPU调度]] - vCPU上下线
- [[KVM]] - 内核虚拟化
- [[半虚拟化]] - pv preempt

[^1]: 来源：raw/notes/virt/虚拟机被调度的基本逻辑