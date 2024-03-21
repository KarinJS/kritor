# 消息接口服务

提供发消息，获取消息相关的服务。

## 基础信息

- **服务名**: `MessageService`
- **Java包名**: `io.kritor.message`
- **C#命名空间**: `Kritor.Message`
- **[source proto file](/protos/src/main/proto/kritor/message/message.proto)**

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

其中的`scene`表示来自何方的类型，而`peer`则为来自何方，他有以下几种情况：

- **GROUP**：`peer`为群号。
- **FRIEND**：`peer`为QQ号或者用户`uid`。
- **GUILD**：`peer`为频道号，`sub_peer`为子频道号。
- **NEARBY**：`peer`为QQ号或者用户`tiny_id`。
- **STRANGER**：`peer`为QQ号或者用户`uid`。
- **STRANGER_FROM_GROUP**：`peer`为QQ，`sub_peer`为群号。

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

## 发送消息

发送消息到指定的聊天窗口。

### 参数

- **方法名**: `SendMessage`
- **请求类型**: `SendMessageRequest`
- **响应类型**: `SendMessageResponse`

### 请求与响应

```protobuf
message SendMessageRequest {
  Contact contact = 1; // 发送目标
  repeated Element elements = 2; // 发的什么东西
  optional uint32 retry_count = 3; // 重试次数
}

message SendMessageResponse {
  uint64 message_id = 1; // 发送成功后的消息ID
  uint32 message_time = 2; // 发送时间
}
```

## 通过ResID发送消息

通过ResID发送消息到指定的聊天窗口。

### 参数

- **方法名**: `SendMessageByResId`
- **请求类型**: `SendMessageByResIdRequest`
- **响应类型**: `SendMessageByResIdResponse`

### 请求与响应

```protobuf
message SendMessageByResIdRequest {
  string res_id = 1; // 资源ID
  Contact contact = 2; // 发送目标
  optional uint32 retry_count = 3; // 重试次数
}

message SendMessageByResIdResponse {
  uint64 message_id = 1; // 发送成功后的消息ID
  uint32 message_time = 2; // 发送时间
}
```

## 清空会话消息

清空指定会话的消息。

### 参数

- **方法名**: `ClearMessages`
- **请求类型**: `ClearMessagesRequest`
- **响应类型**: `ClearMessagesResponse`

### 请求与响应

```protobuf
message ClearMessagesRequest {
  Contact contact = 1; // 要清空的目标
}

message ClearMessagesResponse {}
```

## 撤回消息

撤回指定的消息。

### 参数

- **方法名**: `RecallMessage`
- **请求类型**: `RecallMessageRequest`
- **响应类型**: `RecallMessageResponse`

### 请求与响应

```protobuf
message RecallMessageRequest {
  Contact contact = 1; // 要撤回的目标，私聊则为对方，而不是写自己
  uint64 message_id = 2; // 要撤回的消息ID，消息id固有格式：前32bit为时间戳，后32bit为扩展字段，可携带聊天类型等各种信息
}

message RecallMessageResponse {
}
```

## 像telegram一样消息下评论表情（灰度）

在每条消息的底下评论一个表情。

### 参数

- **方法名**: `SetMessageCommentEmoji`
- **请求类型**: `SetMessageCommentEmojiRequest`
- **响应类型**: `SetMessageCommentEmojiResponse`

### 请求与响应

```protobuf
message SetMessageCommentEmojiRequest {
  Contact contact = 1;
  uint64 message_id = 2; // 要设置的消息ID
  uint32 face_id = 3; // 表情ID
  optional bool is_comment = 4; // 是否是评论,如果是false，则为撤销
}

message SetMessageCommentEmojiResponse {
}
```

## 获取合并转发消息

获取合并转发消息。

### 参数

- **方法名**: `GetForwardMessages`
- **请求类型**: `GetForwardMessagesRequest`
- **响应类型**: `GetForwardMessagesResponse`

### 请求与响应

```protobuf
message GetForwardMessagesRequest {
  string res_id = 1; // 资源ID
}

message GetForwardMessagesResponse {
  repeated MessageBody messages = 1; // 获取到的消息
}
```

## 获取消息

获取指定消息。

### 参数

- **方法名**: `GetMessage`
- **请求类型**: `GetMessageRequest`
- **响应类型**: `GetMessageResponse`

### 请求与响应

```protobuf
message GetMessageRequest {
  Contact contact = 1; // 要获取的目标
  uint64 message_id = 2; // 要获取的消息ID
}

message GetMessageResponse {
  MessageBody message = 1; // 获取到的消息
}
```

## 通过`seq`获取消息

通过`seq`获取指定消息。

### 参数

- **方法名**: `GetMessageBySeq`
- **请求类型**: `GetMessageBySeqRequest`
- **响应类型**: `GetMessageBySeqResponse`

### 请求与响应

```protobuf
message GetMessageBySeqRequest {
  Contact contact = 1; // 要获取的目标
  uint64 message_seq = 2; // 要获取的消息序号
}

message GetMessageBySeqResponse {
  MessageBody message = 1; // 获取到的消息
}
```

## 获取历史消息

获取指定目标的历史消息。

### 参数

- **方法名**: `GetHistoryMessage`
- **请求类型**: `GetHistoryMessageRequest`
- **响应类型**: `GetHistoryMessageResponse`

### 请求与响应

```protobuf
message GetHistoryMessageRequest {
  Contact contact = 1; // 要获取的目标
  optional uint64 start_message_id = 2; // 起始消息ID 默认最新一条开始
  optional uint32 count = 3; // 获取数量 默认10
}

message GetHistoryMessageResponse {
  repeated MessageBody messages = 1; // 获取到的消息
}
```

## 删除群精华消息

删除群精华消息。

### 参数

- **方法名**: `DeleteEssenceMsg`
- **请求类型**: `DeleteEssenceMsgRequest`
- **响应类型**: `DeleteEssenceMsgResponse`

### 请求与响应

```protobuf
message DeleteEssenceMsgRequest {
  uint64 group_id = 1;
  uint64 message_id = 2; // 要删除的消息ID
}

message DeleteEssenceMsgResponse {
}
```

## 获取群精华消息列表

获取群精华消息列表。

### 参数

- **方法名**: `GetEssenceMessages`
- **请求类型**: `GetEssenceMessagesRequest`
- **响应类型**: `GetEssenceMessagesResponse`

### 请求与响应

```protobuf
message EssenceMessage {
  uint64 sender_uin = 1;
  string sender_nick = 2;
  uint32 msg_time = 3;
  uint64 operator_uin = 4;
  string operator_nick = 5;
  uint32 operation_time = 6;
  uint64 message_id = 7;
  uint64 message_seq = 8;
  repeated Element elements = 9;
}

message GetEssenceMessagesRequest {
  uint64 group_id = 1;
}

message GetEssenceMessagesResponse {
  repeated EssenceMessage essence_message = 1;
}
```

## 设置群精华消息

设置群精华消息。

### 参数

- **方法名**: `SetEssenceMessage`
- **请求类型**: `SetEssenceMessageRequest`
- **响应类型**: `SetEssenceMessageResponse`

### 请求与响应

```protobuf
message SetEssenceMessageRequest {
  uint64 group_id = 1;
  uint64 message_id = 2; // 要设置为精华消息的消息ID
}

message SetEssenceMessageResponse {
}
```