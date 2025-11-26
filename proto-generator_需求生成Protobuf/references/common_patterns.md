# 常见业务模式的 Proto 定义

## 用户管理模式

### 基础用户实体

```protobuf
message User {
  string user_id = 1;
  string username = 2;
  string email = 3;
  string phone = 4;
  string display_name = 5;
  string avatar_url = 6;
  UserStatus status = 7;
  google.protobuf.Timestamp created_at = 8;
  google.protobuf.Timestamp updated_at = 9;
}

enum UserStatus {
  USER_STATUS_UNSPECIFIED = 0;
  USER_STATUS_ACTIVE = 1;
  USER_STATUS_INACTIVE = 2;
  USER_STATUS_SUSPENDED = 3;
  USER_STATUS_DELETED = 4;
}
```

### 用户服务接口

```protobuf
service UserService {
  rpc CreateUser(CreateUserRequest) returns (CreateUserResponse);
  rpc GetUser(GetUserRequest) returns (GetUserResponse);
  rpc UpdateUser(UpdateUserRequest) returns (UpdateUserResponse);
  rpc DeleteUser(DeleteUserRequest) returns (DeleteUserResponse);
  rpc ListUsers(ListUsersRequest) returns (ListUsersResponse);
  rpc SearchUsers(SearchUsersRequest) returns (SearchUsersResponse);
}

message CreateUserRequest {
  string username = 1;
  string email = 2;
  string phone = 3;
  string display_name = 4;
  string password = 5;
}

message CreateUserResponse {
  User user = 1;
}

message GetUserRequest {
  string user_id = 1;
}

message GetUserResponse {
  User user = 1;
}

message ListUsersRequest {
  int32 page_size = 1;
  string page_token = 2;
  string filter = 3;
  string order_by = 4;
}

message ListUsersResponse {
  repeated User users = 1;
  string next_page_token = 2;
  int32 total_count = 3;
}
```

## 电商业务模式

### 商品管理

```protobuf
message Product {
  string product_id = 1;
  string name = 2;
  string description = 3;
  string sku = 4;
  repeated string categories = 5;
  double price = 6;
  string currency = 7;
  int32 stock_quantity = 8;
  repeated string image_urls = 9;
  ProductStatus status = 10;
  map<string, string> attributes = 11;
  google.protobuf.Timestamp created_at = 12;
  google.protobuf.Timestamp updated_at = 13;
}

enum ProductStatus {
  PRODUCT_STATUS_UNSPECIFIED = 0;
  PRODUCT_STATUS_ACTIVE = 1;
  PRODUCT_STATUS_INACTIVE = 2;
  PRODUCT_STATUS_OUT_OF_STOCK = 3;
  PRODUCT_STATUS_DISCONTINUED = 4;
}
```

### 订单管理

```protobuf
message Order {
  string order_id = 1;
  string user_id = 2;
  repeated OrderItem items = 3;
  double total_amount = 4;
  string currency = 5;
  OrderStatus status = 6;
  Address shipping_address = 7;
  Address billing_address = 8;
  PaymentInfo payment_info = 9;
  google.protobuf.Timestamp created_at = 10;
  google.protobuf.Timestamp updated_at = 11;
}

message OrderItem {
  string product_id = 1;
  string product_name = 2;
  int32 quantity = 3;
  double unit_price = 4;
  double total_price = 5;
}

enum OrderStatus {
  ORDER_STATUS_UNSPECIFIED = 0;
  ORDER_STATUS_PENDING = 1;
  ORDER_STATUS_CONFIRMED = 2;
  ORDER_STATUS_PROCESSING = 3;
  ORDER_STATUS_SHIPPED = 4;
  ORDER_STATUS_DELIVERED = 5;
  ORDER_STATUS_CANCELLED = 6;
  ORDER_STATUS_REFUNDED = 7;
}

message Address {
  string street = 1;
  string city = 2;
  string state = 3;
  string postal_code = 4;
  string country = 5;
}

message PaymentInfo {
  string payment_method = 1;
  string transaction_id = 2;
  PaymentStatus status = 3;
}

enum PaymentStatus {
  PAYMENT_STATUS_UNSPECIFIED = 0;
  PAYMENT_STATUS_PENDING = 1;
  PAYMENT_STATUS_COMPLETED = 2;
  PAYMENT_STATUS_FAILED = 3;
  PAYMENT_STATUS_REFUNDED = 4;
}
```

