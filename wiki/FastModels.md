---
type: knowledge-node
tags: [core-concept, auto-generated, ARM, simulation]
last_compiled: 2026-05-22
---

# Fast Models

ARM Fast Models 是 ARM 官方的快速仿真平台，用于软硬件协同开发[^1]。

## 组成

| 安装包 | 说明 |
|--------|------|
| **Fast Models Tools** | 编译构建工具链 (SimGen, sgcanvas) |
| **Fast Models Portfolio** | 预编译 ARM IP 模型库 |

## 关键目录

```
FastModelsTools_<ver>/
  ├── bin/simgen        ← LISA+ 编译器
  ├── bin/sgcanvas      ← System Canvas GUI
  └── source_all.sh     ← 环境变量脚本

FastModelsPortfolio_<ver>/
  ├── examples/LISA/    ← 官方示例
  ├── lib/              ← 模型库
  └── include/pv/       ← PV 接口头文件
```

## 开发私有外设

使用 [[LISA+]] 语言描述组件行为[^1]：

1. 定义 ports、parameters
2. 实现 behaviors (read/write)
3. 通过 System Canvas 连线
4. SimGen 编译生成模型

## 许可证

- 需要 **Fast Models Tools License**[^1]
- FVP License 只能运行预构建平台

## 相关概念

- [[LISA+]]
- [[SystemC]]
- [[仿真]]

[^1]: 来源: raw/notes/ai_gen/ARM_FastModels_私有外设开发指南.md