---
name: knowledge-base
description: Use when the user asks about system programming topics covered in this knowledge base. Topics include Linux kernel (KVM, SMMU, memory management, IOMMU, VFIO, page tables), ARM64 architecture (virtualization, exceptions, SMMU), RISC-V, CPU microarchitecture, QEMU development, virtualization (migration, live migration), device drivers, DMA, and performance optimization. Trigger on keywords like "KVM", "ARM64", "SMMU", "migration", "QEMU", "page table", "IOMMU", "VFIO", "RISC-V", "DMA", "kernel", "virtualization", or when user wants to search/explore the knowledge base.
---

# Knowledge Base Navigation Skill

This skill helps navigate and retrieve information from a technical knowledge base focused on systems programming.

## Primary Knowledge Source: AI Directory

**IMPORTANT**: The AI/ directory is the primary organized knowledge source with structured indexing.

### AI/ Directory Structure

The AI directory contains organized, indexed technical documents:

- **01-virtualization/** - KVM/ARM64, QEMU, Migration, VFIO
- **02-architecture/** - ARM, RISC-V, CPU Microarch, RAS
- **03-kernel/** - Memory Management, Scheduler, Drivers, Sync
- **04-hardware/** - PCI/PCIe, SMMU/IOMMU, DMA, Firmware, ACPI
- **05-development/** - Git, Build Systems, Debug, Perf
- **06-programming/** - C/C++, Python, Bash/Shell, Algorithms

Each subdirectory has README.md with topic index and core document list.

### AI/index.json Quick Reference

The `AI/index.json` file provides fast keyword-based search:

```json
{
  "search_index": {
    "KVM": ["path1.md", "path2.md"],
    "SMMU": ["path1.md", "path2.md"],
    ...
  }
}
```

**Usage**: Check index.json first for keyword matches, then read specific documents.

## Knowledge Base Structure

### Root Directory (~150 files)
Miscellaneous technical notes on various topics:
- **Linux Kernel**: `memory_note`, `kthread_note`, `workqueue_note`, `lock_usage`, `cpuhp_note`, `ftrace_note`
- **Device Drivers**: `dma_buf_note`, `dma_coherent_note`, `gpio_irq`, `uacce_fork`, `uacce_summary`
- **System Tools**: `gdb_note`, `strace_note`, `perf/`, `ftrace_func`, `vim_note`
- **Build Systems**: `make_note`, `cmake_note`, `meson_note`, `autoconfig_automake`
- **Programming**: `C_C++/`, `python/`, `APUE/`, `algorithm/`, `bash/`, `shell/`
- **Hardware**: `pci/`, `iommu/`, `acpi/`, `firmware/`
- **Performance**: `perf/`, `ebpf_to_get_duration_dist`, `memory_tools`
- **Debug**: `debug/`, `RAS/`
- **Misc**: `docker_note`, `git/`, `ssh_note`, `tmux_note`, `vim_note`

### ai_gen/ (26 files)
AI-assisted technical documentation:
- **KVM/ARM64 Virtualization**: 
  - `KVM_Stage2_PageTable_Walk_Summary.md` - Stage-2 page table operations
  - `ARM64_KVM_Stage2_2M大页拆分场景.md` - 2MB huge page splitting
  - `ARM64_KVM_Stage2_Contig_Hugetlb_设计文档.md` - Contiguous hugetlb design
  - `KVM_ARM64_mmu_lock_write_lock_scenarios.md` - MMU lock scenarios
  - `KVM_FPMR_Save_Restore_Patch_Analysis.md` - FPMR save/restore
  
- **ARM Architecture**:
  - `arm64_exception_vector_analysis.md` - Exception vector table analysis
  - `ARM_SMMU_BBML2_Analysis.md` - SMMU BBML2 feature
  - `ARM_SMMU_HTTU_Analysis.md` - SMMU HTTU (Hardware Table Walk Update)
  - `ARM_CPU_BBML2_Analysis.md` - CPU BBML2 feature
  - `ARMv9_RME_GPC_GPT_Notes.md` - ARMv9 RME (Realm Management Extension)
  - `ARM_FastModels_私有外设开发指南.md` - FastModels peripheral development

- **QEMU Development**:
  - `QEMU内存管理基本逻辑.md` - QEMU memory management
  - `如何利用AI做QEMU模型开发.md` - AI-assisted QEMU development
  - `skill_qemu_device_model.md` - QEMU device modeling
  - `kvmtool_ARM地址空间定义.md` - kvmtool address space

- **Virtualization Migration**:
  - `QEMU_Live_Migration_Flow.md` - Live migration flow
  - `v6.3_VM_Migration_LargePage_Split.md` - Large page split during migration
  - `QEMU_NUMA_Migration_Check.md` - NUMA migration checks

- **Other Technical**:
  - `dma-buf-analysis.md` - DMA buffer sharing
  - `virtio-mem-block.md` - Virtio-mem device
  - `VFIO_SMMU_Hugepage_Mapping_and_Split.md` - VFIO hugepage mapping
  - `PTE0_Block_Lock_分析.md` - PTE0 block locking
  - `DMA芯片手册.md` - DMA chip manual

### ARM/ (5 files)
ARM architecture notes:
- `gic/` - GIC (Generic Interrupt Controller)
- `arm64_compile_code/` - ARM64 compilation
- `ARM_ABI/` - ARM ABI specifications

### virt/ (2 files)
Virtualization topics:
- `arm_kvm/` - ARM KVM
- `migration/` - VM migration
- `qemu/` - QEMU specifics
- `vfio/` - VFIO

### riscv/
RISC-V architecture notes

### cpu硬件/
CPU microarchitecture studies

## Search Strategy (Updated)

### Priority Order

When searching, follow this priority order:

1. **AI/index.json keyword lookup** (fastest)
   - Check `AI/index.json` for exact keyword matches
   - Read matched documents directly

2. **AI/ directory search** (organized)
   - Check relevant subdirectory README.md
   - Search within AI/ subdirectories

3. **Legacy directories** (fallback)
   - ai_gen/, virt/, ARM/, linux/, etc.
   - Use grep/find for keyword search

### Search Commands

```bash
# 1. Check index.json for keyword
jq '.search_index.KVM' AI/index.json

# 2. Search in AI/ organized directory
grep -r "keyword" AI/ --include="*.md"

# 3. Fallback to legacy directories
grep -r "keyword" ai_gen/ virt/ ARM/ --include="*.md"
```

### 2. Directory-Based Search
For broad topics, explore specific directories:

| Topic | Directories/Files |
|-------|-------------------|
| KVM/ARM64 Virtualization | `ai_gen/KVM_*.md`, `ai_gen/ARM64_KVM_*.md`, `virt/arm_kvm/` |
| SMMU/IOMMU | `ai_gen/ARM_SMMU_*.md`, `iommu/`, `VFIO_SMMU_*.md` |
| QEMU | `ai_gen/QEMU*.md`, `virt/qemu/`, `skill_qemu_device_model.md` |
| Page Tables | `ai_gen/*PageTable*.md`, `ai_gen/*page*.md`, `dump_kernel_page` |
| Migration | `ai_gen/*Migration*.md`, `virt/migration/` |
| DMA | `dma_buf_note`, `dma_coherent_note`, `ai_gen/dma-buf-analysis.md` |
| Device Drivers | `LDD/`, `uacce_*`, `dma_*`, `pci/` |
| Performance | `perf/`, `ebpf_to_get_duration_dist` |
| ARM Architecture | `ARM/`, `ai_gen/ARM*.md`, `ai_gen/arm64*.md` |
| RISC-V | `riscv/` |
| Memory Management | `linux/mm/`, `memory_note`, `memory_tools`, `pin_page` |

### 3. Cross-Reference
Many topics are interconnected:
- KVM stage-2 page tables relate to SMMU and IOMMU
- Migration involves page tables, memory, and locking
- DMA interacts with IOMMU and VFIO

## Common Query Patterns

### "Tell me about X"
1. Search for files with X in the name
2. Search for X in file contents
3. Read relevant files and summarize

### "How does X work?"
1. Look in `ai_gen/` for detailed analysis documents
2. Check topic-specific directories
3. Search for function/structure names

### "Compare X and Y"
1. Find documents on both X and Y
2. Look for cross-references
3. Synthesize from multiple sources

## Writing Convention

When creating or editing notes, follow the convention in `skill_note_writing.md`:

- Use Chinese headings/explanations where appropriate
- Include version history at the top
- Keep entries concise and focused
- Follow the 80-character line limit
- Use DrawIt-style text diagrams (not Unicode box-drawing)
- Reference code with `file:line` format

## Quick Reference

### Most Active Topics
1. **ARM64 KVM Virtualization**: Stage-2 page tables, MMU locking, memory management
2. **SMMU**: BBML2, HTTU, hugepage mapping
3. **VM Migration**: Live migration, large page handling, NUMA considerations
4. **QEMU Development**: Device modeling, memory management

### Key Files for Quick Start
- `AGENTS.md` - Repository overview
- `skill_note_writing.md` - Note writing conventions
- `ai_gen/KVM_Stage2_PageTable_Walk_Summary.md` - Good example of technical documentation
- `ai_gen/ARM_SMMU_BBML2_Analysis.md` - Detailed hardware feature analysis

## Workflow

When helping users explore the knowledge base:

1. **Identify the topic**: Parse user's question to extract key technical terms
2. **Search relevant files**: Use keyword and directory-based search
3. **Read and analyze**: Read the most relevant files
4. **Synthesize**: Provide a coherent answer referencing the source files
5. **Suggest related topics**: Point to interconnected topics for deeper exploration

## Note Taking

When adding new notes:
1. Place in appropriate directory based on topic
2. Use `ai_gen/` for AI-assisted comprehensive documents
3. Root directory for quick references and misc notes
4. Topic directories (`ARM/`, `virt/`, etc.) for organized collections
5. Follow `skill_note_writing.md` conventions