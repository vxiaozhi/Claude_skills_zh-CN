---
name: proto-generator
description: 读取项目需求描述文档，分析业务逻辑和数据结构，生成符合 Google Protobuf 规范的 .proto 文件，包含完整的 RPC 接口定义和相关数据结构
allowed-tools: 
disable: false
---

# Proto Generator

根据项目需求描述自动生成符合 Google Protobuf 规范的协议接口文件。该技能能够分析需求文档中的业务逻辑、数据结构和接口需求，生成包含 RPC 服务定义和消息类型的完整 .proto 文件。

## 何时使用

当需要以下任务时使用此技能：

- 根据需求文档生成 protobuf 协议文件
- 将业务需求转换为标准化的 RPC 接口定义
- 为微服务架构设计协议接口
- 基于 API 文档或需求规格生成 proto 文件
- 需要符合 Google Protobuf 规范的接口定义

## 使用方法

### 基本工作流程

1. **需求分析**: 分析项目目录下的需求文档
2. **结构提取**: 自动识别业务实体、接口操作和数据关系
3. **Proto生成**: 生成符合规范的 .proto 文件
4. **验证检查**: 自动验证生成的 proto 文件语法和结构

### 支持的需求文档格式

- Markdown 文件 (*.md)
- 文本文件 (*.txt)
- API 规格文档 (OpenAPI/Swagger)

### 生成的 Proto 文件特性

- 符合 Google Protobuf 3 语法规范
- 包含完整的 RPC 服务定义
- 包含所有依赖的消息类型定义
- 支持嵌套消息和枚举类型
- 包含适当的字段编号和类型映射
- 添加必要的注释和文档

## 资源文件

- `references/protobuf_best_practices.md`: Protobuf 最佳实践指南
- `references/common_patterns.md`: 常见业务模式的 Proto 定义
- `assets/proto_templates/`: Proto 文件模板集合

## 使用示例

```bash
# 验证生成的 proto 文件
protoc --proto_path=. --descriptor_set_out=/dev/null api.proto
```

## 注意事项

- 确保需求文档包含足够的业务逻辑和数据结构信息
- 生成的 proto 文件可能需要根据具体业务场景进行微调
- 建议在生成后使用 protoc 编译器验证语法正确性
- 对于复杂的业务场景，可能需要手动调整消息类型和服务定义