<!-- This Source Code Form is subject to the terms of the Mozilla Public
   - License, v. 2.0. If a copy of the MPL was not distributed with this
   - file, You can obtain one at https://mozilla.org/MPL/2.0/. -->

# 消息事件

消息事件分为`好友`，`群聊`，`频道`，`群临时会话`。

其中对消息事件进行监听前文有所提及。

## 消息来源对象解析

```protobuf
enum Scene {
  UNSPECIFIED = 0; // 保留字段, UNSPECIFIED
  GROUP = 1; // 群聊
  FRIEND = 2; // 好友
  GUILD = 3; // 频道
  STRANGER_FROM_GROUP = 11; // 群临时会话

  // 以下类型为可选实现
  NEARBY = 6; // 附近的人
  STRANGER = 10; // 陌生人
}

message Contact {
  Scene scene = 1;
  string peer = 2; // 群聊则为群号 私聊则为 Uix (Uin 或 Uid) 频道消息则为频道号
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

## 消息事件

下方是一个完整的消息事件结构体：

```protobuf
message PrivateSender {
  optional string uid = 1;
  uint64 uin = 2;
  string nick = 3;
}

message GroupSender {
  string group_id = 1;
  optional string uid = 2;
  uint64 uin = 3;
  string nick = 4;
}

message GuildSender {
  string guild_id = 1;
  string channel_id = 2;
  string tiny_id = 3;
  string nick = 4;
  Role role = 5;
}

message PushMessageBody {
  uint64 time = 1;
  string message_id = 2;
  uint64 message_seq = 3;
  Scene scene = 4;
  oneof sender {
    PrivateSender private = 5;
    GroupSender group = 6;
    GuildSender guild = 7;
  }
  repeated Element elements = 9; // 发的什么东西
}
```
其中`message_id`作为接下来各种操作的主要对象，`message_seq`则是由腾讯维护的一个自增的序列号，用于标识消息的顺序，您也可以使用它反查一些它的`message_id`（**GetMessageBySeq**）。

## 消息元素

消息元素是消息的主要内容，他有很多种类型，下面将列出一个表格：

| 名称          | 说明       | 接收  |
|-------------|----------|-----|
| TEXT        | 文本       | yes |
| AT          | @        | yes |
| FACE        | 表情       | yes |
| BUBBLE_FACE | 弹射表情     | yes |
| REPLY       | 回复       | yes |
| IMAGE       | 图片       | yes |
| VOICE       | 语音       | yes |
| VIDEO       | 视频       | yes |
| BASKETBALL  | 篮球       | yes |
| DICE        | 骰子       | yes |
| RPS         | 石头剪刀布    | yes |
| POKE        | 戳一戳      | yes |
| MUSIC       | 音乐       | yes |
| WEATHER     | 天气       | yes |
| LOCATION    | 位置       | yes |
| SHARE       | 分享       | yes |
| GIFT        | 礼物       | yes |
| MARKET_FACE | 商城表情     | yes |
| FORWARD     | 转发       | yes |
| CONTACT     | 名片       | yes |
| JSON        | JSON     | yes |
| XML         | XML      | yes |
| FILE        | 文件       | yes |
| MARKDOWN    | Markdown | yes |
| BUTTON      | 按钮       | yes |
| NODE        | 节点       | no  |

详细的消息元素定义请参考[消息元素](/protos/src/main/proto/kritor/event/comm_msg.proto)。

> 注意：接收的消息对象只是和发消息的对象名字相同，但是对应结构不同。