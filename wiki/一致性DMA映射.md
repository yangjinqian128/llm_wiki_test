---
type: knowledge-node
tags: [core-concept, auto-generated, dma, cache-coherence]
last_compiled: 2026-05-22
---

一致性DMA映射(Coherent DMA Mapping)保证CPU和设备对DMA缓冲区的访问一致性。

配置方式[^1]：
- Device Tree：dma-coherent属性
- ACPI：_CCA方法

一致性配置的影响[^1]：
- **配置时**：CPU访问为cacheable，设备通过SMMU访问为cacheable
- **未配置时**：CPU访问为non-cacheable，设备访问为cacheable

实现细节[^2]：

配置一致性时：
- `alloc_pages()`分配内存
- 内存属性已配置为cacheable[^2]

未配置一致性时：
- `__alloc_from_pool()`从atomic_pool分配
- atomic_pool属性为non-cacheable[^2]

SMMU页表配置[^3]：
- `iommu_dma_map_page()`配置SMMU页表
- 配置_CCA或dma-coherent时，pte包含IOMMU_CACHE
- 设备访问DMA区域为cacheable

[[DMA]]映射的一致性由硬件和软件配置共同保证。

相关概念包括[[DMA]]、[[SMMU]]、[[cache一致性]]。

[^1]: 来源于原始素材 raw/notes/dma_coherent_note 第7-14行。
[^2]: 来源于原始素材 raw/notes/dma_coherent_note 第26-40行。
[^3]: 来源于原始素材 raw/notes/dma_coherent_note 第42-62行。