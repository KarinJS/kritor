# Kritor扩展服务

由Kritor端提供的几个扩展功能。

## 基础信息

- **服务名**: `DeveloperService`
- **Java包名**: `io.kritor.core`
- **C#命名空间**: `Kritor.Core`
- **[source proto file](/protos/src/main/proto/kritor/core/core.proto)**

## 清理缓存

清理Kritor的缓存。

### 参数

- **方法名**: `ClearCache`
- **请求类型**: `ClearCacheRequest`
- **响应类型**: `ClearCacheResponse`

### 请求与响应

```protobuf
message ClearCacheRequest {}

message ClearCacheResponse {}
```

## 获取设备电池状态

获取设备电池状态。

### 参数

- **方法名**: `GetDeviceBattery`
- **请求类型**: `GetDeviceBatteryRequest`
- **响应类型**: `GetDeviceBatteryResponse`

### 请求与响应

```protobuf
message GetDeviceBatteryRequest {}

message GetDeviceBatteryResponse {
  uint32 battery = 1; // 设备电量
  uint32 scale = 2;
  uint32 status = 3;
}
```