# 被动式Grpc

被动Grpc是为了弥补Grpc在Kritor作为client时候，Bot端（server）无法主动发起请求的一个实现，其原理是基于grpc的双向流来传输数据，实现比较繁琐，需要用户自行实现接包的等待以及处理。

## 对接流程

1. Bot端的Server启动之后，Kritor端建立连接（调用`reverseStream`方法）。
2. Kritor端会输入一个 `Channel` **(Flow / Stream)**，这个管道流允许你从其中获取`Response`(返回包)，我们称其为收包流。
3. 因为是Bot端启动的server，所以说Bot端需要实现`reverseStream`方法，这个方法需要返回一个`Flow` **(Flow / Stream)**，我们称其为发包流，这个流允许你发送`Request`(请求包)。
4. Bot端通过发包流向Kritor端发送请求包，注意标明正确的`cmd`和`seq`。 
5. Kritor端会从Bot端的发包流中获取到来自Bot端的请求包，然后处理，处理完成之后，会返回一个`Response`给Bot端。
6. Bot端通过发包的`seq`和`cmd`来匹配之前发过的包，从而获取到对应的返回包。

> 这个发包流（Bot端的发包流）在Kritor端，会被用于接收来自Bot端的请求包，所以说也叫做Kritor端的收包流。
> 
> Kritor端的发包流会被用于在Bot端接收来自Kritor端的返回包，也叫做Bot端的收包流。

## 请求包格式

### 请求命令名

```js
let command = "Authentication.Auth"
```

其中`Authentication`是服务名，`Auth`是方法名，这里可以康出[这些东西](/docs/request/authentication.md#%E5%9F%BA%E7%A1%80%E4%BF%A1%E6%81%AF)什么地方来的。

### 自增序列

```js
let seq = 1
```

这个序列是一个自增的序列，用于标记这个请求包的唯一性，Kritor端会通过这个序列来匹配返回包。如果超过了最大值，可以从头开始继续自增。当然如果你觉得一定不会重复，写个随机数也可以捏。

> 来自Kritor端的推送的seq一定是负数！（除Bot端请求包对应的返回包除外）
> 
> Kritor端推送的包是负数，一般接收消息推送/事件推送...
> 
> 从Bot端发出的请求包，必须是正数！
> 
> 从Kritor端返回的包，seq必定和请求包一致，否则无法匹配。 

## 代码示例

这是一个被动Grpc，Bot端作为Server的代码示例，仅限为Kotlin，如有其它语言补充请PR提交。

```kotlin
package passive

import io.grpc.ServerBuilder
import io.kritor.ReverseServiceGrpcKt
import io.kritor.authReq
import io.kritor.reverse.Request
import io.kritor.reverse.Response
import io.kritor.reverse.request
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.delay
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.flow.channelFlow
import kotlinx.coroutines.launch
import java.util.*

class PassiveKritorServer(
    port: Int,
) {
    private val requestList = Collections.synchronizedList(mutableListOf<Request>()) // 用于存储请求包

    private val server = ServerBuilder
        .forPort(port)
        .addService(object: ReverseServiceGrpcKt.ReverseServiceCoroutineImplBase() {
            override fun reverseStream(requests: Flow<Response>): Flow<Request> {
                // requests: Flow<Response> 用于接收来自Kritor端的返回包
                
                // 这里Kritor端建立了一个连接, 并且给予了一个Flow
                // 这个flow用于接收来自Kritor端的返回包
                requestList.add(request {
                    cmd = "Authentication.Auth" // 服务名 + "." + 方法名
                    buf = authReq {
                        account = "admin"
                        ticket = "admin"
                    }.toByteString()
                    seq = 1 // 理论上是一个自增序列，超了可以从头开始继续自增
                })

                // 这里给一个flow给Kritor端，让Kritor端可以获取到客户端发的请求包
                return channelFlow {
                    GlobalScope.launch {
                        requests.collect {
                            println("收到请求返回包：$it")
                        }
                        // 开启一个协程去接收来自kritor端的返回包
                    }

                    while (true) {
                        if (requestList.isNotEmpty()) {
                            send(requestList.removeFirst()) // 发送请求包
                        } // 如果有请求包，就发送
                        delay(10) // 没有就等待
                    }
                }
            }
        })
        .build()!!

    fun start(block: Boolean = false) {
        server.start()
        if (block) {
            server.awaitTermination()
        }
    }
}

fun main() {
    PassiveKritorServer(8080)
        .start(true)
}
```
