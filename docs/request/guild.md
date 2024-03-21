# 频道服务

实现频道相关的服务，提供简化操作的频道管理接口。

## 基础信息

- **服务名**: `GuildService`
- **Java包名**: `io.kritor.guild`
- **C#命名空间**: `Kritor.Guild`
- **[source proto file](/protos/src/main/proto/kritor/guild/guild.proto)**

## 基础定义

### 频道信息  

```protobuf
message GuildInfo {
  uint64 guild_id = 1; // 频道ID
  string guild_name = 2; // 频道名称
  string guild_display_id = 3; // 频道显示ID
  string profile = 4; // 频道简介
  bool is_enable = 5; // 是否启用
  bool is_banned = 6; // 是否被封禁
  bool is_frozen = 7; // 是否被冻结
  uint64 owner_id = 8; // 频道拥有者ID
  uint64 shutup_expire_time = 9; // 禁言过期时间
  bool allow_search = 10; // 是否允许搜索
}
```

### 子频道信息  

```protobuf
message ChannelInfo {
  uint64 channel_id = 1; // 子频道ID
  uint64 guild_id = 2; // 频道ID
  string channel_name = 3; // 子频道名称
  uint64 create_time = 4; // 创建时间
  uint64 max_member_count = 5; // 最大成员数
  uint64 creator_tiny_id = 6; // 创建者tinyid
  uint64 talk_permission = 7; // 发言权限
  uint64 visible_type = 8; // 可见类型
  uint64 current_slow_mode = 9; // 当前发言限制
  repeated SlowModes slow_modes = 10; // 发言限制
  string icon_url = 11; // 频道图标
  uint64 jump_switch = 12; // 跳转开关
  uint64 jump_type = 13; // 跳转类型
  string jump_url = 14; // 跳转地址
  uint64 category_id = 15; // 分类ID
  uint64 my_talk_permission = 16; // 我的发言权限
}
```

### 频道成员信息  

```protobuf
message MemberInfo {
  uint64 tiny_id = 1; // 用户tinyid
  string title = 2; // 用户头衔
  string nickname = 3; // 用户昵称
  uint64 role_id = 4; // 用户角色ID
  string role_name = 5; // 用户角色名称
  uint64 role_color = 6; // 用户角色颜色
  uint64 join_time = 7; // 加入时间
  uint64 robot_type = 8; // 机器人类型
  uint64 type = 9; // 用户类型
  bool in_black = 10; // 是否在黑名单
  uint64 platform = 11; // 平台
}
```

### 频道成员资料  

```protobuf
message MemberProfile {
  uint64 tiny_id = 1; // 用户tinyid
  string nickname = 2; // 用户昵称
  string avatar_url = 3; // 用户头像
  uint64 join_time = 4; // 加入时间
  repeated RoleInfo roles = 5; // 用户角色
}
```

### 用户角色信息  

```protobuf
message RoleInfo {
  uint64 role_id = 1; // 用户角色ID
  string role_name = 2; // 用户角色名称
  uint64 color = 3; // 用户角色颜色
  repeated PermissionInfo permissions = 4; // 用户角色权限
  uint64 type = 5; // 用户角色类型
  string display_name = 6; // 用户角色显示名称
}
```

### 频道身份组详细信息  

```protobuf
message RolesInfo  {
  uint64 role_id = 1; // 身份组ID
  string role_name = 2; // 身份组名称
  uint64 argb_color = 3; // 身份组颜色
  bool disabled = 4; // 是否禁用
  bool independent = 5; // 是否独立
  uint64 max_count = 6; // 最大成员数
  uint64 member_count = 7; // 成员数
  bool owned = 8; // 是否拥有
  repeated Permission permissions = 9; // 权限
}
```

### 用户角色权限信息  

```protobuf
message PermissionInfo {
  uint64 root_id = 1; // 权限根ID
  repeated uint64 child_ids = 2; // 权限子ID
}
```

### 发言限制  

```protobuf
message SlowModes {
  uint64 slow_mode_key = 1; // 详情见下方
  string slow_mode_text = 2; // 发言限制描述
  uint64 speak_frequency = 3; // 每分钟发言频率
  uint64 slow_mode_circle = 4; // 发言限制周期
}
```

