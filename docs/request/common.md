# 消息接口服务

提供发消息，获取消息相关的服务。

## 基础信息

- **Java包名**: `io.kritor.common`
- **C#命名空间**: `Kritor.Common`
- **[source proto file](/protos/src/main/proto/kritor/message/message.proto)**

## 基础定义

```protobuf
enum Scene {
  GROUP = 0; // 群聊
  FRIEND = 1; // 私聊
  GUILD = 2; // 频道
  STRANGER_FROM_GROUP = 10; // 群临时会话

  // 以下类型为可选实现
  NEARBY = 5; // 附近的人
  STRANGER = 9; // 陌生人
}

message Contact {
  Scene scene = 1;
  string peer = 2; // 群聊则为群号 私聊则为uid 频道消息则为频道号
  optional string sub_peer = 3; // 群临时聊天则为群号 频道消息则为子频道号 其它情况可不提供
}

message Sender {
  string uid = 1;
  optional uint64 uin = 2;
  optional string nick = 3;
}
```

其中的`scene`表示来自何方的类型，而`peer`则为来自何方，他有以下几种情况：

- **GROUP**：`peer`为群号。
- **FRIEND**：`peer`为QQ号或者用户`uid`。
- **GUILD**：`peer`为频道号，`sub_peer`为子频道号。
- **NEARBY**：`peer`为QQ号或者用户`tiny_id`。
- **STRANGER**：`peer`为QQ号或者用户`uid`。
- **STRANGER_FROM_GROUP**：`peer`为QQ，`sub_peer`为群号。
