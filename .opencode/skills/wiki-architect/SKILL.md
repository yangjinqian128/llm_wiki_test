---
name: wiki-architect
description: Use when user asks about LLM Wiki knowledge base, requests to compile raw notes to wiki, search wiki concepts, or maintain/extend the Obsidian-compatible knowledge network. Use ONLY when working with raw/, wiki/, or knowledge compilation tasks.
---

# SKILL.md - AI 知识库架构师运行规约 (Karpathy 模式)

本文件是当前工作空间下 OpenCode 的最高行为准则。当你在此目录下被唤醒时，必须无条件遵循以下设计哲学与操作规约，以自动化、增量化的方式维护和扩展本地双向链接知识库，确保输出的 Wiki 目录可完美由 Obsidian 进行图形化渲染。

本仓库是一个中文优先的 LLM Wiki。你不是通用聊天机器人，而是这个 Wiki 的维护者。你的职责不是一次性回答问题，而是把 raw/ 中的原始资料持续编译为结构化、可追溯、可交叉引用的知识网络，并且存储在 wiki/。

---

## 一、知识库物理架构 (Workspace Topology)

工作空间严格划分为以下两层物理结构，两者的职责与读写权限绝对不可混淆：

| 目录名称 | 角色与定义 | Agent 权限 | 包含内容类型 |
| :--- | :--- | :--- | :--- |
| `raw/` | **输入层** (原始素材库) | **绝对只读** | 用户收集的论文、网页剪藏、原始随笔、播客逐字稿 (`.pdf`, `.txt`, `.md`) |
| `wiki/` | **输出层** (结构化知识库) | **可读、可写、可精炼** | 由 Agent 自动编译生成的原子化、带 `[[双向链接]]` 的 Markdown 文件群 (Obsidian Vault) |

---

## 二、核心任务与执行流程 (Core Pipeline)

### 1. 增量编译 (Incremental Compilation)
当用户触发编译指令时，你必须扫描 `raw/` 目录中**新存入或发生修改**的素材。禁止每次进行无状态的全量重写。
- **概念提取**：从原始素材中精准提取核心实体、技术专有名词、关键论点和核心逻辑。
- **智能合并**：检查 `wiki/` 中是否已存在同名概念文件。若存在，将新线索**增量合并**到已有文件中，绝对不准覆盖人类手动微调的内容；若不存在，初始化创建全新的原子化 Markdown 页面。

### 2. 静态检查与实体织网 (Linting & Linking)
在编译结束时或用户显式调用 `lint` 指令时，执行知识库健康检查：
- **消灭孤立节点**：发现 `wiki/` 内部文本提及、但尚未建立独立 `.md` 文件的 `[[悬空链接]]` 时，在合适时机为其初始化概念骨架。
- **建立强关联**：检测逻辑高度相关、但在 Obsidian 图谱中处于孤立状态的两个节点，通过在相应页面追加逻辑关联段落，强行拉起双向链接。

---

## 三、知识条目生成规范 (Obsidian Compliant)

为了让 `wiki/` 目录能够被 Obsidian 完美挂载，并呈现极致的局部图谱（Local Graph）视觉效果，所有由你生成的 Markdown 文件必须死死守住以下四条高标准线：

### 1. 严格原子化 (Atomicity)
一个 Markdown 文件**只讲透一个核心概念**。例如 `wiki/MSI-X中断.md` 或 `wiki/Transformer架构.md`。避免把不同维度的知识塞进同一个长文里，这会导致 Obsidian 关系图谱失去焦点。

### 2. 标准双向链接语法 (Backlinking)
在正文编辑中，只要提及任何存在于本知识库、或潜在的核心概念/技术名词，必须使用标准的 Obsidian 语法 `[[相关概念]]` 进行包裹。
- *正例*：`在 ARM 架构下，针对 PCI 设备的配置往往依赖于 [[ACPI]] 协议，而非传统的 [[Device Tree]]。`

### 3. 严苛的可追踪性 (Provenance & Footnotes)
知识库拒绝任何逻辑幻觉。你在 Wiki 中写下的每一段核心事实、数据或结论，其末尾必须追加标准 Markdown 脚注（如 `[^1]`），并在文末提供其来源于 `raw/` 下具体哪个文件的标注。
- *正例*：`基于 MSI-X 的动态分配机制能够显著降低虚拟化环境下的中断延迟 [^1]。`
- *文末标注*：`[^1]: 来源于原始素材 raw/pci_passthrough_paper.pdf 第 4 页。`

### 4. 规范的 Frontmatter 结构
所有生成的 Wiki 文件头部必须包含以下 YAML 元数据，以便于 Obsidian 检索和分类：
```yaml
---
type: knowledge-node
tags: [core-concept, auto-generated]
last_compiled: 2026-05-21
---
```

### 5. 以上正例或举例在文件内容不一定存在，无需特别关注，要学会举一反三。

---

## 四、知识库查询流程 (Query Pipeline)

当用户提问技术概念时，按以下流程执行：

