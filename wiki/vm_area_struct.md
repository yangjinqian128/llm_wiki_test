---
type: knowledge-node
tags: [core-concept, auto-generated, device-driver]
last_compiled: 2026-05-22
---

# vm_area_struct

[[vm_area_struct]]管理进程虚拟地址区域[^1]。

## 基本用途

描述一个连续的虚拟地址范围[^1]。

## 查看

```
cat /proc/pid/maps
```
[^1]

显示进程所有虚拟地址区域[^1]。

## mmap回调参数

mmap回调接收vm_area_struct[^1]。

包含[^1]：
- vm_start：起始虚拟地址[^1]
- vm_end：结束虚拟地址[^1]
- vm_flags：标志[^1]

## vm_flags

- VM_IO[^1]
- VM_LOCKED[^1]
- VM_DONTEXPAND[^1]
- VM_DONTDUMP[^1]

## 设备映射

映射设备后显示设备路径[^1]。

如`/dev/scull0`[^1]。

## 相关概念

- [[mmap设备]] - 设备内存映射

[^1]: 来源：raw/notes/LDD/scull_3