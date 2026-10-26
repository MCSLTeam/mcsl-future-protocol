# JSON-RPC 请求与响应

V2 使用 JSON-RPC 2.0，不使用旧 action/retcode envelope。每条请求是一个 JSON object，method 名称直接来自 frozen catalog，不从 C# enum 名称推导。

## 请求

```json
{
  "jsonrpc": "2.0",
  "id": "request-1",
  "method": "mcsl.system.info.get",
  "params": {}
}
```

- `jsonrpc` 必须为 `"2.0"`。
- `id` 使用 string 或有符号 Int64 整数；不要发送 null、小数或指数形式的 ID。浏览器客户端宜使用字符串，避免 JavaScript number 精度损失。
- `method` 必须在当前 catalog 中注册。
- `params` 使用该 method schema 定义的 JSON object；无参数请求使用 `{}`，不使用位置参数数组。

内置 RPC descriptor 不允许客户端 notification 调用，应始终携带 `id`。不要把 JSON-RPC 规范中可选的 batch/notification 能力推定为该 daemon profile 已支持；准入插件的调用能力也以 catalog metadata 为准。

## 成功响应

响应带相同 `id` 和 `result`，不再包含 `status`、`retcode` 或旧顶层 `data`。以 ping 为例：

```json
{
  "jsonrpc": "2.0",
  "id": "ping-1",
  "result": {
    "time": 1717171717000
  }
}
```

`result` 的结构由对应 method schema 决定。客户端必须区分成功响应、错误响应、无 `id` 的 event notification 和 upload acknowledgement，不能只按接收顺序匹配请求。

## 错误响应

失败使用 JSON-RPC `error`，包含数字 `code`、`message` 和结构化 `data`；解析或无效请求错误可能无法关联有效 `id`。字段和错误分类见 [](action-errcode.md)。

完整方法发现见 [](actions.md)，服务端事件见 [](event.md)。
