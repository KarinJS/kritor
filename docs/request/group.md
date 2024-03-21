# 群聊服务

实现群聊相关的服务，提供简化操作的群管理接口。

## 基础信息

- **服务名**: `GroupService`
- **Java包名**: `io.kritor.group`
- **C#命名空间**: `Kritor.Group`
- **[source proto file](/protos/src/main/proto/kritor/group/group.proto)**

## 基础定义

### 群信息

```protobuf
message GroupInfo {
  uint64 group_id = 1;
  string group_name = 2;
  string group_remark = 3;
  uint64 owner = 4;
  repeated uint64 admins = 5;
  uint32 max_member_count = 6;
  uint32 member_count = 7;
  uint64 group_uin = 10;
}
```

### 群成员角色

```protobuf
enum MemberRole {
  ADMIN = 0;
  MEMBER = 1;
  OWNER = 2;
  STRANGER = 3;
}
```

### 群成员信息

```protobuf
message GroupMemberInfo {
  string uid = 1;
  uint64 uin = 2;
  string nick = 3;
  uint32 age = 4;
  string unique_title = 5;
  uint32 unique_title_expire_time = 6;
  string card = 7;
  uint64 join_time = 8;
  uint64 last_active_time = 9;
  uint32 level = 10;
  uint64 shut_up_timestamp = 11;

  optional uint32 distance = 100;
  repeated uint32 honor = 101;
  optional bool unfriendly = 102;
  optional bool card_changeable = 103;
}
```

## 禁言用户

禁言用户，时间的单位一般是秒，时间为0则为解除禁言。

### 参数

- **方法名**: `BanMember`
- **请求类型**: `BanMemberRequest`
- **响应类型**: `BanMemberResponse`

### 请求与响应

```protobuf
message BanMemberRequest {
  uint64 group_id = 1; // 群组ID
  oneof target{
    string target_uid = 2; // 被禁言目标uid
    uint64 target_uin = 3; // 被禁言目标uin
  }
  uint64 duration = 4; // 禁言时长(单位：秒)
}

message BanMemberResponse {
}
```

## 戳一戳用户头像

戳一戳用户头像。

### 参数

- **方法名**: `PokeMember`
- **请求类型**: `PokeMemberRequest`
- **响应类型**: `PokeMemberResponse`

### 请求与响应

```protobuf
message PokeMemberRequest {
  uint64 group_id = 1; // 群组ID
  oneof target{
    string target_uid = 2; // 被戳目标uid
    uint64 target_uin = 3; // 被戳目标uin
  }
}

message PokeMemberResponse {
}
```

## 踢出群成员

踢出群成员。

### 参数

- **方法名**: `KickMember`
- **请求类型**: `KickMemberRequest`
- **响应类型**: `KickMemberResponse`

### 请求与响应

```protobuf
message KickMemberRequest {
  uint64 group_id = 1; // 群组ID
  oneof target{
    string target_uid = 2; // 被踢目标uid
    uint64 target_uin = 3; // 被踢目标uin
  }
  optional bool reject_add_request = 4; // 是否拒绝再次申请 默认false
  optional string kick_reason = 5; // 踢出原因，可选
}

message KickMemberResponse {
}
```

## 离开群聊

离开群聊。

### 参数

- **方法名**: `LeaveGroup`
- **请求类型**: `LeaveGroupRequest`
- **响应类型**: `LeaveGroupResponse`

### 请求与响应

```protobuf
message LeaveGroupRequest {
  uint64 group_id = 1; // 群组ID
}

message LeaveGroupResponse {
}
```

## 修改群成员名片

修改群成员名片。

### 参数

- **方法名**: `ModifyMemberCard`
- **请求类型**: `ModifyMemberCardRequest`
- **响应类型**: `ModifyMemberCardResponse`

### 请求与响应

```protobuf
message ModifyMemberCardRequest {
  uint64 group_id = 1; // 群组ID
  oneof target{
    string target_uid = 2; // 目标uid
    uint64 target_uin = 3; // 目标uin
  }
  string card = 4; // 新的群名片
}

message ModifyMemberCardResponse {
}
```

