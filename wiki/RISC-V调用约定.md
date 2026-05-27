---
title: RISC-V调用约定
created: 2026-05-22
tags:
  - RISC-V
  - ABI
  - 函数调用
---

# RISC-V调用约定

## 概述

调用约定(Calling Convention)定义函数调用时的二进制接口，确保不同编译单元间正确交互。

## 寄存器约定

### Caller Saved寄存器

调用者负责保存，被调函数可自由使用：

- **ra(x1)**：返回地址
- **t0-t6**：临时寄存器
- **a0-a7**：参数/返回值寄存器

### Callee Saved寄存器

被调函数负责保存恢复：

- **sp(x2)**：栈指针
- **s0-s11**：保存寄存器

## 参数传递

- **a0-a7**：前8个参数
- **栈**：超过8个参数时，从栈顶依次排列

## 返回值

- **a0-a1**：函数返回值

## 示例分析

### 简单函数

```c
int add(int a, int b) {
    return a + b;
}

int main() {
    int a = 1, b = 2, c = 5;
    c += add(a, b);
    return c;
}
```

反汇编关键部分：

```asm
<main>:
  addi  sp, sp, -32    # 开栈
  sd    ra, 24(sp)      # 保存ra(caller saved)
  sd    s0, 16(sp)      # 保存s0(callee saved)
  ...
  mv    a1, a4          # 准备参数
  mv    a0, a5
  jal   ra, <add>       # 函数调用
  mv    a5, a0          # 取返回值
  ...
  ld    ra, 24(sp)      # 恢复ra
  ld    s0, 16(sp)      # 恢复s0
  addi  sp, sp, 32      # 退栈
  ret                    # 返回
```

### 多参数函数

```asm
# 参数超过8个时
sd    a5, 8(sp)         # 第10个参数入栈
sd    a5, 0(sp)         # 第9个参数入栈
...
li    a7, 8             # a7=第8个参数
li    a6, 7             # a6=第7个参数
...
jal   ra, <func>        # 调用
```

## ABI兼容性

内核模块与内核之间需要ABI兼容，发行版需在版本内保持ABI稳定。

参见[[RISC-V特权级模型]]。

---

**参考来源**：[riscv_calling_convention](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/riscv/riscv_calling_convention)