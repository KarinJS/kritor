<!-- This Source Code Form is subject to the terms of the Mozilla Public
   - License, v. 2.0. If a copy of the MPL was not distributed with this
   - file, You can obtain one at https://mozilla.org/MPL/2.0/. -->

# 鉴权服务

Kritor提供的基础鉴权操作，可避免grpc泄露导致的被骇入操作。

## 基础信息

- **服务名**: `AuthenticationService`
- **Java包名**: `io.kritor.authentication`
- **C#命名空间**: `Kritor.Authentication`
- **[source proto file](/protos/src/main/proto/kritor/auth/authenticate.proto)**

## 鉴权

服务端鉴权操作，用于验证客户端的合法性。

### 参数

- **方法名**: `Authenticate`
- **请求类型**: `AuthenticateRequest`
- **响应类型**: `AuthenticateResponse`

### 请求与响应

```protobuf
message AuthenticateRequest {
  string account = 1; // 客户端连接认证账号
  string ticket = 2;  // 客户端连接认证ticket
}

message AuthenticateResponse {
  enum AuthenticateResponseCode {
    OK = 0;
    NO_ACCOUNT = 1;
    NO_TICKET = 2;
    LOGIC_ERROR = 3;
  }

  AuthenticateResponseCode code = 1; // 认证结果
  string msg = 2;    // 错误信息
}
```

## 获取是否需要鉴权

获取是否需要在元数据携带鉴权信息。

### 参数

- **方法名**: `GetAuthenticationState`
- **请求类型**: `GetAuthenticationStateRequest`
- **响应类型**: `GetAuthenticationStateResponse`

### 请求与响应

```protobuf
message GetAuthenticationStateRequest {
  string account = 1; // 客户端连接认证账号
}

message GetAuthenticationStateResponse {
  bool is_required = 1; // 是否需要认证
}
```

## 获取鉴权Ticket （WebUI）

WebUI通过superTicket获取鉴权ticket，用于实现远程控制kritor。

### 参数

- **方法名**: `GetTicket`
- **请求类型**: `GetTicketRequest`
- **响应类型**: `GetTicketResponse`

### 请求与响应

```protobuf
enum TicketOperationResponseCode {
  OK = 0;
  ERROR = 1;
}

message GetTicketRequest {
  string account = 1; // 客户端连接认证账号
  string ticket = 2;  // 客户端连接认证super ticket
}

message GetTicketResponse {
  TicketOperationResponseCode code = 1;
  string msg = 2;
  repeated string tickets = 3; // 返回的客户端ticket，非super ticket
}
```

## 删除鉴权Ticket （WebUI）

WebUI通过superTicket删除鉴权ticket，用于实现远程控制kritor。

### 参数

- **方法名**: `DeleteTicket`
- **请求类型**: `DeleteTicketRequest`
- **响应类型**: `DeleteTicketResponse`

### 请求与响应

```protobuf
message DeleteTicketRequest {
  string account = 1; // 客户端连接认证账号
  string ticket = 2;  // 客户端连接认证super ticket
  string delete_ticket = 3;
}

message DeleteTicketResponse {
  TicketOperationResponseCode code = 1;
  string msg = 2;
}
```

## 添加鉴权Ticket （WebUI）

WebUI通过superTicket添加鉴权ticket，用于实现远程控制kritor。

### 参数

- **方法名**: `AddTicket`
- **请求类型**: `AddTicketRequest`
- **响应类型**: `AddTicketResponse`

### 请求与响应

```protobuf
message AddTicketRequest {
  string account = 1; // 客户端连接认证账号
  string ticket = 2;  // 客户端连接认证super ticket
  string new_ticket = 3;
}

message AddTicketResponse {
  TicketOperationResponseCode code = 1;
  string msg = 2;
}
```