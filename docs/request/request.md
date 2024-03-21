# 请求响应相关

## 请求与响应

[主动Grpc](/docs/request/req_active.md): 主动与被动Grpc的行为有所差异，主动Grpc是客户端主动发起请求，服务端被动响应。主动Grpc提供了`挂起/堵塞`和`异步`的两种请求方式。

[被动Grpc](/docs/request/req_passive.md): 被动Grpc则为双向流式实现，客户端与服务端可以同时发送和接收数据。
但是需要客户端对接收的数据进行`command`与`seq`的配对比较，才能拿到对应请求的数据包。

## 请求错误处理（主动Grpc）

当Kritor端无法正常处理某个请求，或者请求失败的时候，将会使用Grpc的错误码来返回错误信息。 其中大量错误信息的描述通常在``status.description``中。

除去Grpc用于保证传输稳定和网络波动的那几个状态码外，Kritor端还会使用以下状态码：

- `OK`: 一切正常。
- `INVALID_ARGUMENT`: 参数错误，例如群禁言没提供群号。
- `UNAUTHENTICATED`: 未认证，通常是鉴权失败或者越级调用。
- `PERMISSION_DENIED`: 权限不足，例如没有权限解除群禁言或者无权使用某个服务。
- `INTERNAL`: Kritor内部出现问题，例如数据库连接失败或者其他异常。

我们通过几个简单的Kotlin代码示例来演示如何处理请求错误。

如果需要查看错误码的详细信息，可以查看[官方文档](https://github.com/grpc/grpc/blob/master/doc/statuscodes.md)。

### Kotlin

```kotlin
suspend fun main() {
    val channel = ManagedChannelBuilder
        .forAddress("localhost", 8080)
        .usePlaintext()
        .enableRetry() // 允许尝试
        .executor(Dispatchers.IO.asExecutor()) // 使用协程的调度器
        .build()
    
    val stub = AuthenticationGrpcKt.AuthenticationCoroutineStub(channel)
    runCatching {
        val rsp = stub.auth(authReq {
            account = "1145141919810"
            ticket = "A123456"
        })
        println(rsp.code)
    }.onFailure {
        // 如果错误，打印错误信息
        val status = Status.fromThrowable(it)
        println(status) // 直接打印code + cause + description
        println(status.code) // 打印错误码
        println(status.description) // 打印错误描述
    }
}
```

更多语言请查看[Grpc官方错误处理示例](https://grpc.io/docs/guides/error/)。

## 请求错误处理（被动Grpc）

当Kritor端无法正常处理某个请求，或者请求失败的时候，将会在返回包携带错误信息。其中大量错误信息的描述通常在`msg`中。

Kritor推送的返回包将会提供以下状态码：

- `SUCCESS`: 一切正常。
- `INVALID_ARGUMENT`: 参数错误，例如群禁言没提供群号。
- `UNAUTHENTICATED`: 未认证，通常是鉴权失败或者越级调用。
- `PERMISSION_DENIED`: 权限不足，例如没有权限解除群禁言或者无权使用某个服务。
- `INTERNAL`: Kritor内部出现问题，例如数据库连接失败或者其他异常。

详细信息可以查看，请求包与返回包的[定义](/protos/src/main/proto/kritor/comm_request.proto)。

## 鉴权操作

如果需要进行鉴权操作的校验，可以查看[鉴权服务](/docs/request/authentication.md)。

如果确保`ticket`可用，请在每次请求的元数据中携带鉴权`ticket`，下面是一个简单的示例：

### 主动RPC

```kotlin
ContactServiceGrpcKt.ContactServiceCoroutineStub(channel).getProfileCard(profileCardRequest {
    uin = 114514 
}, Metadata().also { 
    // 114514就是你的ticket
    it.put(Metadata.Key.of("ticket", Metadata.ASCII_STRING_MARSHALLER), "114514")
})
```

### 被动RPC

Kritor端将携带`ticket`在元数据，发起双向流请求，Bot端作为Server应该去实现一个拦截器或者其它操作已获取鉴权`ticket`达到鉴权目的！

- [Go语言拦截器示例](https://golang2.eddycjy.com/posts/ch3/08-grpc-interceptor/)
- [NodeJs拦截器示例](https://juejin.cn/post/6844904016221044750)

## 实现接口

Kritor提供了多种接口供客户端调用，包括但不限于以下服务：

> - ### [鉴权服务](/docs/request/authentication.md)
>
> - ### [核心服务](/docs/request/core.md)
>
> - ### [群聊服务](/docs/request/group.md)
> 
> - ### [好友服务](/docs/request/friend.md)
> 
> - ### [消息服务](/docs/request/message.md)
> 
> - ### [文件服务](/docs/request/group_file.md)
>
> - ### [Web服务](/docs/request/web.md)
>
> - ### [联系人服务](/docs/request/contact.md)
>
> - ### [频道服务](/docs/request/guild.md)