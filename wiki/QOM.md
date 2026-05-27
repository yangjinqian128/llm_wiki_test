---
type: knowledge-node
tags: [core-concept, auto-generated, qemu]
last_compiled: 2026-05-22
---

# QOM

[[QOM]]（QEMU Object Model）是QEMU的面向对象模型[^1]。

## 面向对象实现

用C语言实现面向对象[^1]：
- **封装**：struct实现[^1]
- **继承**：struct包含[^1]
- **多态**：函数指针[^1]

## TypeInfo

class的描述[^1]。

注册到系统链表[^1]。

## 类注册

```
type_init(fn)
  -> module_init
    -> register_module_init
      -> e->init = fn
```
[^1]

## TypeInfo定义

```
TypeInfo edu_info = {
    .name          = TYPE_PCI_EDU_DEVICE,
    .parent        = TYPE_PCI_DEVICE,
    .instance_size = sizeof(EduState),
    .instance_init = edu_instance_init,
    .class_init    = edu_class_init,
    .interfaces    = interfaces,
};
```
[^1]

## 对象生成流程

```
main -> qemu_init
  -> module_call_init(MODULE_INIT_QOM)
    -> type_initialize 根据TypeInfo创建class
```
[^1]

## 设备创建

```
qdev_device_add
  -> qdev_new(driver)
    -> object_new(typename)
      -> object_new_with_type(ti)
  -> qdev_realize
```
[^1]

## Property

属性定义在对象hash表[^1]。

realized属性触发realize函数[^1]。

## 命令行配置

```
-device edu,dma_mask=0xffffff
```
[^1]

## interface

PCI/PCIe设备使用interface[^1]：
- INTERFACE_CONVENTIONAL_PCI_DEVICE[^1]
- INTERFACE_PCIE_DEVICE[^1]

## 相关概念

- [[虚拟化]] - QEMU虚拟化
- [[设备驱动]] - 设备模型

[^1]: 来源：raw/notes/virt/qemu_qom