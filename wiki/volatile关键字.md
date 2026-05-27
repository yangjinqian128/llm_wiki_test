---
type: knowledge-node
tags: [core-concept, auto-generated, c-language, compiler]
last_compiled: 2026-05-22
---

volatile关键字告诉编译器不做特定优化，确保每次读写都直接访问内存。

需要volatile的场景[^1]：
1. MMIO(内存映射IO寄存器)
2. 多执行流共享变量
3. 硽件自动改变的状态

编译器优化行为[^2]：
- 多次写：只保留最后一次
- 多次读：缓存到寄存器，只读一次

volatile强制编译器：
- 每次写操作都生效
- 每次读操作都从内存读取[^2]

示例对比(O3优化)[^2]：
```
// 无volatile：三次写只保留最后一次
test = 111; test = 222; test = 333; 
// 编译为：str 333, [test]

// 有volatile：三次写全部保留
volatile test_v = 111; test_v = 222; test_v = 333;
// 编译为：str 111; str 222; str 333
```

重要限制：volatile不保证写生效顺序，需要[[内存屏障]]指令[^3]。

[[内存可见性]]问题需要volatile配合其他同步机制解决。

相关概念包括[[内存可见性]]、[[编译器优化]]、[[内存屏障]]。

[^1]: 来源于原始素材 raw/notes/volatile_note 第20-33行。
[^2]: 来源于原始素材 raw/notes/volatile_note 第236-245行。
[^3]: 来源于原始素材 raw/notes/volatile_note 第247-251行。