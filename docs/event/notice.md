# 通知事件

用于上报QQ的撤回事件，禁言事件，诸如此类。

## 事件类型

```protobuf
enum NoticeType {
  FRIEND_POKE = 0; // 好友头像戳一戳
  FRIEND_RECALL = 1; // 好友消息撤回
  FRIEND_FILE_COME = 2; // 私聊文件上传

  GROUP_POKE = 12; // 群头像戳一戳
  GROUP_CARD_CHANGED = 13; // 群名片改变
  GROUP_MEMBER_UNIQUE_TITLE_CHANGED = 14; // 群成员专属头衔改变
  GROUP_ESSENCE_CHANGED = 15; // 群精华消息改变
  GROUP_RECALL = 16; // 群消息撤回
  GROUP_MEMBER_INCREASE = 17; // 群成员增加
  GROUP_MEMBER_DECREASE = 18; // 群成员减少
  GROUP_ADMIN_CHANGED = 19; // 群管理员变动
  GROUP_MEMBER_BANNED = 20; // 群成员被禁言
  GROUP_SIGN = 21; // 群签到
  GROUP_WHOLE_BAN = 22; // 群全员禁言
  GROUP_FILE_COME = 23; // 群文件上传
}
```

详细的事件内容请参考[通知事件内容](/protos/src/main/proto/kritor/event/comm_notice.proto)。