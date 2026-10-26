# Swift ABI 调用约定（Calling Convention）

`docs/ABI/CallingConventionSummary.rst` 汇总了 Swift 在不同平台上的调用约定，以及与 C/C++/ObjC 的差异。

## 1. 什么是调用约定

调用约定定义：

- 参数如何传递给函数
- 返回值如何返回
- 哪些寄存器由调用方/被调用方保留
- `self`、`error`、`async context` 等特殊参数如何使用

## 2. x86-64 的示例

文档中给出了一个汇总表，说明在 macOS/iOS 上寄存器的用途，例如：

- `rax`：返回值
- `rdi` / `rsi` / `rdx` / `rcx`：参数/寄存器传递
- `r13`：Swift `self`
- `r14`：async context
- `r12`：用于错误返回

这说明 Swift 的调用约定并不等同于通用 C/C++ 调用约定，而是额外设计了语言特定的寄存器语义。

## 3. 关键概念

### 3.1 Swift `self` register

Swift 的 `self` 在 ABI 中有特定寄存器语义。它不是普通的 C++ `this`，而是 Swift 的上下文寄存器。它是 ABI 层设计的重要一部分。

### 3.2 Error return

Swift 的错误返回也有专门约定，文档中提到 `ABI stability manifesto`：

- error return
- `self` register
- async context

这些设计都说明 Swift 需要在二进制层做更高层语言级的支持。

## 4. 调用约定和 ABI 兼容性的关系

如果调用约定改变：

- 参数传递规则变化
- 返回值布局变化
- error 语义变化
- async context 改变

那么旧的编译产物可能不再与新库兼容。

## 5. 实践意义

调用约定影响：

- 函数签名在 machine code 中如何表示
- ABI compatibility checks 在编译器中的判定
- Swift 函数与 C/C++/Objective-C 的桥接

## 6. 对应官方文件

- https://github.com/swiftlang/swift/blob/main/docs/ABI/CallingConventionSummary.rst

## 一句话总结

Swift 的调用约定不仅描述了寄存器和栈使用，还为语言级语义（如 `self`、错误返回、async context）预留了稳定的二进制接口。