# AGENTS.md - Notes Repository

This is a personal knowledge base containing technical notes about systems programming.

## Knowledge Base Skill

A navigation skill is available at `.opencode/skills/knowledge-base/SKILL.md` to help explore and retrieve information from this knowledge base. It provides:

- Topic-based directory mapping
- Search strategies for common queries
- Cross-reference patterns between related topics
- Quick reference to most active topics

## Structure

### Root Directory (~150 files)
Miscellaneous technical notes including:
- **Linux Kernel**: Memory management, kthreads, workqueue, locking, CPU hotplug, tracing
- **Device Drivers**: DMA buffers, coherent DMA, GPIO, IRQ, UACCE accelerator framework
- **System Tools**: GDB, strace, perf, vim, tmux, docker, git
- **Build Systems**: Make, CMake, Meson, autotools
- **Programming**: C/C++, Python, algorithms, bash scripting
- **Hardware**: PCI/PCIe, IOMMU, ACPI, firmware
- **Performance**: perf, eBPF, memory tools
- **Debug**: Debug techniques, RAS (Reliability, Availability, Serviceability)

### ai_gen/ (26 files)
AI-assisted comprehensive technical documentation:
- **KVM/ARM64 Virtualization**: Stage-2 page tables, MMU locking, memory management
- **ARM Architecture**: Exception vectors, SMMU (BBML2, HTTU), CPU features, RME
- **QEMU Development**: Memory management, device modeling
- **VM Migration**: Live migration flow, large page handling, NUMA migration
- **System Topics**: DMA-buf, VFIO, hugepage mapping

### Topic Directories
- `ARM/`: ARM architecture (GIC, ABI, compilation)
- `riscv/`: RISC-V architecture notes
- `cpu硬件/`: CPU microarchitecture studies
- `virt/`: Virtualization (ARM KVM, migration, QEMU, VFIO)
- `linux/mm/`: Linux memory management
- `linux/sched/`: Linux scheduler
- `pci/`: PCI/PCIe subsystem
- `iommu/`: IOMMU subsystem
- `perf/`: Performance monitoring
- `LDD/`: Linux device drivers
- `python/`: Python notes

## Usage

This is a reference/memo repository, not a software project. No build, test, or CI workflows exist.

### Searching the Knowledge Base

1. **By keyword**: Use `grep -r "keyword" --include="*.md" --include="*.txt"`
2. **By topic**: Check relevant directories (see knowledge base skill)
3. **By filename**: Use `find . -name "*keyword*"`

### Writing Notes

Follow conventions in `skill_note_writing.md`:
- Preserve technical accuracy
- Keep entries concise and focused
- Use Chinese headings/explanations as appropriate
- Include version history
- Follow 80-character line limit
- Reference code with `file:line` format