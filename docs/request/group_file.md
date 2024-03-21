# 群聊文件服务

实现于操作群聊/频道文件。

## 基础信息

- **服务名**: `GroupFileService`
- **Java包名**: `io.kritor.file`
- **C#命名空间**: `Kritor.File`
- **[source proto file](/protos/src/main/proto/kritor/file/group_file.proto)**

## 基础定义

### 文件信息

```protobuf
message File {
  string file_id = 1;
  string file_name = 2;
  uint64 file_size = 3;
  int32 bus_id = 4;
  uint32 upload_time = 5;
  uint32 dead_time = 6;
  uint32 modify_time = 7;
  uint32 download_times = 8;
  uint64 uploader = 9;
  string uploader_name = 10;
  string sha = 11;
  string sha3 = 12;
  string md5 = 13;
}
```
### 文件夹信息

```protobuf
message Folder {
  string folder_id = 1;
  string folder_name = 2;
  uint32 total_file_count = 3;
  uint32 create_time = 4;
  uint64 creator = 5;
  string creator_name = 6;
}
```

## 创建文件夹

创建一个文件夹。

### 参数

- **方法名**: `CreateFolder`
- **请求类型**: `CreateFolderRequest`
- **响应类型**: `CreateFolderResponse`

### 请求与响应

```protobuf
message CreateFolderRequest {
  uint64 group_id = 1; // 群号
  string name = 2; // 文件夹名
}

message CreateFolderResponse {
  string id = 1; // 文件夹id
  uint64 used_space = 2; // 已使用空间
}
```

## 删除文件夹

删除一个文件夹。

### 参数

- **方法名**: `DeleteFolder`
- **请求类型**: `DeleteFolderRequest`
- **响应类型**: `DeleteFolderResponse`

### 请求与响应

```protobuf
message DeleteFolderRequest {
  uint64 group_id = 1; // 群号
  string folder_id = 2; // 文件夹id
}

message DeleteFolderResponse {
}
```

## 删除文件

删除一个文件。

### 参数

- **方法名**: `DeleteFile`
- **请求类型**: `DeleteFileRequest`
- **响应类型**: `DeleteFileResponse`

### 请求与响应

```protobuf
message DeleteFileRequest {
  uint64 group_id = 1; // 群号
  string file_id = 2; // 文件id
  int32 bus_id = 3; // 文件类型ID
}

message DeleteFileResponse {
}
```

## 重命名文件夹

重命名一个文件夹。

### 参数

- **方法名**: `RenameFolder`
- **请求类型**: `RenameFolderRequest`
- **响应类型**: `RenameFolderResponse`

### 请求与响应

```protobuf
message RenameFolderRequest {
  uint64 group_id = 1; // 群号
  string folder_id = 2; // 文件夹id
  string name = 3; // 文件夹名
}

message RenameFolderResponse {
}
```

## 获取文件系统信息

获取文件系统信息，例如文件数量，空间大小限制。

### 参数

- **方法名**: `GetFileSystemInfo`
- **请求类型**: `GetFileSystemInfoRequest`
- **响应类型**: `GetFileSystemInfoResponse`

### 请求与响应

```protobuf
message GetFileSystemInfoRequest {
  uint64 group_id = 1; // 群号
}

message GetFileSystemInfoResponse {
  uint32 file_count = 1; // 文件数量
  uint32 total_count = 2; // 文件数量上限
  uint32 used_space = 3; // 已使用空间
  uint32 total_space = 4; // 空间上限
}
```

## 获取根目录文件/文件夹列表

获取根目录文件/文件夹列表。

### 参数

- **方法名**: `GetRootFilesList`
- **请求类型**: `GetRootFilesListRequest`
- **响应类型**: `GetRootFilesListResponse`

### 请求与响应

```protobuf
message GetRootFilesRequest {
  uint64 group_id = 1; // 群号
}

message GetRootFilesResponse {
  repeated File files = 1; // 文件列表
  repeated Folder folders = 2;
}
```

## 获取子目录文件/文件夹列表

获取子目录文件/文件夹列表。

### 参数

- **方法名**: `GetFiles`
- **请求类型**: `GetFilesRequest`
- **响应类型**: `GetFilesResponse`

### 请求与响应

```protobuf
message GetFilesRequest {
  uint64 group_id = 1; // 群号
  string folder_id = 2; // 文件夹id
}

message GetFilesResponse {
  repeated File files = 1;
  repeated Folder folders = 2;
}
```