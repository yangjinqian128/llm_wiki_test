---
type: knowledge-node
tags: [core-concept, auto-generated, verification-tool, memory-model]
last_compiled: 2026-05-22
---

herd7是用于内存模型验证的工具，与cat文件和Litmus测试用例配合使用，验证处理器内存模型的正确性。

基本工作流程：
1. 使用cat文件描述内存模型定义
2. 使用Litmus定义测试指令序列和断言
3. herd7以两者为参数运行模型，输出所有可能结果[^1]

herd7能够遍历Litmus测试用例的所有可能结果，检测断言正确性，并生成结果对应的图形[^1]。输出结果用ca(coherence after)和rf(read from)标记指令实际执行序列。

ARM官方提供了在线工具，可以可视化展示整个验证流程[^2]。

典型输出示例：对于MP(Message Passing)测试用例，herd7可以显示P1上寄存器的各种可能值(00/01/10/11)，以及产生这些值的指令执行顺序[^2]。

[[内存模型验证]]依赖herd7完成自动化验证。相关概念包括[[ARM内存模型]]、[[Litmus测试]]。

[^1]: 来源于原始素材 raw/notes/使用herd7做ARM内存模型验证 第110-124行。
[^2]: 来源于原始素材 raw/notes/使用herd7做ARM内存模型验证 第125-146行。