<!-- This Source Code Form is subject to the terms of the Mozilla Public
   - License, v. 2.0. If a copy of the MPL was not distributed with this
   - file, You can obtain one at https://mozilla.org/MPL/2.0/. -->

# 通知事件

用于上报QQ的撤回事件，禁言事件，诸如此类。

## 事件类型

```protobuf
enum NoticeType {
  UNKNOWN = 0;

  PRIVATE_POKE = 10; // 私聊头像戳一戳
  PRIVATE_RECALL = 11; // 私聊消息撤回
  PRIVATE_FILE_UPLOADED = 12; // 私聊文件上传

  GROUP_POKE = 20; // 群头像戳一戳
  GROUP_RECALL = 21; // 群消息撤回
  GROUP_FILE_UPLOADED = 22; // 群文件上传
  GROUP_CARD_CHANGED = 23; // 群名片改变
  GROUP_MEMBER_UNIQUE_TITLE_CHANGED = 24; // 群成员专属头衔改变
  GROUP_ESSENCE_CHANGED = 25; // 群精华消息改变
  GROUP_MEMBER_INCREASE = 26; // 群成员增加
  GROUP_MEMBER_DECREASE = 27; // 群成员减少
  GROUP_ADMIN_CHANGED = 28; // 群管理员变动
  GROUP_SIGN_IN = 29; // 群签到
  GROUP_MEMBER_BAN = 30; // 群成员被禁言
  GROUP_WHOLE_BAN = 31; // 群全员禁言
  GROUP_REACT_MESSAGE_WITH_EMOJI = 32; // 群消息被表情回应
  GROUP_TRANSFER = 33; // 群转让

  FRIEND_INCREASE = 40; // 好友增加
  FRIEND_DECREASE = 41; // 好友减少
}
```

详细的事件内容请参考[通知事件内容](/protos/src/main/proto/kritor/event/comm_notice.proto)。