# Swift ABI 总览

ABI（Application Binary Interface，应用二进制接口）是描述二进制层面兼容性的规范。它规定了：

- 函数参数/返回值如何在寄存器和栈上传递
- 对象布局如何存储在内存中
- 运行时 metadata 的结构和约定
- 类型名 mangling 与符号兼容性
- 期望的二进制兼容性边界

在 Swift 中，ABI 不只是“函数调用约定”，还包括：

- type layout
- vtable / witness table / metadata layout
- memory model 对低层结构的假设
- C/C++/ObjC 互操作方式
- 语言特性（enum、class、actor 等）的二进制稳定性

## 关键理解

1. ABI 与 API 不同
   - API 关注源代码层面的调用方式
   - ABI 关注编译后的二进制可兼容性

2. Swift 的 ABI 是分层的
   - 高层语义：`enum`、`struct`、`class`、`protocol` 等
   - 中层表示：metadata、value witness table
   - 低层实现：寄存器、栈、指针、布局

3. ABI 设计的核心目标
   - 兼容旧库和新库
   - 允许语言演进而不破坏已编译代码
   - 保证与 C/C++/ObjC 的互操作性

## 与官方文件的对应关系

- `docs/ABI/TypeLayout.rst`：类型布局
- `docs/ABI/CallingConventionSummary.rst`：调用约定汇总
- `docs/ABI/Mangling.rst`：mangling 与符号
- `include/swift/ABI/Metadata.h`：元数据定义
- `include/swift/ABI/Actor.h`：actor 的 ABI 结构

## 实际学习顺序建议

1. 先阅读 TypeLayout：理解值和对象在二进制层面的表示
2. 再看 CallingConvention：理解参数/返回值的传递规则
3. 接着看 Metadata：理解 Swift 运行时如何描述类型
4. 最后看 Mangling：理解符号与 ABI 兼容性决策

## 一句话总结

Swift 的 ABI 不是单一文档，而是一套跨语言、跨运行时、跨平台的一致性规范；它把“语义层”的 Swift 类型与“机器层”的内存与寄存器布局连接起来。