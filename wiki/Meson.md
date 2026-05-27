---
title: Meson
aliases: [meson, meson构建系统]
tags: [构建系统, meson, 编译, ninja]
source: raw/notes/meson_note
---

# Meson

用 Python 编写的现代构建系统，生成 Ninja 构建文件，速度比 Make 更快。

## 基本逻辑

```
meson.build → meson → build.ninja → ninja → 可执行文件
```

Meson 必须在独立目录构建，支持多种构建配置并存。

## 核心概念

| 概念 | 说明 |
|------|------|
| project() | 定义工程 |
| executable() | 定义可执行目标 |
| library() | 定义库目标 |
| dependency() | 定义外部依赖 |
| declare_dependency() | 定义内部依赖 |
| subdir() | 包含子目录 |

## 基本示例

```python
project('learn_meson', 'c')
executable('hello', 'test.c')
```

## 构建流程

```bash
meson setup builddir     # 配置构建目录
meson compile -C builddir  # 编译
```

## 交叉编译

创建交叉编译配置文件 `rv_cross_file`：

```ini
[host_machine]
system = 'linux'
cpu_family = 'riscv64'
cpu = 'riscv64'
endian = 'little'

[binaries]
c = 'riscv64-linux-gnu-gcc'
cpp = 'riscv64-linux-gnu-g++'
```

构建：

```bash
meson setup --cross-file ./rv_cross_file rv_builddir
meson compile -C rv_builddir
```

## 静态链接

```bash
cd builddir
meson configure -Ddefault_library=static
cd ..
meson compile -C builddir
```

## 库定义示例

```python
libglib = library('glib-2.0',
  sources : [deprecated_sources, glib_sources],
  version : library_version,
  dependencies : [thread_dep, libm, ...],
  c_args : glib_c_args,
)
```

## Compiler Properties

检测系统配置：

```python
cc = meson.get_compiler('c')

uint128_t_src = '''int main() {
static __uint128_t v1 = 100;
}'''
if cc.compiles(uint128_t_src, name : '__uint128_t available')
  glib_conf.set('HAVE_UINT128_T', 1)
endif
```

## Configuration

生成配置文件：

```python
glib_conf = configuration_data()
glib_conf.set('HAVE_UINT128_T', 1)
configure_file(output : 'config.h', configuration : glib_conf)
```

生成 `config.h`：

```c
#define HAVE_UINT128_T 1
```

## Subproject/Wrap

定义外部依赖：

```ini
[wrap-file]
directory = pcre2-10.42
source_url = https://github.com/PhilipHazel/pcre2/releases/download/...
source_hash = 8d36cd8cb6ea2a4c2bb...

[provide]
libpcre2-8 = libpcre2_8
```

## 调试技巧

修改 `build.ninja` 文件，从 target 列表去掉不想构建的 target。

## 相关概念

- [[Makefile]] - 传统构建系统
- [[CMake]] - 跨平台构建系统
- [[Ninja]] - 快速构建工具

---

**参考来源**：[meson_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/meson_note)