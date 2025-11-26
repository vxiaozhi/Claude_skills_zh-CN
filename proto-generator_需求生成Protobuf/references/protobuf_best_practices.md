# Protocol Buffers 最佳实践指南

## 语法和结构

### 基本语法规范

1. **使用 proto3 语法**
   ```protobuf
   syntax = "proto3";
   ```

2. **包命名规范**
   ```protobuf
   package com.company.project.api.v1;
   ```
   - 使用小写字母
   - 使用点分隔的层次结构
   - 包含版本信息

3. **导入语句**
   ```protobuf
   import "google/protobuf/timestamp.proto";
   import "google/protobuf/empty.proto";
   ```

## 消息定义最佳实践

### 字段命名

1. **使用 snake_case**
   ```protobuf
   message User {
     string user_name = 1;
     int64 user_id = 2;
     google.protobuf.Timestamp created_at = 3;
   }
   ```

2. **字段编号分配**
   - 1-15: 使用单字节编码，分配给最常用字段
   - 16-2047: 使用双字节编码
   - 19000-19999: 保留字段，不要使用
   - 保持字段编号的连续性和逻辑性

3. **字段类型选择**
   ```protobuf
   // 推荐的类型映射
   string name = 1;           // 文本
   int32 count = 2;           // 32位整数
   int64 id = 3;              // 64位整数（ID）
   double price = 4;          // 浮点数
   bool is_active = 5;        // 布尔值
   bytes data = 6;            // 二进制数据
   repeated string tags = 7;   // 数组
   ```

### 消息设计原则

1. **保持消息简洁**
   - 每个消息应该有明确的职责
   - 避免过于复杂的嵌套结构

2. **使用嵌套消息**
   ```protobuf
   message Order {
     string order_id = 1;
     Customer customer = 2;
     repeated OrderItem items = 3;
     
     message OrderItem {
       string product_id = 1;
       int32 quantity = 2;
       double price = 3;
     }
   }
   ```

3. **枚举定义**
   ```protobuf
   enum Status {
     STATUS_UNSPECIFIED = 0;  // 默认值
     STATUS_PENDING = 1;
     STATUS_APPROVED = 2;
     STATUS_REJECTED = 3;
   }
   ```

## 服务定义最佳实践

### RPC 方法命名

1. **使用动词+名词格式**
   ```protobuf
   service UserService {
     rpc CreateUser(CreateUserRequest) returns (CreateUserResponse);
     rpc GetUser(GetUserRequest) returns (GetUserResponse);
     rpc UpdateUser(UpdateUserRequest) returns (UpdateUserResponse);
     rpc DeleteUser(DeleteUserRequest) returns (DeleteUserResponse);
     rpc ListUsers(ListUsersRequest) returns (ListUsersResponse);
   }
   ```

2. **标准 CRUD 操作**
   - Create: 创建资源
   - Get: 获取单个资源
   - Update: 更新资源
   - Delete: 删除资源
   - List: 列出资源集合

### 请求/响应消息设计

1. **请求消息**
   ```protobuf
   message CreateUserRequest {
     string name = 1;
     string email = 2;
     string phone = 3;
   }
   
   message GetUserRequest {
     string user_id = 1;
   }
   
   message ListUsersRequest {
     int32 page_size = 1;
     string page_token = 2;
     string filter = 3;
   }
   ```

2. **响应消息**
   ```protobuf
   message CreateUserResponse {
     User user = 1;
   }
   
   message GetUserResponse {
     User user = 1;
   }
   
   message ListUsersResponse {
     repeated User users = 1;
     string next_page_token = 2;
     int32 total_count = 3;
   }
   ```

3. **错误处理**
   ```protobuf
   message ErrorResponse {
     int32 code = 1;
     string message = 2;
     repeated ErrorDetail details = 3;
   }
   
   message ErrorDetail {
     string field = 1;
     string description = 2;
   }
   ```

## 版本控制和兼容性

### 向后兼容性规则

1. **不要更改现有字段的编号**
2. **不要更改现有字段的类型**
3. **可以添加新字段**
4. **可以删除字段，但不要重用字段编号**

### 版本管理策略

1. **包版本控制**
   ```protobuf
   package api.v1;  // 第一版
   package api.v2;  // 第二版
   ```

2. **字段弃用**
   ```protobuf
   message User {
     string name = 1;
     string email = 2;
     string old_field = 3 [deprecated = true];  // 标记为弃用
     string new_field = 4;
   }
   ```

## 性能优化

### 字段编号优化

1. **高频字段使用小编号**
   ```protobuf
   message User {
     string id = 1;        // 最常用
     string name = 2;      // 常用
     string email = 3;     // 常用
     string description = 15;  // 不常用
   }
   ```

### 消息大小优化

1. **使用合适的数据类型**
   - 使用 `int32` 而不是 `int64`（如果值范围允许）
   - 使用 `float` 而不是 `double`（如果精度允许）

2. **避免深度嵌套**
   - 限制嵌套层级
   - 考虑使用引用而不是嵌套

## 文档和注释

### 注释规范

1. **消息注释**
   ```protobuf
   // 用户信息
   // 包含用户的基本信息和状态
   message User {
     // 用户唯一标识符
     string user_id = 1;
     
     // 用户显示名称
     string display_name = 2;
   }
   ```

2. **服务注释**
   ```protobuf
   // 用户管理服务
   // 提供用户的 CRUD 操作接口
   service UserService {
     // 创建新用户
     // 返回创建的用户信息
     rpc CreateUser(CreateUserRequest) returns (CreateUserResponse);
   }
   ```

## 常用模式

### 分页模式

```protobuf
message ListRequest {
  int32 page_size = 1;
  string page_token = 2;
}

message ListResponse {
  repeated Item items = 1;
  string next_page_token = 2;
}
```

### 批量操作模式

```protobuf
message BatchCreateRequest {
  repeated CreateRequest requests = 1;
}

message BatchCreateResponse {
  repeated CreateResponse responses = 1;
}
```

### 过滤和排序模式

```protobuf
message ListRequest {
  string filter = 1;     // "status=active AND created_at>2023-01-01"
  string order_by = 2;   // "created_at desc"
  int32 page_size = 3;
  string page_token = 4;
}
```

## 工具和验证

### 编译验证

```bash
# 验证 proto 文件语法
protoc --proto_path=. --descriptor_set_out=/dev/null *.proto

# 生成代码
protoc --proto_path=. --go_out=. --go-grpc_out=. *.proto
```

### 格式化工具

```bash
# 使用 clang-format 格式化
clang-format -i *.proto
```