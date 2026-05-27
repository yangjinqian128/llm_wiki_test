---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization]
last_compiled: 2026-05-22
---

# KVM

[[KVM]]（Kernel-based Virtual Machine）是 Linux 内核的虚拟化模块[^1]。

## 基本架构

[[KVM]] 入口在体系架构相关代码中（如 `arch/riscv/kvm/main.c`）[^1]。

`kvm_init` 创建 `/dev/kvm` 字符设备作为操作入口[^1]。

## 创建虚拟机

`KVM_CREATE_VM` ioctl 创建虚拟机[^1]。

返回的 fd 代表虚拟机，支持配置 CPU、内存等资源[^1]。

## 创建 vCPU

`KVM_CREATE_VCPU` ioctl 创建虚拟 CPU[^1]。

每个 vCPU 有独立的 fd，通过 `KVM_RUN` ioctl 投入运行[^1]。

## 内存配置

`KVM_SET_USER_MEMORY_REGION` ioctl 配置虚拟机内存[^1]。

涉及三个地址：host va、pa、guest gpa[^1]。

第二级翻译实现 gpa → pa 映射[^1]。

## Stage-2 页表管理

- [[Stage2大页拆分]] - 脏页跟踪触发拆分
- [[KVM_mmu_lock]] - 页表操作锁保护
- [[Contiguous PTE]] - TLB优化支持
- [[脏页跟踪]] - 热迁移核心机制

## ARM64 特性

- [[BBML2]] - 页表修改同步
- [[FPMR]] - FP8浮点寄存器支持

## 虚拟机退出

虚拟机退出由异常或中断触发[^1]。

MMIO 访问触发缺页异常，[[KVM]] 处理后退出[^1]。

`sepc` 保存退出时的 PC[^1]。

## 相关主题

### 虚拟化核心
- [[VFIO]] - 设备直通框架
- [[SMMU]] - 设备内存管理
- [[QOM]] - QEMU 对象模型
- [[MemoryRegion]] - QEMU 内存抽象

### ARM 特性
- [[Stage2翻译]] - 二阶段地址翻译
- [[VMID]] - 虚拟机标识
- [[RME]] - Realm 管理扩展

### RISC-V 支持
- [[RISC-V]] - RISC-V 架构
- [[SBI]] - RISC-V 监管者二进制接口

---
[^1]: 来源：raw/notes/virt/linux_kvm_note