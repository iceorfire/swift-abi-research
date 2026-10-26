# Swift ABI 类型布局（Type Layout）

`docs/ABI/TypeLayout.rst` 是理解 Swift ABI 最重要的文件之一。它讨论了 Swift 类型在二进制层如何表示，以及不同类型的存储与布局规则。

## 1. 本质

Type layout 关注：

- 一个值在内存中的字节序列是什么
- 间接引用、引用计数、tag bits 是如何处理的
- `enum`、`struct`、`class` 在二进制层的表示规则

## 2. 关键概念

### 2.1 Class pointer assumptions

文档中明确提到：

> Swift class pointers are never tagged pointers.

这说明 Swift 的类对象指针在 ABI 层面不使用 tagged pointer 这类优化。该约束有助于保证类对象的稳定性和可分析性。

### 2.2 Fragile Enum Layout

TypeLayout 中对 `enum` 的布局进行了一系列说明，主要是：

- Empty enums
- Single-case enums
- C-like enums
- Single-payload enums
- Multi-payload enums

其中，最关键的是：

- C-like enum 通常按最小整数类型存储
- Single-payload enum 通过 data type 的表示加上 tag bits 或 spare bits 来编码
- 额外 inhabitant（额外值）可能被利用作无数据 case 的表示

这意味着 Swift 的 enum 在 ABI 层并不是“总是一个额外的 tag field”，而是会根据具体布局选择最省空间的方案。

## 3. 为什么重要

Enum 的布局直接影响：

- 内存大小
- ABI 稳定性
- 二进制兼容性
- Swift 与 C/C++ 互操作的边界

如果 enum 的布局改变了，就可能导致已编译代码在二进制层出现不兼容。

## 4. 典型示例

```swift
enum Empty {}

enum EmptyCase { case X }

enum EnumLike2 {
  case A
  case B
}

enum CharOrSectionMarker {
  case Paragraph
  case Char(UnicodeScalar)
}
```

在这类场景里，ABI 会尽量压缩存储，并尽可能不用额外的 tag 字段。

## 5. 学习建议

- 先看文档中的“Fragile Enum Layout”部分
- 再结合 runtime metadata 研究 enum 的描述符和 witness table
- 由此理解为什么某些 enum 修改会被视为 ABI breaking

## 6. 对应官方文件

- https://github.com/swiftlang/swift/blob/main/docs/ABI/TypeLayout.rst

## 一句话总结

Swift 的类型布局并不是“简单按对象结构堆叠”，而是根据数据语义在 ABI 层做了大量压缩和优化，以尽量降低空间开销并维持兼容性。