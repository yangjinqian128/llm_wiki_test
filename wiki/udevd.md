---
title: udevd
aliases: [udev守护进程]
tags: [kernel, device, udev]
---

# udevd

用户态设备管理守护进程，处理内核uevent并管理设备节点。

## 自动加载

参见[[模块自动加载]]，udevd检测设备并调用modprobe加载驱动。

## uevent

内核通过netlink发送uevent，udevd接收处理。

## 相关概念

- [[模块自动加载]] - udevd触发模块加载
- [[内核模块]] - 自动加载目标
- [[initramfs]] - udevd在initramfs中运行