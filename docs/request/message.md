<!-- This Source Code Form is subject to the terms of the Mozilla Public
   - License, v. 2.0. If a copy of the MPL was not distributed with this
   - file, You can obtain one at https://mozilla.org/MPL/2.0/. -->

# 消息接口服务

提供发消息，获取消息相关的服务。

## 基础信息

- **服务名**: `MessageService`
- **Java包名**: `io.kritor.message`
- **C#命名空间**: `Kritor.Message`
- **[source proto file](/protos/src/main/proto/kritor/message/message.proto)**

## 基础定义

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
  kritor.common.Contact contact = 1; // 发送目标
  repeated kritor.common.Element elements = 2; // 发的什么东西
  optional uint32 retry_count = 3; // 重试次数
}

message SendMessageResponse {
  string message_id = 1; // 发送成功后的消息ID
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
  kritor.common.Contact contact = 1; // 发送目标
  string res_id = 2; // 资源ID
  optional uint32 retry_count = 3; // 重试次数
}

message SendMessageByResIdResponse {
  string message_id = 1; // 发送成功后的消息ID
  uint32 message_time = 2; // 发送时间
}
```

## 置消息已读

将指定会话的消息设置为已读状态。

### 参数

- **方法名**: `SetMessageRead`
- **请求类型**: `SetMessageReadRequest`
- **响应类型**: `SetMessageReadResponse`

### 请求与响应

```protobuf
message SetMessageReadRequest {
  kritor.common.Contact contact = 1; // 要清空的目标
}

message SetMessageReadResponse {
}
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

## 在消息下回应表情

在消息的底下回应一个表情。

### 参数

- **方法名**: `SetMessageCommentEmoji`
- **请求类型**: `ReactMessageWithEmojiRequest`
- **响应类型**: `ReactMessageWithEmojiResponse`

### 请求与响应

```protobuf
message ReactMessageWithEmojiRequest {
  kritor.common.Contact contact = 1;
  string message_id = 2; // 要设置的消息ID
  uint32 face_id = 3; // 表情ID
  bool is_comment = 4; // 是否是评论,如果是false，则为撤销
}

message ReactMessageWithEmojiResponse {
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
  kritor.common.Contact contact = 1; // 要获取的目标
  string message_id = 2; // 要获取的消息ID
}

message GetMessageResponse {
  kritor.common.PushMessageBody message = 1; // 获取到的消息
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
  kritor.common.Contact contact = 1; // 要获取的目标
  uint64 message_seq = 2; // 要获取的消息序号
}

message GetMessageBySeqResponse {
  kritor.common.PushMessageBody message = 1; // 获取到的消息
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
  kritor.common.Contact contact = 1; // 要获取的目标
  optional string start_message_id = 2; // 起始消息ID 默认最新一条开始
  optional uint32 count = 3; // 获取数量 默认10
}

message GetHistoryMessageResponse {
  repeated kritor.common.PushMessageBody messages = 1; // 获取到的消息
}
```

## 上传合并转发消息

上传合并转发消息。

### 参数

- **方法名**: `UploadForwardMessage`
- **请求类型**: `UploadForwardMessageRequest`
- **响应类型**: `UploadForwardMessageResponse`

### 请求与响应

```protobuf
message UploadForwardMessageRequest {
  kritor.common.Contact contact = 1;
  repeated kritor.common.ForwardMessageBody messages = 2;
  optional uint32 retry_count = 3;
}

message UploadForwardMessageResponse {
  string res_id = 1;
}
```

## 获取合并转发消息

获取合并转发消息。

### 参数

- **方法名**: `DownloadForwardMessage`
- **请求类型**: `DownloadForwardMessageRequest`
- **响应类型**: `DownloadForwardMessageResponse`

### 请求与响应

```protobuf
message DownloadForwardMessageResponse {
  repeated kritor.common.PushMessageBody messages = 1; // 获取到的消息
}

message DeleteEssenceMessageRequest {
  uint64 group_id = 1;
  string message_id = 2; // 要删除的消息ID
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
message GetEssenceMessageListRequest {
  uint64 group_id = 1;
  uint32 page = 2;
  uint32 page_size = 3;
}

message GetEssenceMessageListResponse {
  repeated kritor.common.EssenceMessageBody messages = 1;
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
  string message_id = 2; // 要设置为精华消息的消息ID
}

message SetEssenceMessageResponse {
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
