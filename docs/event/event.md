# 事件推送

Kritor的事件推送由于基于grpc，所以说主动RPC和被动RPC，有不同的消息监听写法。

## 事件监听示例

### 主动RPC

接下来将举例一段代码，来说明如何使用主动RPC去实现事件监听。

```kotlin
val channel = ManagedChannelBuilder
    .forAddress("localhost", 8080)
    .usePlaintext()
    .enableRetry() // 允许尝试
    .executor(Dispatchers.IO.asExecutor()) // 使用协程的调度器
    .build()

EventServiceGrpcKt.EventServiceCoroutineStub(channel).registerActiveListener(requestPushEvent { 
    type = EventType.EVENT_TYPE_MESSAGE // 声明需要监听的是消息事件
}).collect { 
// 这里处理消息事件
}
```

需要注意的是，这里的`EventServiceGrpcKt.EventServiceCoroutineStub`是一个自动生成的类，它是由`proto`文件生成的。

**如果您开启了鉴权，请在监听的时候提供正确的鉴权`ticket`**！

### 被动RPC

被动Grpc比较复杂，其实更需要一个框架去封装它，[点我查看示例](/src/test/kotlin/passive/Server.kt)。

## 事件大全

> - ### [核心事件](/docs/event/core.md)
>
> - ### [消息事件](/docs/event/msg.md)
>
> - ### [通知事件](/docs/event/notice.md)
>
> - ### [请求事件](/docs/event/request.md)