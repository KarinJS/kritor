# 鉴权服务

Kritor提供的基础鉴权操作，可避免grpc泄露导致的被骇入操作。

## 基础信息

- **服务名**: `Authentication`
- **Java包名**: `io.kritor`
- **C#命名空间**: `Kritor`
- **[source proto file](/protos/src/main/proto/kritor/authenticate.proto)**

## 鉴权

服务端鉴权操作，用于验证客户端的合法性。

### 参数

- **方法名**: `Auth`
- **请求类型**: `AuthReq`
- **响应类型**: `AuthRsp`

### 请求与响应

```protobuf
enum AuthCode {
  OK = 0;
  NO_ACCOUNT = 1;
  NO_TICKET = 2;
  LOGIC_ERROR = 3;
}

message AuthReq {
  string ticket = 1; // 客户端连接认证ticket
  string account = 2; // 客户端连接认证账号
}

message AuthRsp {
  AuthCode code = 1; // 认证结果
  string msg = 2; // 错误信息
}
```

## 获取是否需要鉴权

获取是否需要在元数据携带鉴权信息。

### 参数

- **方法名**: `GetAuthState`
- **请求类型**: `GetAuthStateReq`
- **响应类型**: `GetAuthStateRsp`

### 请求与响应

```protobuf
message GetAuthStateReq {
  string account = 1; // 客户端连接认证账号
}

message GetAuthStateRsp {
  bool is_required_auth = 1; // 是否需要认证
}
```

## 获取鉴权Ticket （WebUI）

WebUI通过superTicket获取鉴权ticket，用于实现远程控制kritor。

### 参数

- **方法名**: `GetTicket`
- **请求类型**: `GetTicketReq`
- **响应类型**: `GetTicketRsp`

### 请求与响应

```protobuf
message TicketReq {
  string account = 1; // 客户端连接认证账号
  string ticket = 2; // 客户端连接认证super ticket
}

message TicketRsp {
  int32 code = 1;
  string msg = 2;
  repeated string ticket = 3; // 返回的客户端ticket，非super ticket
}
```

## 删除鉴权Ticket （WebUI）

WebUI通过superTicket删除鉴权ticket，用于实现远程控制kritor。

### 参数

- **方法名**: `DeleteTicket`
- **请求类型**: `DeleteTicketReq`
- **响应类型**: `DeleteTicketRsp`

### 请求与响应

```protobuf
message DeleteTicketReq {
  string account = 1; // 客户端连接认证账号
  string ticket = 2; // 客户端连接认证super ticket
  string deleted_ticket = 3;
}

message DeleteTicketRsp {
  int32 code = 1;
  string msg = 2;
}
```

## 添加鉴权Ticket （WebUI）

WebUI通过superTicket添加鉴权ticket，用于实现远程控制kritor。

### 参数

- **方法名**: `AddTicket`
- **请求类型**: `AddTicketReq`
- **响应类型**: `AddTicketRsp`

### 请求与响应

```protobuf
message AddTicketReq {
  string account = 1; // 客户端连接认证账号
  string ticket = 2; // 客户端连接认证super ticket
  string new_ticket = 3;
}

message AddTicketRsp {
  int32 code = 1;
  string msg = 2;
}
```