# 连接 daemon

## HTTP metadata

`GET /` 返回 daemon 名称（`message`）、版本、`status: "ok"` 和 `api_version: "v2"`。`GET /info` 使用 `name` 字段返回名称、版本和相同的 API 标识。

```json
{
  "name": "MCServerLauncher Future Daemon CSharp",
  "version": "1.0.0.0",
  "api_version": "v2"
}
```

示例中的产品版本仅用于展示响应形状，不是支持矩阵或 SDK 包版本。

## WebSocket

使用主令牌或由主令牌签发的 JWT 子令牌建立连接：

```text
ws(s)://<host>:<port>/api/v2?token=<token>
```

daemon 只接受路径完全等于 `/api/v2` 的 WebSocket upgrade。缺失或无效 token 会返回未授权响应；其他 WebSocket 路径不属于协议 endpoint。远程生产连接应使用 `wss`，代理及诊断日志必须脱敏查询参数中的 token。

## 运行时协议发现

建立已认证连接后，调用 `rpc.discover` 获取 frozen runtime catalog 的 OpenRPC 文档，见 [](actions.md)。HTTP `GET /apifox.json` 返回从 catalog 生成的原生 Apifox 项目，用于调试 WebSocket RPC，不是 HTTP action bridge。

当前不提供旧 Postman Collection、旧 HTTP 子令牌申请入口或 V1 fallback。短期令牌通过 `mcsl.auth.token.issue` 签发，配置和权限要求见 [](web-user-system.md)。

## 连接生命周期

JSON-RPC response、event notification、upload acknowledgement 和 binary frame 共用一个 connection-owned 有序写队列。

重连后必须重新建立 subscription 并重新同步 instance catalog；未完成的 file/console binary session 不跨连接恢复。慢消费者超过 bounded queue 容量时会被断开，不能依赖静默丢包或假定断线期间的事件会重放。
