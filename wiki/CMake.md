---
title: CMake
aliases: [cmake, cmake构建系统]
tags: [构建系统, cmake, 编译]
source: raw/notes/cmake_note
---

# CMake

跨平台构建系统，生成 Makefile 或其他构建文件。

## 基本逻辑

CMake 根据配置文件（CMakeLists.txt）生成 Makefile，然后用户使用 make 进行构建。

## 核心概念

| 概念 | 说明 |
|------|------|
| project() | 定义工程名称 |
| add_executable() | 定义可执行文件目标 |
| add_library() | 定义库目标 |
| add_subdirectory() | 包含子目录 |
| target_link_libraries() | 链接库 |

## 基本示例

```cmake
project(hello_demo)

add_executable(hello hello.c hello.h)

add_subdirectory(lib)

target_link_libraries(hello add)
```

子目录 `lib/CMakeLists.txt`：

```cmake
add_library(add SHARED add.c)  # 动态库
```

## 指定编译器

```cmake
set(CMAKE_C_COMPILER "/opt/gcc/bin/gcc")
```

## 编译参数

```cmake
set(CMAKE_C_FLAGS "-O2 -g")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fprofile-arcs -ftest-coverage")
```

## 链接参数

```cmake
target_link_libraries(hello add numa)  # 链接库
set(CMAKE_EXE_LINKER_FLAGS "-L/opt -I/opt/include/")
```

## 库类型

| 参数 | 说明 |
|------|------|
| SHARED | 动态库 |
| STATIC | 静态库 |

## 构建流程

```bash
cmake .           # 生成 Makefile
make              # 构建
```

或使用独立构建目录：

```bash
mkdir build && cd build
cmake ..
make
```

## 需要描述的场景

1. 指定编译目标和源文件
2. 指定编译/链接参数
3. 支持多目录层次构建
4. 支持伪目标（clean/install）
5. 支持系统环境检测
6. 支持条件构建

## 相关概念

- [[Makefile]] - 传统构建系统
- [[Meson]] - 新一代构建系统
- [[ELF格式]] - 编译输出格式

---

**参考来源**：[cmake_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/cmake_note)