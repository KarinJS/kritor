# 请求事件

用于上报QQ的好友申请，加群申请，诸如此类。

## 事件类型

```protobuf
enum RequestType {
  FRIEND_APPLY = 0;
  GROUP_APPLY = 1;
  INVITED_GROUP = 2;
}
```

详细的事件内容请参考[通知事件内容](/protos/src/main/proto/kritor/event/comm_request.proto)。
