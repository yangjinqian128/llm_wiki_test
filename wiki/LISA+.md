---
type: knowledge-node
tags: [core-concept, auto-generated, ARM, simulation]
last_compiled: 2026-05-22
---

# LISA+

LISA+ (Language for Instruction Set Architecture Plus) 是 ARM Fast Models 的组件建模语言[^1]。

## 定位

| 语言 | 定位 |
|------|------|
| Verilog/VHDL | RTL级，最精确但最慢 |
| SystemC | TLM级，折中 |
| **LISA+** | PV级，最快，功能正确 |

## 核心语法概念

| 概念 | 作用 |
|------|------|
| **properties** | 组件元信息（版本、类型） |
| **ports** | 对外接口（总线从端口、中断） |
| **parameters** | 可配置参数（base_addr） |
| **internal** | 内部状态（寄存器、标志位） |
| **behaviors** | 行为（init、reset） |

## 组件结构

```
component
  ├── properties (元信息)
  ├── ports (对外接口)
  │     ├── slave port<PVBus>
  │     └── master port<Signal>
  ├── parameters (可配置参数)
  ├── internal (内部状态)
  └── behaviors (行为)
       ├── init()
       ├── reset()
       └── read()/write()
```

## 与其他技术关系

- SimGen 编译器只认 `.lisa` 文件[^1]
- Fast Models 也支持包装 SystemC 组件[^1]

## 相关概念

- [[FastModels]]
- [[SystemC]]

[^1]: 来源: raw/notes/ai_gen/ARM_FastModels_私有外设开发指南.md