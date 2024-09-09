<!-- This Source Code Form is subject to the terms of the Mozilla Public
   - License, v. 2.0. If a copy of the MPL was not distributed with this
   - file, You can obtain one at https://mozilla.org/MPL/2.0/. -->

# 请求事件

用于上报QQ的好友申请，加群申请，诸如此类。

## 事件类型

```protobuf
enum RequestType {
  UNKNOWN = 0; // 保留字段, UNSPECIFIED
  
  FRIEND_APPLY = 10;
  GROUP_APPLY = 11;
  INVITED_GROUP = 12;
}
```

详细的事件内容请参考[通知事件内容](/protos/src/main/proto/kritor/event/comm_request.proto)。
