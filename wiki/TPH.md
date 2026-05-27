---
type: knowledge-node
tags: [core-concept, auto-generated, pci, cache]
last_compiled: 2026-05-22
---

# TPH

[[TPH]]（TLP Processing Hints）允许PCIe设备直接将数据写入CPU L3 Cache[^1]。

## 架构示意

```
CPU → L3 Cache → DDR
          ↑
         RP (stash table)
          ↑
         EP (TH/PH/ST)
```
[^1]

## TLP字段

| 字段 | 位数 | 功能 |
|------|------|------|
| TH | 1bit | TPH使能标志[^1] |
| PH | 2bit | 处理类型[^1] |
| ST | 8bit | 目标core/cluster[^1] |

## 软件接口

- **PR（Root Port）**：Device Capability 2的TPH Completer Supported位[^1]
- **EP（Endpoint）**：TPH Requester Capability、Control Register、ST Table[^1]
- **DMA Descriptor**：包含PH信息[^1]

## ST表位置

ST表可放在MSI-X表或TPH Capability中[^1]。

## Cache属性

需先使能Cacheable属性（CCA=1），再使能TPH[^1]。

可通过[[SMMU]]和CCA=1配置cacheable属性[^1]。

## 适用场景

- 数据将被CPU立即读取[^1]
- 减少DDR访问延迟[^1]

## 相关概念

- [[ATS]] - 地址翻译服务
- [[PRI]] - 页请求接口
- [[SMMU]] - 地址翻译

[^1]: 来源：raw/notes/pci/TPH_analysis