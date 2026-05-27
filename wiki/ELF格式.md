---
type: knowledge-node
tags: [core-concept, auto-generated, binary-format, compilation]
last_compiled: 2026-05-22
---

ELF(Executable and Linkable Format)是Linux系统的标准可执行文件格式。

文件结构：
- ELF Header：文件头信息
- Section Headers：段描述信息
- Program Headers：段信息
- 各段内容[^1]

关键工具：readelf、objdump、nm、strings[^1]。

ELF Header包含：
- 文件类型(可执行/目标文件)
- 目标架构
- 入口地址
- 段表位置等信息[^2]

Section与Segment：
- Section是链接时的单位，可自定义
- Segment是加载时的单位，相同属性的Section可合并[^1]
- Program Headers描述Segment信息

符号表(Symbol Table)：
- 存储程序符号信息
- readelf -s或nm可查看
- 符号地址对于用户态是虚拟地址[^1]

内核链接场景中，符号表地址可能与实际运行地址不同(MMU开启前)[^3]。

[[链接]]过程生成ELF文件，[[加载]]过程解析ELF并执行。

相关概念包括[[链接]]、[[加载]]、[[编译流程]]。

[^1]: 来源于原始素材 raw/notes/重读《程序员的自我修养》 第190-231行。
[^2]: 来源于原始素材 raw/notes/重读《程序员的自我修养》 第200-222行。
[^3]: 来源于原始素材 raw/notes/重读《程序员的自我修养》 第225-230行。