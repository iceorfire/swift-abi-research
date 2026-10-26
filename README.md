# Swift ABI 研究资料包

本仓库整理了 Swift ABI 相关的关键文档、核心概念和代码入口，便于快速学习和引用。

## 目录

- `ABI-Research/00-Overview.md` — ABI 总览
- `ABI-Research/01-TypeLayout.md` — 类型布局与枚举布局
- `ABI-Research/02-CallingConvention.md` — 调用约定
- `ABI-Research/03-Mangling.md` — Mangling 与 ABI 兼容性
- `ABI-Research/04-MetadataAndRuntime.md` — Metadata 与运行时结构
- `ABI-Research/05-KeyLinks.md` — 关键链接索引

## 说明

这份资料按 Swift 官方仓库中的真实文件整理，重点覆盖：

- Swift ABI 文档：`docs/ABI/`
- Swift 运行时结构：`include/swift/ABI/`
- 类型表述与语法层面：`include/swift/AST/`

## 读取方式

在 GitHub 页面中，使用“Download ZIP”即可下载整个资料包。

如果你希望进一步深入，我也可以继续把这份内容扩展为：

- 更完整的中文注释版
- 代码级注释版
- 更偏学习路线的版本
- 按平台（x86_64 / arm64 / Linux / Apple）拆分的版本

## 参考仓库

- Swift 官方仓库：https://github.com/swiftlang/swift

## 关键文件（官方）

- TypeLayout: https://github.com/swiftlang/swift/blob/main/docs/ABI/TypeLayout.rst
- CallingConventionSummary: https://github.com/swiftlang/swift/blob/main/docs/ABI/CallingConventionSummary.rst
- Mangling: https://github.com/swiftlang/swift/blob/main/docs/ABI/Mangling.rst
- Metadata: https://github.com/swiftlang/swift/blob/main/include/swift/ABI/Metadata.h
- Actor: https://github.com/swiftlang/swift/blob/main/include/swift/ABI/Actor.h