## 修改群名称

修改群名称。

### 参数

- **方法名**: `ModifyGroupName`
- **请求类型**: `ModifyGroupNameRequest`
- **响应类型**: `ModifyGroupNameResponse`

### 请求与响应

```protobuf
message ModifyGroupNameRequest {
  uint64 group_id = 1; // 群组ID
  string group_name = 2; // 新的群名称
}

message ModifyGroupNameResponse {
}
```

## 修改群备注

修改群备注。

### 参数

- **方法名**: `ModifyGroupRemark`
- **请求类型**: `ModifyGroupRemarkRequest`
- **响应类型**: `ModifyGroupRemarkResponse`

### 请求与响应

```protobuf
message ModifyGroupRemarkRequest {
  uint64 group_id = 1; // 群组ID
  string remark = 2; // 新的群备注
}

message ModifyGroupRemarkResponse {
}
```

## 设置群管理员

设置群管理员。

### 参数

- **方法名**: `SetGroupAdmin`
- **请求类型**: `SetGroupAdminRequest`
- **响应类型**: `SetGroupAdminResponse`

### 请求与响应

```protobuf
message SetGroupAdminRequest {
  uint64 group_id = 1; // 群组ID
  oneof target{
    string target_uid = 2; // 目标uid
    uint64 target_uin = 3; // 目标uin
  }
  bool is_admin = 4; // 是否设置为管理员
}

message SetGroupAdminResponse {
}
```

## 设置群成员头衔

设置群成员专属头衔。

### 参数

- **方法名**: `SetGroupUniqueTitle`
- **请求类型**: `SetGroupUniqueTitleRequest`
- **响应类型**: `SetGroupUniqueTitleResponse`

### 请求与响应

```protobuf
message SetGroupUniqueTitleRequest {
  uint64 group_id = 1; // 群组ID
  oneof target{
    string target_uid = 2; // 目标uid
    uint64 target_uin = 3; // 目标uin
  }
  string unique_title = 4; // 新的群头衔
}

message SetGroupUniqueTitleResponse {
}
```

## 打开/关闭全员禁言

打开/关闭全员禁言。

### 参数

- **方法名**: `SetGroupWholeBan`
- **请求类型**: `SetGroupWholeBanRequest`
- **响应类型**: `SetGroupWholeBanResponse`

### 请求与响应

```protobuf
message SetGroupWholeBanRequest {
  uint64 group_id = 1; // 群组ID
  bool is_ban = 2; // 是否全员禁言
}

message SetGroupWholeBanResponse {
}
```

## 获取群聊信息

获取群聊信息。

### 参数

- **方法名**: `GetGroupInfo`
- **请求类型**: `GetGroupInfoRequest`
- **响应类型**: `GetGroupInfoResponse`

### 请求与响应

```protobuf
message GetGroupInfoRequest {
  uint64 group_id = 1; // 群组ID
}

message GetGroupInfoResponse {
  GroupInfo group_info = 1; // 群组信息
}
```

## 获取群列表

获取群列表。

### 参数

- **方法名**: `GetGroupList`
- **请求类型**: `GetGroupListRequest`
- **响应类型**: `GetGroupListResponse`

### 请求与响应

```protobuf
message GetGroupListRequest {
  optional bool refresh = 1; // 是否刷新缓存
}

message GetGroupListResponse {
  repeated GroupInfo group_info = 1; // 群组信息
}
```

## 获取群成员信息

获取群成员信息。

### 参数

- **方法名**: `GetGroupMemberInfo`
- **请求类型**: `GetGroupMemberInfoRequest`
- **响应类型**: `GetGroupMemberInfoResponse`

### 请求与响应

