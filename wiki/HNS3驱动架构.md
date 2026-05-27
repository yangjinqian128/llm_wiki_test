---
title: HNS3驱动架构
tags: [network, driver, hns3, hisilicon]
created: 2026-05-22
source: raw/notes/net/hns3各个模块的关系
---

# HNS3驱动架构

HNS3是海思Hipxxx系列SoC的网络加速器驱动[^1]。

## 模块组成

- **hnae3**: 核心库，提供注册接口
- **hclge/hclgevf**: ae_algo实现（回调函数）
- **hns3**: PCIe设备驱动，NIC client
- **hns_roce_hw_v2**: RoCE client

## 架构关系

```
ae_dev (设备) <-> ae_algo (算法) <-> client (功能实例)
```

- ae_dev: 具有网络功能的设备[^1]
- client: ae_dev上的功能(NIC、RoCE)[^1]
- ae_algo: 设备驱动业务代码[^1]

## 数据结构

```c
pci_dev->priv(ae_dev)->priv(hclge_dev)->vport
                                          |-- nic (hnae3_handle)
                                          |-- roce (hnae3_handle)
```

## 注册流程

```c
hnae3_register_ae_dev()    // 注册设备
hnae3_register_ae_algo()   // 注册算法
hnae3_register_client()    // 注册client
```

## remove注意事项

hns3 remove时应：
1. 拒绝外部流量[^1]
2. 排空内部流量[^1]
3. 停住对应硬件[^1]

## 相关概念

- [[设备驱动]]
- [[内核模块]]
- [[RoCE测试]]

[^1]: raw/notes/net/hns3各个模块的关系