## 内容管理模式

### 文章/博客系统

```protobuf
message Article {
  string article_id = 1;
  string title = 2;
  string content = 3;
  string summary = 4;
  string author_id = 5;
  repeated string tags = 6;
  string category = 7;
  ArticleStatus status = 8;
  int32 view_count = 9;
  int32 like_count = 10;
  string featured_image_url = 11;
  google.protobuf.Timestamp published_at = 12;
  google.protobuf.Timestamp created_at = 13;
  google.protobuf.Timestamp updated_at = 14;
}

enum ArticleStatus {
  ARTICLE_STATUS_UNSPECIFIED = 0;
  ARTICLE_STATUS_DRAFT = 1;
  ARTICLE_STATUS_PUBLISHED = 2;
  ARTICLE_STATUS_ARCHIVED = 3;
  ARTICLE_STATUS_DELETED = 4;
}
```

### 评论系统

```protobuf
message Comment {
  string comment_id = 1;
  string article_id = 2;
  string user_id = 3;
  string content = 4;
  string parent_comment_id = 5;  // 用于回复
  CommentStatus status = 6;
  int32 like_count = 7;
  google.protobuf.Timestamp created_at = 8;
  google.protobuf.Timestamp updated_at = 9;
}

enum CommentStatus {
  COMMENT_STATUS_UNSPECIFIED = 0;
  COMMENT_STATUS_ACTIVE = 1;
  COMMENT_STATUS_HIDDEN = 2;
  COMMENT_STATUS_DELETED = 3;
}
```

## 权限管理模式

### 角色权限系统

```protobuf
message Role {
  string role_id = 1;
  string name = 2;
  string description = 3;
  repeated string permissions = 4;
  google.protobuf.Timestamp created_at = 5;
  google.protobuf.Timestamp updated_at = 6;
}

message Permission {
  string permission_id = 1;
  string name = 2;
  string resource = 3;
  string action = 4;
  string description = 5;
}

message UserRole {
  string user_id = 1;
  string role_id = 2;
  google.protobuf.Timestamp assigned_at = 3;
  google.protobuf.Timestamp expires_at = 4;
}
```

## 文件管理模式

### 文件上传和管理

```protobuf
message File {
  string file_id = 1;
  string filename = 2;
  string original_name = 3;
  string mime_type = 4;
  int64 size = 5;
  string url = 6;
  string thumbnail_url = 7;
  string hash = 8;
  string uploader_id = 9;
  FileStatus status = 10;
  google.protobuf.Timestamp created_at = 11;
}

enum FileStatus {
  FILE_STATUS_UNSPECIFIED = 0;
  FILE_STATUS_UPLOADING = 1;
  FILE_STATUS_ACTIVE = 2;
  FILE_STATUS_DELETED = 3;
}

service FileService {
  rpc UploadFile(stream UploadFileRequest) returns (UploadFileResponse);
  rpc GetFile(GetFileRequest) returns (GetFileResponse);
  rpc DeleteFile(DeleteFileRequest) returns (DeleteFileResponse);
  rpc ListFiles(ListFilesRequest) returns (ListFilesResponse);
}

message UploadFileRequest {
  oneof data {
    FileMetadata metadata = 1;
    bytes chunk = 2;
  }
}

message FileMetadata {
  string filename = 1;
  string mime_type = 2;
  int64 size = 3;
}
```

## 通知系统模式

### 消息通知

