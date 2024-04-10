<!-- This Source Code Form is subject to the terms of the Mozilla Public
   - License, v. 2.0. If a copy of the MPL was not distributed with this
   - file, You can obtain one at https://mozilla.org/MPL/2.0/. -->

# Kritor

**本项目目前处于开发阶段, 所定义 api 可能会有较大变动**


*Kritor*是一个聊天机器人应用接口标准.
旨在统一腾讯QQ IM平台上的机器人应用开发接口.
使开发者只需编写一次业务逻辑代码即可应用到多种机器人平台.
在QQNT中, 协议出现大范围变动, OneBot 的原有定义已经不再适用或过于复杂, 因此我们制定了一套新的定义来满足现在的需要.

*Kritor*使用[*grpc*](https://grpc.io/)作为通信协议, 
并提供了一套标准的接口定义, 开发者可以使用任何支持grpc的语言进行开发.

## 特性

- **多语言**：支持多种编程语言, 包括Nodejs、Kotlin、C#等
- **标准化**：提供一套标准的接口定义(提供全套`proto`文件), 使开发者只需编写一次业务逻辑代码即可应用到多种机器人平台
- **稳定性**：使用高质量, 低延迟的通信协议, 保证稳定性和性能

## 项目结构

- [docs](./docs): 文档
- [protos](./protos): 接口定义

### 注解字段解释

```protobuf
extend google.protobuf.MethodOptions {
  bool require = 1; // 需要强制实现的服务
  string desc = 2; // 服务描述
}
```

## 使用说明

### 主动RPC

协议端作为 Server, 插件端作为 Client.
插件端可以通过构造 Stub 对象来直接对协议端进行请求操作.

### 被动RPC

协议端作为 Client, 插件端作为 Server.
因为 Grpc 并没有服务端直接调用客户端的方法, 所以*Kritor*通过双向流来实现插件端对协议端的调用.

### 各语言插件示例

- [Kotlin](https://github.com/KarinJS/kritor-kotlin)
- [TypeScript](https://github.com/KarinJS/kritor-ts)
- [Go](https://github.com/KarinJS/kritor-go)
- [C#](https://github.com/KarinJS/kritor-csharp)

## 接入状态

- [Shamrock](https://github.com/whitechi73/OpenShamrock): **v1.1.0+**
- [Lagrange.Core](https://github.com/LagrangeDev/Lagrange.Core): **0.0.3+**
  - [Lagrange.Kritor](https://github.com/LagrangeDev/Lagrange.Kritor): 一个**Kritor**在 C# 的实现.