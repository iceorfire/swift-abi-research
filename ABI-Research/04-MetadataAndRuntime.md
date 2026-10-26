# Swift ABI Metadata 与运行时结构

`include/swift/ABI/Metadata.h` 和 `include/swift/ABI/Actor.h` 是研究 Swift 运行时 ABI 的核心代码入口。

## 1. Metadata.h 的作用

该头文件定义了 Swift 运行时中的 metadata 结构，包含：

- 类型元数据
- 类元数据
- 结构体和枚举描述符
- 泛型上下文
- value witness table
- context descriptor

这些结构构成了 Swift 在运行时识别和操作类型的底层基础。

## 2. 关键思想

Swift 的运行时不只是“对象指针”，而是依赖 metadata 来知道：

- 这个值是什么类型
- 需要如何初始化/销毁/复制
- 是否是 heap object
- 是否有特殊 memory layout

Metadata 是 Swift 运行时和 ABI 的核心连接点。

## 3. Actor.h 的作用

`Actor.h` 定义了 actor 的 ABI 结构，例如：

- `DefaultActor`
- `NonDefaultDistributedActor`

它们继承自 `HeapObject`，说明 actor 在堆对象层面仍然遵循 Swift 的对象模型，并在 runtime 中需要专门初始化和销毁规范。

## 4. 为什么这部分很关键

当研究 ABI 时，很多人会关注：

- 参数传递和寄存器约定
- enum layout
- metadata layout

但真正决定 Swift 对“对象/类型/值”如何工作的是这些运行时结构。没有 metadata，Swift 很难在 runtime 中知道对象的真实类型和布局。

## 5. 对应官方文件

- Metadata: https://github.com/swiftlang/swift/blob/main/include/swift/ABI/Metadata.h
- Actor: https://github.com/swiftlang/swift/blob/main/include/swift/ABI/Actor.h

## 一句话总结

Swift 的 ABI 并不是仅停留在函数调用层面；它同时定义了运行时用来描述和管理类型、对象和 actor 的底层结构，这些结构是二进制兼容性的根基。