```protobuf
message GetGroupMemberInfoRequest {
  uint64 group_id = 1; // 群组ID
  oneof target{
    string uid = 2; // 目标uid
    uint64 uin = 3; // 目标uin
  }
  optional bool refresh = 4; // 是否刷新缓存
}

message GetGroupMemberInfoResponse {
  GroupMemberInfo group_member_info = 1; // 群成员信息
}
```

## 获取群成员列表

获取群成员列表。

### 参数

- **方法名**: `GetGroupMemberList`
- **请求类型**: `GetGroupMemberListRequest`
- **响应类型**: `GetGroupMemberListResponse`

### 请求与响应

```protobuf
message GetGroupMemberListRequest {
  uint64 group_id = 1; // 群组ID
  optional bool refresh = 2; // 是否刷新缓存
}

message GetGroupMemberListResponse {
  repeated GroupMemberInfo group_member_info = 1; // 群成员信息
}
```

## 获取被禁言的群成员列表

获取被禁言的群成员列表。

### 参数

- **方法名**: `GetProhibitedUserList`
- **请求类型**: `GetProhibitedUserListRequest`
- **响应类型**: `GetProhibitedUserListResponse`

### 请求与响应

```protobuf
message ProhibitedUserInfo {
  string uid = 1;
  uint64 uin = 2;
  uint32 prohibited_time = 3;
}

message GetProhibitedUserListRequest {
  uint64 group_id = 1; // 群组ID
}

message GetProhibitedUserListResponse {
  repeated ProhibitedUserInfo prohibited_user_info = 1; // 禁言用户信息
}
```

## 获取 `@全体成员` 剩余次数

获取 `@全体成员` 剩余次数。

### 参数

- **方法名**: `GetRemainCountAtAll`
- **请求类型**: `GetRemainCountAtAllRequest`
- **响应类型**: `GetRemainCountAtAllResponse`

### 请求与响应

```protobuf
message GetRemainCountAtAllRequest {
  uint64 group_id = 1; // 群组ID
}

message GetRemainCountAtAllResponse {
  uint32 remain_count_for_group = 1; // 剩余次数对于全群
  bool access_at_all = 2;
  uint32 remain_count_for_self = 3; // 剩余次数对于自己
}
```

## 获取尚未加入的群聊信息

获取尚未加入的群聊信息。

### 参数

- **方法名**: `GetNotJoinedGroupInfo`
- **请求类型**: `GetNotJoinedGroupInfoRequest`
- **响应类型**: `GetNotJoinedGroupInfoResponse`

### 请求与响应

```protobuf
message NotJoinedGroupInfo {
  uint64 group_id = 1;
  uint32 max_member_count = 2;
  uint32 member_count = 3;
  string group_name = 4;
  string group_desc = 5;
  uint64 owner = 6;
  uint32 create_time = 7;

  uint32 group_flag = 8; // 群聊类型什么的都在这里，如果获取不到可以不实现
  uint32 group_flag_ext = 9; // 扩展群聊类型
}

message GetNotJoinedGroupInfoRequest {
  uint64 group_id = 1; // 群号
}

message GetNotJoinedGroupInfoResponse {
  NotJoinedGroupInfo group_info = 1; // 未加入群组信息
}
```

## 获取群荣誉列表

获取群荣誉列表。

### 参数

- **方法名**: `GetGroupHonor`
- **请求类型**: `GetGroupHonorRequest`
- **响应类型**: `GetGroupHonorResponse`

### 请求与响应

```protobuf
message GroupHonorInfo {
  string uid = 1; // 荣誉成员uid
  uint64 uin = 2; // 荣誉成员uin
  string nick = 3; // 荣誉成员昵称
  string honor_name = 4; // 荣誉名称
  string avatar = 5; // 荣誉图标url
  uint32 id = 6; // 荣誉id
  string description = 7; // 荣誉描述
}

message GetGroupHonorRequest {
  uint64 group_id = 1; // 群号
  optional bool refresh = 2; // 是否刷新缓存
}

message GetGroupHonorResponse {
  repeated GroupHonorInfo group_honor_info = 1; // 群荣誉信息
}
```
