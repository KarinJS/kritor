<!-- This Source Code Form is subject to the terms of the Mozilla Public
   - License, v. 2.0. If a copy of the MPL was not distributed with this
   - file, You can obtain one at https://mozilla.org/MPL/2.0/. -->

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
message FriendInfo {
  string uid = 1; // uid 'u_*********'
  uint64 uin = 2; // qq 如果没有可不提供
  string qid = 3; // qid 用户自定义的qid 如若没有则为空

  string nick = 4;     // 名称
  string remark = 5;   // 备注
  uint32 level = 6;    // 等级
  uint32 age = 7;      // 年龄
  uint32 vote_cnt = 8; // 赞数量
  int32 gender = 9;    // 性别 可不提供
  int32 group_id = 10; // 好友分组id

  
  optional ExtInfo ext = 99; // 扩展信息，根据协议平台提供的扩展信息
}

message ProfileCard {
  string uid = 1;
  uint64 uin = 2;
  string qid = 3;

  string nick = 4;
  optional string remark = 5;
  uint32 level = 6;
  optional uint64 birthday = 7;
  uint32 login_day = 8; // 登录天数
  uint32 vote_cnt = 9;  // 点赞数

  /* 以下字段可以不实现 */
  optional bool is_school_verified = 51;
  optional ExtInfo ext = 99; // 扩展信息，根据协议平台提供的扩展信息
}

// 通用好友信息扩展字段
// 所有第三方协议分发扩展字段，
// 必须基于本字段修改，
// 并保存定制的副本到本仓库特定路径！！
message ExtInfo {
  optional bool big_vip = 1;       // 大会员
  optional bool hollywood_vip = 2; // 好莱坞/腾讯视频会员
  optional bool qq_vip = 3;        // QQ会员
  optional bool super_vip = 4;     // QQ超级会员
  optional bool voted = 5;         // 是否已经赞过
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

## 获取好友名片

获取好友的名片信息，该接口仅仅适用于好友。

### 参数

- **方法名**: `GetFriendProfileCard`
- **请求类型**: `GetFriendProfileCardRequest`
- **响应类型**: `GetFriendProfileCardResponse`

### 请求与响应

```protobuf
message GetFriendProfileCardRequest {
  repeated string target_uids = 1;
  repeated uint64 target_uins = 2;
}

message GetFriendProfileCardResponse {
  repeated ProfileCard friends_profile_card = 1;
}
```

## 获取陌生人信息

获取陌生人的信息，该接口适用于陌生人和好友。

### 参数

- **方法名**: `GetStrangerProfileCard`
- **请求类型**: `GetStrangerProfileCardRequest`
- **响应类型**: `GetStrangerProfileCardResponse`

### 请求与响应

```protobuf
message GetStrangerProfileCardRequest {
  repeated string target_uids = 1;
  repeated uint64 target_uins = 2;
}

message GetStrangerProfileCardResponse {
  repeated ProfileCard strangers_profile_card = 1;
}
```

## 设置Bot的名片信息

设置Bot的名片信息，该接口仅提供较为基础的设置操作。

### 参数

- **方法名**: `SetProfileCard`
- **请求类型**: `SetProfileCardRequest`
- **响应类型**: `SetProfileCardResponse`

### 请求与响应

```protobuf
message SetProfileCardRequest {
  optional string nick_name = 1;
  optional string company = 2;
  optional string email = 3;
  optional string college = 4;
  optional string personal_note = 5;
  optional uint32 birthday = 6;
  optional uint32 age = 7;
}

message SetProfileCardResponse {
}
```

## 是否是黑名单用户

判断是否是黑名单用户。

### 参数

- **方法名**: `IsBlackListUser`
- **请求类型**: `IsBlackListUserRequest`
- **响应类型**: `IsBlackListUserResponse`

### 请求与响应

```protobuf
message IsBlackListUserRequest {
  oneof target {
    string target_uid = 1;
    uint64 target_uin = 2;
  }
}

message IsBlackListUserResponse {
  bool is_black_list_user = 1;
}
```

## 点赞

点赞好友或陌生人。

### 参数

- **方法名**: `VoteUser`
- **请求类型**: `VoteUserRequest`
- **响应类型**: `VoteUserResponse`

### 请求与响应

```protobuf
message VoteUserRequest {
  oneof target {
    string target_uid = 1;
    uint64 target_uin = 2;
  }
  uint32 vote_count = 3;
}

message VoteUserResponse {
}
```

## 获取Uid通过QQ号（缓存）

获取Uid通过QQ号，通过Kritor端本地缓存。

### 参数

- **方法名**: `GetUidByUin`
- **请求类型**: `GetUidByUinRequest`
- **响应类型**: `GetUidByUinResponse`

### 请求与响应

```protobuf
message GetUidByUinRequest {
  repeated uint64 target_uins = 1;
}

message GetUidByUinResponse {
  map<uint64, string> uid_map = 1;
}
```

## 获取Uin通过Uid（缓存）

获取Uin通过Uid，通过Kritor端本地缓存。

### 参数

- **方法名**: `GetUinByUid`
- **请求类型**: `GetUinByUidRequest`
- **响应类型**: `GetUinByUidResponse`

### 请求与响应

```protobuf
message GetUinByUidRequest {
  repeated string target_uids = 1;
}

message GetUinByUidResponse {
  map<string, uint64> uin_map = 1;
}
```