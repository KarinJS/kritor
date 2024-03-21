# 主动Grpc

主动Grpc由Grpc框架提供与实现请求的接收等待以及重试，是Kritor最为推荐的请求方式。

## 请求与响应

Grpc提供了`挂起/堵塞`和`异步`的两种请求方式。

### 异步 (流式传输)

```kotlin
suspend fun main() {
    val channel = ManagedChannelBuilder
        .forAddress("localhost", 8080)
        .usePlaintext()
        .enableRetry() // 允许尝试
        .executor(Dispatchers.IO.asExecutor()) // 使用协程的调度器
        .build()
    
    val observer: StreamObserver<AuthRsp> = object: StreamObserver<AuthRsp> {
        override fun onCompleted() {

        }

        override fun onNext(rsp: AuthRsp?) {
            // doSomething
        }

        override fun onError(e: Throwable?) {

        }
    }

    AuthenticationGrpc.newStub(channel).auth(authReq {
        account = "1145141919810"
        ticket = "A123456"
    }, observer)
}
```

### 挂起

```kotlin
suspend fun main() {
    val channel = ManagedChannelBuilder
        .forAddress("localhost", 8080)
        .usePlaintext()
        .enableRetry() // 允许尝试
        .executor(Dispatchers.IO.asExecutor()) // 使用协程的调度器
        .build()

    val stub = AuthenticationGrpcKt.AuthenticationCoroutineStub(channel)
    val rsp = stub.auth(authReq {
        account = "1145141919810"
        ticket = "A123456"
    })
}
```

### 其它语言的请求示例

- [Java](https://grpc.io/docs/languages/java/basics/#calling-service-methods)（同步请求）
- [Java](https://grpc.io/docs/languages/java/basics/#client-side-streaming-rpc)（流式异步）
- [C#](https://learn.microsoft.com/zh-cn/aspnet/core/tutorials/grpc/grpc-start?view=aspnetcore-8.0&tabs=visual-studio#create-the-grpc-client-in-a-net-console-app) （微软文档）
- [C++](https://grpc.io/docs/languages/cpp/basics/#calling-service-methods)
- [Golang](https://grpc.io/docs/languages/go/basics/#calling-service-methods)
- [Node](https://grpc.io/docs/languages/node/basics/#calling-service-methods)
- [Object-C](https://grpc.io/docs/languages/objective-c/basics/#calling-service-methods)
- [PHP](https://grpc.io/docs/languages/php/basics/#calling-service-methods)
- [Python](https://grpc.io/docs/languages/python/basics/#calling-service-methods)