```protobuf
message Notification {
  string notification_id = 1;
  string user_id = 2;
  string title = 3;
  string content = 4;
  NotificationType type = 5;
  NotificationStatus status = 6;
  map<string, string> data = 7;
  google.protobuf.Timestamp created_at = 8;
  google.protobuf.Timestamp read_at = 9;
}

enum NotificationType {
  NOTIFICATION_TYPE_UNSPECIFIED = 0;
  NOTIFICATION_TYPE_SYSTEM = 1;
  NOTIFICATION_TYPE_USER = 2;
  NOTIFICATION_TYPE_ORDER = 3;
  NOTIFICATION_TYPE_PROMOTION = 4;
}

enum NotificationStatus {
  NOTIFICATION_STATUS_UNSPECIFIED = 0;
  NOTIFICATION_STATUS_UNREAD = 1;
  NOTIFICATION_STATUS_READ = 2;
  NOTIFICATION_STATUS_ARCHIVED = 3;
}
```

## 日志和审计模式

### 操作日志

```protobuf
message AuditLog {
  string log_id = 1;
  string user_id = 2;
  string action = 3;
  string resource_type = 4;
  string resource_id = 5;
  map<string, string> old_values = 6;
  map<string, string> new_values = 7;
  string ip_address = 8;
  string user_agent = 9;
  google.protobuf.Timestamp timestamp = 10;
}

service AuditService {
  rpc CreateAuditLog(CreateAuditLogRequest) returns (CreateAuditLogResponse);
  rpc GetAuditLogs(GetAuditLogsRequest) returns (GetAuditLogsResponse);
}
```

## 配置管理模式

### 系统配置

```protobuf
message Configuration {
  string key = 1;
  string value = 2;
  ConfigType type = 3;
  string description = 4;
  bool is_sensitive = 5;
  google.protobuf.Timestamp updated_at = 6;
}

enum ConfigType {
  CONFIG_TYPE_UNSPECIFIED = 0;
  CONFIG_TYPE_STRING = 1;
  CONFIG_TYPE_INTEGER = 2;
  CONFIG_TYPE_BOOLEAN = 3;
  CONFIG_TYPE_JSON = 4;
}
```

## 搜索和过滤模式

### 通用搜索请求

```protobuf
message SearchRequest {
  string query = 1;
  repeated SearchFilter filters = 2;
  SearchSort sort = 3;
  int32 page_size = 4;
  string page_token = 5;
}

message SearchFilter {
  string field = 1;
  SearchOperator operator = 2;
  repeated string values = 3;
}

enum SearchOperator {
  SEARCH_OPERATOR_UNSPECIFIED = 0;
  SEARCH_OPERATOR_EQUALS = 1;
  SEARCH_OPERATOR_NOT_EQUALS = 2;
  SEARCH_OPERATOR_GREATER_THAN = 3;
  SEARCH_OPERATOR_LESS_THAN = 4;
  SEARCH_OPERATOR_CONTAINS = 5;
  SEARCH_OPERATOR_IN = 6;
  SEARCH_OPERATOR_NOT_IN = 7;
}

message SearchSort {
  string field = 1;
  SortDirection direction = 2;
}

enum SortDirection {
  SORT_DIRECTION_UNSPECIFIED = 0;
  SORT_DIRECTION_ASC = 1;
  SORT_DIRECTION_DESC = 2;
}

message SearchResponse {
  repeated google.protobuf.Any results = 1;
  string next_page_token = 2;
  int32 total_count = 3;
  SearchMetadata metadata = 4;
}

message SearchMetadata {
  int32 took_ms = 1;
  map<string, int32> facets = 2;
}
```

## 健康检查模式

### 服务健康检查

```protobuf
service HealthService {
  rpc Check(HealthCheckRequest) returns (HealthCheckResponse);
  rpc Watch(HealthCheckRequest) returns (stream HealthCheckResponse);
}

message HealthCheckRequest {
  string service = 1;
}

message HealthCheckResponse {
  HealthStatus status = 1;
  map<string, string> details = 2;
}

enum HealthStatus {
  HEALTH_STATUS_UNKNOWN = 0;
  HEALTH_STATUS_SERVING = 1;
  HEALTH_STATUS_NOT_SERVING = 2;
  HEALTH_STATUS_SERVICE_UNKNOWN = 3;
}
```