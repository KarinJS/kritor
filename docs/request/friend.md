# 好友服务

实现好友相关的服务，提供简化操作的接口。

## 基础信息

- **服务名**: `FriendService`
- **Java包名**: `io.kritor.friend`
- **C#命名空间**: `Kritor.Friend`
- **[source proto file](/protos/src/main/proto/kritor/friend/friend.proto)**

## 基础定义

### 好友信息

```protobuf
message FriendData {
  string uid = 1; // uid 'u_*********'
  uint64 uin = 2; // qq，如果没有可不提供
  string qid = 3; // qid 用户自定义的qid 如若没有则为空

  string nick = 4; // 名称
  uint32 age = 5; // 年龄
  uint32 level = 6; // 等级
  uint32 vote_cnt = 7; // 赞数量
  int32 gender = 8; // 性别 可不提供
  int32 group_id = 9; // 好友分组id
  string remark = 10; // 备注

  optional bytes ext = 99; // 扩展信息，根据协议平台提供的扩展信息
}

// 通用好友信息扩展字段
// 所有第三方协议分发扩展字段，
// 必须基于本字段修改，
// 并保存定制的副本到本仓库特定路径！！
message FriendExt {
  optional bool big_vip = 1; // 大会员
  optional bool hollywood_vip = 2; // 好莱坞/腾讯视频会员
  optional bool qq_vip = 3; // QQ会员
  optional bool super_vip = 4; // QQ超级会员
  optional bool voted = 5; // 是否已经赞过
}
```

## 获取好友列表

获取好友列表。

### 参数

- **方法名**: `GetFriendList`
- **请求类型**: `GetFriendListRequest`
- **响应类型**: `GetFriendListResponse`

### 请求与响应

```protobuf
message GetFriendListRequest {
  optional bool refresh = 1; // 是否刷新好友列表
}

message GetFriendListResponse {
  repeated FriendData friendList = 1;
}
```