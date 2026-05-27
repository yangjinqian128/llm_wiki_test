---
type: knowledge-node
tags: [core-concept, auto-generated, process, linux-kernel]
last_compiled: 2026-05-22
---

fork是Linux进程创建的系统调用，通过复制当前进程创建子进程。

调用链[^1]：
```
fork → kernel_clone → copy_process
```

核心复制操作[^1]：

文件描述符：
- CLONE_FILES：共享files_struct，增加引用计数
- 无CLONE_FILES：复制files_struct

内存管理：
- CLONE_VM：共享mm_struct
- 无CLONE_VM：调用dup_mm()复制[^1]

地址空间复制[^1]：
- `dup_mmap()`复制vma
- `vm_area_dup()`复制vma结构
- `copy_page_range()`复制页表

[[copy-on-write]]机制[^1]：
- 不配置VM_WIPEONFORK时
- 页表配置为只读
- 写操作触发COW复制

fork创建的子进程继承父进程的资源，通过COW优化性能。

相关概念包括[[copy-on-write]]、[[进程创建]]、[[进程管理]]。

[^1]: 来源于原始素材 raw/notes/fork_note 第4-30行。