---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization]
last_compiled: 2026-05-22
---

# MMIO虚拟化

[[MMIO虚拟化]]处理设备MMIO访问[^1]。

## 触发机制

未对MMIO gpa做第二级映射[^1]。

第二级翻译触发缺页异常[^1]。

## 处理流程

```
gstage_page_fault -> emulate_load
  -> run->exit_reason = KVM_EXIT_MMIO
```
[^1]

KVM处理后退出虚拟机[^1]。

## 返回处理

下次vcpu run处理MMIO[^1]：

```
if (run->exit_reason == KVM_EXIT_MMIO)
    kvm_riscv_vcpu_mmio_return(vcpu, vcpu->run)
```
[^1]

## 异常PC

保存在sepc寄存器[^1]。

sret从sepc继续运行[^1]。

## QEMU处理

QEMU处理MMIO退出[^1]。

如网络IO，启动socket发送[^1]。

## 相关概念

- [[Stage2翻译]] - 第二级地址翻译
- [[KVM]] - 内核虚拟化

[^1]: 来源：raw/notes/virt/linux_kvm_note