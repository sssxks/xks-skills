# Rust 路径规范

本规范定义后续 lint 的判断依据，不要求现在实现工具。当前项目使用的稳定版 Rust 是语言能力基线。

## 规则

1. `use` 声明中的路径允许 `::`。
2. 宏调用、属性宏和 derive 宏的路径允许 `::`。
3. 可以通过 `use` 独立导入的目标项，导入后使用短名。包括模块内函数、类型、模块级常量、enum variant。名称冲突通过 `as` 别名处理。
4. 不能通过 `use` 独立导入的关联项允许 `::`。包括 inherent 和 trait 关联函数、关联常量、关联类型；不区分项目自有项与依赖项。
5. 允许关联项不等于允许其基类型仍带模块前缀。例如导入 `Arc` 后使用 `Arc::clone`，不要写 `std::sync::Arc::clone`。
6. 完全限定语法确有 trait 消歧需要时可以使用；其中可导入的类型和 trait 仍使用短名。
7. 所有 turbofish 禁止，包括 `::<T>`、`::<_>`、`::<T, U>`。通过 `let` 类型标注提供推导上下文。类型位置的 `Table<Row>`、关联类型的 `R::Refs<'a>` 不属于 turbofish。

## 示例

| 表达 | 判定 | 原因或改法 |
| --- | --- | --- |
| `use std::sync::Arc;` | 允许 | 导入声明 |
| `tokio::main` 属性宏 | 允许 | 宏路径 |
| `Arc::clone(&value)` | 允许，保留惯用写法 | trait 关联调用，显式表达共享所有权克隆 |
| `Device::try_from(value)` | 允许 | trait 关联调用 |
| `JsValue::NULL`、`Duration::ZERO` | 允许 | 关联常量 |
| `Self::Err`、`D::Error`、`S::Ok` | 允许 | 关联类型 |
| `R::Refs<'a>` | 允许 | 泛型关联类型 |
| `<T as Trait>::method(value)` | 消歧需要时允许 | 无法独立导入的关联项 |
| `serde_json::from_str(input)` | 修改 | `use serde_json::from_str;` 后调用 `from_str` |
| `std::sync::Arc::clone(&value)` | 修改 | 导入 `Arc`，保留 `Arc::clone` |
| `State::Ready`（enum variant） | 修改 | `use State::Ready;` 后使用 `Ready` |
| `parse::<Device>()` | 修改 | `let device: Device = text.parse()?;` |

## Lint 实现边界

- 通过 AST 和名称解析识别目标项：enum variant 与关联常量不能按大小写区分；同名静态函数可能是模块函数，也可能是 trait 关联函数。
- 区分导入、宏、表达式路径、类型路径和泛型实参语法；不能用搜索 `::` 的正则直接报告违规。
- 对允许的关联路径继续检查其可导入前缀，避免整条路径因末尾是关联项而被放行。
- 检查自有源码、测试和自有宏定义中的代码；不向用户报告第三方宏展开内部的风格问题。宏元变量未展开时不能猜测其解析结果。
- 自动修复必须保持名称绑定和语义；不能因为短名冲突就导入错误的同名项。
- 不为消除诊断增加包装、裸值、泛型参数或存储分配，也不把允许的关联常量改成函数。
