# Swift ABI Mangling 与兼容性

`docs/ABI/Mangling.rst` 解释了 Swift 在符号生成和类型表示中的规则。Mangling 是 ABI 兼容性中非常关键的一环。

## 1. Mangling 的作用

Mangling 是把符号中的类型、模块、泛型参数等信息编码为唯一名称。它让编译器能够：

- 区分同名函数
- 处理泛型实例化
- 生成稳定且唯一的符号名
- 在跨模块、跨编译单元中保持一致性

## 2. 为什么 ABI 需要 mangling

ABI 兼容性要求：

- 同一语义的函数需要生成稳定符号
- 不同类型的参数不能混淆
- 在库更新时，旧调用点与新实现仍能匹配

如果 mangling 规则变动，符号可能不再匹配，导致 linker 或运行时错误。

## 3. 关键点

### 3.1 C/C++ 类型在 Swift ABI 中的表示

文档说明了 C/C++ 类型在 Swift ABI 中被放到 `__C` 模块中，这是为了保持与 C/C++ ABI 的一致性，而不是将 Clang 模块本身当成 Swift ABI 的一部分。

这体现了一个重要原则：

- Swift ABI 只编码 C/C++ ABI 已经理解的内容
- 这样能尽量保证跨不同版本、跨编译器环境下的兼容性

### 3.2 泛型实例化与 imports

文档还展示了 C++ 模板实例化如何被导入到 Swift 中，并给出它们在 Swift 里被抽象成的结构。

这说明 mangling 对于泛型、模板和跨语言桥接之类的场景至关重要。

## 4. ABI 与 mangling 的关系

- mangling 决定符号名
- 符号名决定二进制层的可识别性
- ABI 兼容性最终依赖这些符号和布局规则是否稳定

因此，mangling 可以视为 ABI 的“名称层面”规则。

## 5. 对应官方文件

- https://github.com/swiftlang/swift/blob/main/docs/ABI/Mangling.rst

## 一句话总结

Mangling 不是“只是给符号起名字”，而是 Swift ABI 对类型、模块和语义进行稳定编码的关键机制，它直接影响符号兼容性和跨库调用。