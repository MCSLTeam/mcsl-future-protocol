# JSON-RPC 错误

V2 不再使用旧 `retcode` 返回码。成功响应使用 `result`，失败响应使用 `error`；客户端不应解析 daemon exception 或依赖可变的 message 文本进行业务判断。

## 错误形状

```json
{
  "jsonrpc": "2.0",
  "id": "request-1",
  "error": {
    "code": -32601,
    "message": "Method not found",
    "data": {
      "daemon_error_kind": "not_found",
      "correlation_id": "example-correlation-id"
    }
  }
}
```

示例用于展示结构，实际 message 和 correlation ID 由 daemon 生成。

| 字段 | 含义 |
| --- | --- |
| `error.code` | JSON-RPC 层的整数错误码 |
| `error.message` | 面向诊断的错误说明 |
| `error.data.daemon_error_kind` | typed 分类：`validation`、`not_found`、`conflict`、`permission`、`storage`、`transport`、`internal` |
| `error.data.daemon_error_code` | 可选的稳定应用错误标识 |
| `error.data.correlation_id` | 关联服务端诊断的非空标识 |
| `error.data.details` | 可选的结构化附加数据，形状以 schema 为准 |
| `error.data.origin_plugin`、`execution_owner` | 可选的插件/执行归属身份，由 `id`、`version` 组成 |

可选字段不应被推断为必定存在。无法识别有效请求 ID 的错误可使用 `id: null`。

## 错误码来源

标准 JSON-RPC 错误包括解析失败（`-32700`）、无效请求（`-32600`）、method 不存在（`-32601`）、参数无效（`-32602`）和内部错误（`-32603`）。daemon 的执行和权限等错误使用实现定义的服务端错误码，并通过 `daemon_error_code` / `daemon_error_kind` 保留应用语义。

完整 error schema 随 [](actions.md) 中的发现文档导出，wire 实现在 Common 的 `Contracts/Protocol/BuiltInProtocolContracts.cs`，dispatch 映射在 daemon 的 `Remote/Rpc/Dispatch/V2RpcDispatcher.cs`。不要把旧 action 错误枚举的数值映射到 V2。
