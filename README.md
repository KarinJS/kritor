# Kritor

**Kritor** （OneBotX）是一个聊天机器人应用接口标准，
旨在统一腾讯QQ IM平台上的机器人应用开发接口 ，
使开发者只需编写一次业务逻辑代码即可应用到多种机器人平台。

**Kritor** 使用[**grpc**](https://grpc.io/)作为通信协议，
并提供了一套标准的接口定义，开发者可以使用任何支持grpc的语言进行开发。

# 特性

- **多语言**：支持多种编程语言，包括Nodejs、Kotlin、C#等
- **标准化**：提供一套标准的接口定义（提供全套`proto`文件），使开发者只需编写一次业务逻辑代码即可应用到多种机器人平台
- **稳定性**：使用高质量，低延迟的通信协议，保证稳定性和性能

# 项目结构

- **[docs](./docs)**: 文档
- **[protos](./protos)**: 接口定义

# 使用注意

本项目的目的不是为了取代`OneBot`。 

在新一代客户端协议中出现大范围变动，
其原有定义已经不再适用，因此我们提供了新的定义。

### 注解字段解释

```protobuf
extend google.protobuf.MethodOptions {
  bool require = 1; // 需要强制实现的服务
  string desc = 2; // 服务描述
}
```

## 主动RPC

什么是主动RPC？
即**Kritor**作为Server， Client作为GRPC CLIENT，客户端可以通过构造Stub对象来对Kritor端进行请求操作。

### 代码示例

以下是主动RPC的代码示例，不是被动RPC的哦！

- [Kotlin](https://github.com/KarinJS/kritor-kotlin)
- [Java]()
- [Python]()
- [Go]()
- [C#](https://github.com/KarinJS/kritor-csharp)
- [C++]()
- [Rust]()
- [JavaScript]()

## 被动RPC

因为Grpc并没有明显的服务端直接rpc调用客户端的方法。
这里**Kritor**采用双向流来实现这一操作。

## 接入状态

- [Shamrock](https://github.com/whitechi73/OpenShamrock): **v1.1.0+**
- [Lagrange.Core](https://github.com/LagrangeDev/Lagrange.Core): **0.0.3+**
  - **[Lagrange.Kritor](https://github.com/LagrangeDev/Lagrange.Kritor)**: 一个**Kritor**在C#的实现。