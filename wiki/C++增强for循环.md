---
title: C++增强for循环
tags: [cpp, for, iterator]
created: 2026-05-22
source: raw/notes/C_C++/C++语法速记
---

# C++增强for循环

C++11引入的增强型for循环[^1]。

## 用法

```cpp
for(auto it : container) {
    // it是容器元素的类型，不是迭代器类型
}
```

语法糖，简化遍历[^1]。

## 相关概念

- [[C++ auto变量]]
- [[C++ STL容器]]

[^1]: raw/notes/C_C++/C++语法速记