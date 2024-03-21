# 合并转发消息服务

本服务用于合并转发消息，将多个消息合并为一个消息并转发给指定的用户。

## 基础信息

- **服务名**: `ForwardMessageService`
- **Java包名**: `io.kritor.message`
- **C#命名空间**: `Kritor.Message`
- **[source proto file](/protos/src/main/proto/kritor/message/forward_message.proto)**

## 基础定义

### 联系人

```protobuf
package kritor.message; // 记住是这个路径的！！！！！

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
  string peer = 2; // 群聊则为群号 私聊则为QQ号
  optional string sub_peer = 3; // 群临时聊天则为群号 频道消息则为子频道号 其它情况可不提供
}
```

### 消息元素

```protobuf
enum ElementType {
  TEXT = 0;
  AT = 1;
  FACE = 2;
  BUBBLE_FACE = 3;
  REPLY = 4;
  IMAGE = 5;
  VOICE = 6;
  VIDEO = 7;
  BASKETBALL = 8;
  DICE = 9;
  RPS = 10;
  POKE = 11;
  MUSIC = 12;
  WEATHER = 13;
  LOCATION = 14;
  SHARE = 15;
  GIFT = 16;
  MARKET_FACE = 17;
  FORWARD = 18;
  CONTACT = 19;
  JSON = 20;
  XML = 21;
  FILE = 22;
  MARKDOWN = 23;
  BUTTON = 24;
  NODE = 99;
}

message Element {
  ElementType type = 1;
  oneof data {
    // ... 省略 ...
  }
}
```

如果需要详细的消息元素内容，请参考[消息元素](/protos/src/main/proto/kritor/message/comm_message.proto)。

## 合并转发消息

合并转发消息到指定的聊天窗口。

### 参数

- **方法名**: `ForwardMessage`
- **请求类型**: `ForwardMessageRequest`
- **响应类型**: `ForwardMessageResponse`

### 请求与响应

```protobuf
message ForwardMessageRequest {
  Contact contact = 1;
  optional uint32 retry_count = 2;
  repeated ForwardMessageBody messages = 3;
  optional bool only_upload = 4; // 只上传不发送
}

message ForwardMessageResponse {
  optional uint64 message_id = 1; // 当只上传时，这里啥也没有
  string res_id = 2;
}
```