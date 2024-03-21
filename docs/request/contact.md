# 联系人服务

用于实现一些基础的对好友/群聊/频道的操作。

## 基础信息

- **服务名**: `ContactService`
- **Java包名**: `io.kritor.contact`
- **C#命名空间**: `Kritor.Contact`
- **[source proto file](/protos/src/main/proto/kritor/contact/contact.proto)**

## 获取好友名片

获取好友的名片信息，该接口仅仅适用于好友。

### 参数

- **方法名**: `GetProfileCard`
- **请求类型**: `ProfileCardRequest`
- **响应类型**: `ProfileCard`

### 请求与响应

```protobuf
message ProfileCardRequest {
  oneof account {
    uint64 account_uin = 1; // 可选，uid和uin必须提供一个，两个都提供优先uin
    string account_uid = 2; // 可选，uid和uin必须提供一个，两个都提供优先uin
  }
}

message ProfileCard {
  string uid = 1;
  uint64 uin = 2;
  string name = 3;
  string remark = 4;
  uint32 level = 5;
  uint64 birthday = 6;
  uint32 login_day = 7; // 登录天数
  uint32 vote_cnt = 8; // 点赞数

  /* 以下字段可以不实现 */
  optional string qid = 50;
  optional bool is_school_verified = 51;
}
```

## 获取陌生人信息

获取陌生人的信息，该接口适用于陌生人和好友。

### 参数

- **方法名**: `GetStrangerInfo`
- **请求类型**: `StrangerInfoRequest`
- **响应类型**: `StrangerInfo`

### 请求与响应

```protobuf
message StrangerInfoRequest {
  uint64 uin = 1;
}

message StrangerInfo {
  string uid = 1;
  uint64 uin = 2;
  string name = 3;
  uint32 level = 4;
  uint32 login_day = 5; // 登录天数
  uint32 vote_cnt = 6; // 点赞数
  /* 以下字段可以不实现 */
  optional string qid = 50;
  optional bool is_school_verified = 51;

  optional bytes ext = 99; // 扩展信息，根据协议平台提供的扩展信息
}

// 通用陌生人信息扩展字段
// 所有第三方协议分发扩展字段，
// 必须基于本字段修改，
// 并保存定制的副本到本仓库特定路径！！
message StrangerExt {
  optional bool big_vip = 1; // 大会员
  optional bool hollywood_vip = 2; // 好莱坞/腾讯视频会员
  optional bool qq_vip = 3; // QQ会员
  optional bool super_vip = 4; // QQ超级会员
  optional bool voted = 5; // 是否已经赞过
}
```

## 获取Uid通过QQ号（缓存）

获取Uid通过QQ号，通过Kritor端本地缓存。

### 参数

- **方法名**: `GetUid`
- **请求类型**: `GetUidRequest`
- **响应类型**: `GetUidResponse`

### 请求与响应

```protobuf
message GetUidRequest {
  repeated uint64 uin = 1;
}

message GetUidResponse {
  map<uint64, string> uid = 1;
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
  repeated string uid = 1;
}

message GetUinByUidResponse {
  map<string, uint64> uin = 1;
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
  uint64 uin = 1;
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
  oneof account {
    uint64 account_uin = 1; // 可选，uid和uin必须提供一个，两个都提供优先uin
    string account_uid = 2; // 可选，uid和uin必须提供一个，两个都提供优先uin
  }
  optional uint32 vote_count = 3; // 可选，点赞数量，默认1
}

message VoteUserResponse {}
```