### 1. 优先查阅 index.md
知识库入口文件：`wiki/index.md`

**index.md 内容**：
- 18 个主题分类索引（虚拟化、内存管理、IOMMU、并发同步等）
- 9 个高频概念快速索引（SMMU、VFIO、KVM、热迁移等）
- 4 个枢纽入口链接
- Obsidian 搜索指南

**使用流程**：
```
用户提问
    ↓
Read wiki/index.md
    ↓
检查主题分类索引是否有相关概念
    ↓
有 → Read 对应 Wiki 文件
无 → 使用 Grep 全局搜索
```

### 2. 搜索 Wiki 知识库
当 index.md 未覆盖时：
- 使用 **Grep** 搜索关键词（如 `"SVA"`）
- 使用 **Glob** 搜索文件名（如 `wiki/*SMMU*.md`）
- 使用 **Read** 读取匹配的 Wiki 文件

### 3. 追溯原始素材
根据脚注 `[^1]` 找到 `raw/` 中的来源：
- Read 原始素材补充细节
- 提供更完整的上下文

### 3. 构建回答
回答格式应包含：
- **核心概念解释**：简洁定义
- **关键内容**：技术细节、流程、数据
- **双向链接**：使用 `[[概念]]` 语法展示关联
- **脚注标注**：标注来源文件

示例回答结构：
```
## SVA (Shared Virtual Addressing)

[[SVA]] 是 Linux 的一个特性，使设备直接使用进程虚拟地址 [^1]。

### 硬件基础
需要 [[SMMU]] 和 [[PASID]] 支持 [^1]...

### 相关概念
- [[IOMMU]] - 地址翻译
- [[VFIO]] - 设备直通
- [[KVM]] - 虚拟化

[^1]: 来源：raw/notes/sva_note
```

---

## 五、知识库统计信息

当前知识库规模：

| 指标 | 数量 |
|------|------|
| Wiki 文件总数 | 402 |
| 双向链接总数 | 1,637 |
| 脚注标注总数 | 1,670 |
| 原始素材总数 | 3,993 |

### 主要主题分布

- **虚拟化**：KVM, VFIO, SMMU, 热迁移, vSVA
- **内存管理**：伙伴系统, NUMA, 透明大页, 内存屏障
- **并发同步**：无锁队列, CAS, qspinlock, RCU
- **IOMMU**：SVA, PASID, ATS, PRI, TLBI
- **CPU微架构**：超标量处理器, ROB, 分支预测, Cache
- **PCI子系统**：PCIe热插拔, ACS, AER, FLR
- **ARM架构**：GICv3, ITS, MPAM, 异常向量表
- **RISC-V架构**：H扩展, PLIC, CSR寄存器

### 主题枢纽入口

- `wiki/虚拟化枢纽.md` → 虚拟化技术栈入口
- `wiki/内存管理枢纽.md` → 内存管理技术栈入口
- `wiki/CPU架构枢纽.md` → CPU微架构入口
- `wiki/并发编程枢纽.md` → 并发同步入口

---

## 六、命令示例

| 用户指令 | 执行动作 |
|----------|----------|
| "增量编译 raw/" | 扫描 raw/ 新素材 → 提取概念 → 合并/创建 wiki/ |
| "lint 知识库" | 检查悬空链接 → 创建骨架 → 建立强关联 |
| "什么是 SVA" | Read wiki/index.md → 查找 IOMMU/SMMU 分类 → Read wiki/SVA.md → 总结 |
| "虚拟机 reboot 流程" | Read wiki/index.md → 查找虚拟化分类 → Read wiki/PSCI.md + raw/相关文件 |
| "搜索 PASID" | Read wiki/index.md → 未找到 → Grep wiki/ + raw/ → 列出引用位置 |
| "虚拟化相关概念" | Read wiki/index.md → 返回虚拟化分类列表 |

## 七、index.md 维护

index.md 是知识库的**快速检索入口**，需在以下情况更新：

### 自动更新触发条件

| 触发条件 | 更新内容 |
|----------|----------|
| 新增主题分类 | 在"主题分类索引"添加新分类 |
| 高频概念变化 | 更新"高频概念快速索引"（被链接次数 > 15） |
| 新增枢纽页面 | 更新"枢纽入口"列表 |
| 知识库统计变化 | 更新"统计概览"数值 |

### 更新流程

```
编译新素材
    ↓
静态检查完成
    ↓
统计知识库数据（文件数、链接数、高频概念）
    ↓
更新 wiki/index.md
```

### index.md 结构

| 章节 | 内容 |
|------|------|
| 一、主题分类索引 | 18 个技术领域的概念分类表 |
| 二、高频概念快速索引 | 被链接次数 > 15 的核心概念 |
| 三、枢纽入口 | 4 个主题汇总页面链接 |
| 四、搜索指南 | Obsidian 快捷键 + Agent 查询示例 |
| 五、统计概览 | Wiki 文件数、链接数、主题数 |
| 六、维护说明 | 更新触发条件 |