| slow_mode_key | slow_mode_text | speak_frequency | slow_mode_circle |
| ------------- | -------------- | --------------- | ---------------- |
| 0             | 关闭           | 0               | 0                |
| 1             | 每分钟1条      | 1               | 60               |
| 2             | 每分钟2条      | 2               | 60               |
| 3             | 每分钟5条      | 5               | 60               |
| 4             | 每分钟10条     | 10              | 60               |
| 5             | 每5分钟1条     | 1               | 300              |
| 6             | 每10分钟1条    | 1               | 600              |
| 7             | 每15分钟1条    | 1               | 900              |
| 8             | 每30分钟1条    | 1               | 1800             |
| 9             | 每1小时1条     | 1               | 3600             |
| 10            | 每12小时1条    | 1               | 43200            |
| 11            | 每24小时1条    | 1               | 86400            |


## 获取频道系统内BOT的资料

该接口用于获取频道系统内BOT的资料。

### 参数

- **方法名**: `GetBotInfo`
- **请求类型**: `GetBotInfoRequest`
- **响应类型**: `GetBotInfoResponse`

### 请求与响应

```protobuf
message GetBotInfoRequest {
}

message GetBotInfoResponse {
  string nickname = 1; // 昵称
  uint64 tiny_id = 2; // bot的频道id
  string avatar = 3; // 头像
}
```

## 获取频道列表

获取频道列表。

### 参数

- **方法名**: `GetChannelList`
- **请求类型**: `GetChannelListRequest`
- **响应类型**: `GetChannelListResponse`

### 请求与响应

```protobuf
message GetChannelListRequest {
}

message GetChannelListResponse {
  repeated GuildInfo get_guild_list = 1; // 频道列表
}
```

## 通过访客获取频道元数据

获取频道元数据，例如当前成员数量之类。

### 参数

- **方法名**: `GetGuildMetaByGuest`
- **请求类型**: `GetGuildMetaByGuestRequest`
- **响应类型**: `GetGuildMetaByGuestResponse`

### 请求与响应

```protobuf
message GetGuildMetaByGuestRequest {
  uint64 guild_id = 1; // 频道ID
}

message GetGuildMetaByGuestResponse {
  uint64 guild_id = 1; // 频道ID
  string guild_name = 2; // 频道名称
  string guild_profile = 3; // 频道简介
  uint64 create_time = 4; // 创建时间
  uint64 max_member_count = 5; // 最大成员数量
  uint64 max_robot_count = 6; // 最大机器人数量
  uint64 max_admin_count = 7; // 最大管理员数量
  uint64 member_count = 8; // 当前成员数量
  uint64 owner_id = 9; // 频道所有者ID
  string guild_display_id = 10; // 频道显示ID
}
```

## 获取子频道列表

获取一个频道的子频道(channel)列表。

### 参数

- **方法名**: `GetGuildChannelList`
- **请求类型**: `GetGuildChannelListRequest`
- **响应类型**: `GetGuildChannelListResponse`

### 请求与响应

```protobuf
message GetGuildChannelListRequest {
  uint64 guild_id = 1; // 频道ID
  bool refresh = 2; // 是否刷新数据，默认false
}

message GetGuildChannelListResponse {
  repeated ChannelInfo get_guild_list = 1; // 子频道列表
}
```

## 获取频道成员列表

获取一个频道成员列表，但是因为数据量大，可能需要分页。

### 参数

- **方法名**: `GetGuildMemberList`
- **请求类型**: `GetGuildMemberListRequest`
- **响应类型**: `GetGuildMemberListResponse`

### 请求与响应

```protobuf
message GetGuildMemberListRequest {
  uint64 guild_id = 1; // 频道ID
  string next_token = 2; // 不提供则从首页开始获取
  bool all = 3; // 是否一次性获取完所有成员，默认false
  bool refresh = 4; // 是否刷新数据，默认false
}

message GetGuildMemberListResponse {
  repeated MemberInfo get_member_list = 1; // 成员列表
  string next_token = 2; // 下一页的token
  bool finished = 3; // 是否已经获取完所有成员
}
```

## 单独获取频道成员资料

单独获取频道成员信息，附带有权限信息和身份组哦~！

### 参数

- **方法名**: `GetGuildMember`
- **请求类型**: `GetGuildMemberRequest`
- **响应类型**: `GetGuildMemberResponse`

### 请求与响应

```protobuf
message GetGuildMemberRequest {
  uint64 guild_id = 1; // 频道ID
  uint64 tiny_id = 2; // 成员tinyId
}

message GetGuildMemberResponse {
  MemberProfile member_info = 1; // 成员资料
}
```

