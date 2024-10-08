<!-- This Source Code Form is subject to the terms of the Mozilla Public
   - License, v. 2.0. If a copy of the MPL was not distributed with this
   - file, You can obtain one at https://mozilla.org/MPL/2.0/. -->

# Kritor核心服务

由Kritor端提供的几个操作Kritor端的基础功能。

## 基础信息

- **服务名**: `CoreService`
- **Java包名**: `io.kritor.core`
- **C#命名空间**: `Kritor.Core`
- **[source proto file](/protos/src/main/proto/kritor/core/core.proto)**

## 获取Kritor版本

获取Kritor的版本信息。

### 参数

- **方法名**: `GetVersion`
- **请求类型**: `GetVersionRequest`
- **响应类型**: `GetVersionResponse`

### 请求与响应

```protobuf
message GetVersionRequest {}

message GetVersionResponse {
  string version = 1; // Kritor版本
  string app_name = 2;
}
```

## 请求Kritor端下载文件

请求Kritor端下载文件。

### 参数

- **方法名**: `DownloadFile`
- **请求类型**: `DownloadFileRequest`
- **响应类型**: `DownloadFileResponse`

### 请求与响应

```protobuf
message DownloadFileRequest {
  optional string url = 1; // 下载文件的URL 二选一
  optional string base64 = 2; // 下载文件的base64 二选一
  optional string root_path = 3; // 下载文件的根目录 需要保证Kritor有该目录访问权限 可选
  optional string file_name = 4; // 保存的文件名称 默认为文件MD5 可选
  optional uint32 thread_cnt = 5; // 下载文件的线程数 默认为3 可选
  map<string, string> headers = 6; // 下载文件的请求头
}

message DownloadFileResponse {
  string file_absolute_path = 1; // 下载文件的绝对路径
  string file_md5 = 2; // 下载文件的MD5
}
```

> `root_path`需要保证Kritor有该目录访问权限，否则会报错。
> 
> `file_name`默认为文件MD5，如果你需要指定文件名，请填写。

## 获取当前账户

获取当前账户的信息。

### 参数

- **方法名**: `GetCurrentAccount`
- **请求类型**: `GetCurrentAccountRequest`
- **响应类型**: `GetCurrentAccountResponse`

### 请求与响应

```protobuf
message GetCurrentAccountRequest {}

message GetCurrentAccountResponse {
  optional string account_uid = 1; // 当前账户
  uint64 account_uin = 2;
  string account_name = 3; // 当前账户名称
}
```

## 切换账户

切换账户。

### 参数

- **方法名**: `SwitchAccount`
- **请求类型**: `SwitchAccountRequest`
- **响应类型**: `SwitchAccountResponse`

### 请求与响应

```protobuf
message SwitchAccountRequest {
  oneof account {
    string account_uid = 1; // 切换账户uid
    uint64 account_uin = 2; // 切换账户uin
  }
  string super_ticket = 3; // 凭证
}

message SwitchAccountResponse {}
```