## 发送信息到子频道

发送频道内信息，需要单独的API哦，不要使用/send_message去发频道消息，发不出去的~~

### 参数

- **方法名**: `SendChannelMessage`
- **请求类型**: `SendChannelMessageRequest`
- **响应类型**: `SendChannelMessageResponse`

### 请求与响应

```protobuf
message SendChannelMessageRequest {
  uint64 guild_id = 1; // 频道ID
  uint64 channel_id = 2; // 子频道ID
  string message = 3; // 消息体
  int32 retry_cnt = 4; // 最大重试次数
  int64 recall_duration = 5; // 自动撤回间隔
}

message SendChannelMessageResponse {
  uint64 message_id = 1; // 消息ID
  int64 time = 2; // 时间
}
```

## 获取话题频道帖子

该API接口已经被遗弃！

## 获取频道帖子广场帖子

新的获取帖子广场的帖子哦！

### 参数

- **方法名**: `GetGuildFeeds`
- **请求类型**: `GetGuildFeedsRequest`
- **响应类型**: `GetGuildFeedsResponse`

### 请求与响应

```protobuf
message GetGuildFeedsRequest {
  uint64 guild_id = 1; // 频道ID
  uint32 from = 2; // 开始获取的位置
}

message GetGuildFeedsResponse {
  bytes data = 1; // 该请求携带了大量原生响应数据，无法详细介绍，请自行测试！
}
```

## 获取频道角色列表

获取身份组列表，包括隐藏的身份组哦~~

### 参数

- **方法名**: `GetGuildRoles`
- **请求类型**: `GetGuildRolesRequest`
- **响应类型**: `GetGuildRolesResponse`

### 请求与响应

```protobuf
message GetGuildRolesRequest {
  uint64 guild_id = 1; // 频道ID
}

message GetGuildRolesResponse {
  repeated RolesInfo get_role_list = 1; // 角色列表
}
```

## 获取频道消息

该接口不会实现，因为您可以调用/get_msg来获取来自频道的消息，无需实现一个专属的接口。

## 删除频道身份组

删除一个身份组，首先，你得保证你有权限，因为这个API不会提供任何返回数据哦！

### 参数

- **方法名**: `DeleteGuildRole`
- **请求类型**: `DeleteGuildRoleRequest`
- **响应类型**: `DeleteGuildRoleResponse`

### 请求与响应

```protobuf
message DeleteGuildRoleRequest {
  uint64 guild_id = 1; // 频道ID
  uint64 role_id = 2; // 角色ID
}

message DeleteGuildRoleResponse {
  // 无返回值
}

```

## 设置用户在频道中的角色

设置用户身份组。

### 参数

- **方法名**: `SetGuildMemberRole`
- **请求类型**: `SetGuildMemberRoleRequest`
- **响应类型**: `SetGuildMemberRoleResponse`

### 请求与响应

```protobuf
message SetGuildMemberRoleRequest {
  uint64 guild_id = 1; // 频道ID
  uint64 role_id = 2; // 角色ID
  bool set = 3; // 设置还是移除，默认false
  repeated string users = 4; // 批量设置用户s
  int64 tiny_id = 5; // 单独设置某个用户的身份
}

message SetGuildMemberRoleResponse {
  // 无返回值
}
```

## 修改频道角色

修改频道角色，暂不支持设置权限，如有需要请提交issue。

### 参数

- **方法名**: `UpdateGuildRole`
- **请求类型**: `UpdateGuildRoleRequest`
- **响应类型**: `UpdateGuildRoleResponse`

### 请求与响应

```protobuf
message UpdateGuildRoleRequest {
  uint64 guild_id = 1; // 频道ID
  uint64 role_id = 2; // 角色ID
  string name = 3; // 名称
  int64 color = 4; // 颜色ARGB
}

message UpdateGuildRoleResponse {
  // 无返回值
}
```

## 创建频道角色

创建频道身份组。

### 参数

- **方法名**: `CreateGuildRole`
- **请求类型**: `CreateGuildRoleRequest`
- **响应类型**: `CreateGuildRoleResponse`

### 请求与响应

```protobuf
message CreateGuildRoleRequest {
  uint64 guild_id = 1; // 频道ID
  string name = 2; // 名称
  int64 color = 3; // 颜色ARGB
  repeated int64 initial_users = 4; // 默认身份组成员
}

message CreateGuildRoleResponse {
  uint64 role_id = 1; // 角色ID
}
```
