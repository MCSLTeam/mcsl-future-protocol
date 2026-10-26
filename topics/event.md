# Typed event 订阅

V2 event 是 daemon 发送的 JSON-RPC notification，使用 event 名称作为 `method`，没有请求 `id`。它与 RPC 响应、上传确认和 binary frame 共用当前连接，但不是旧 event packet 或 packet 数组。

## 订阅和取消

通过 `mcsl.event.subscribe` / `mcsl.event.unsubscribe` 管理当前连接的 subscription。日志订阅示例：

```json
{
  "jsonrpc": "2.0",
  "id": "subscribe-1",
  "method": "mcsl.event.subscribe",
  "params": {
    "event": "mcsl.event.instance.log",
    "meta": {
      "instance_id": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa"
    }
  }
}
```

取消订阅时使用 `mcsl.event.unsubscribe`，并按相同 event 的 metadata 语义指定过滤条件。RPC 自身的权限和目标 event 的权限都由 daemon 校验。

`meta` 的 missing、显式 null 和 object 是不同 wire 状态，不可互换。是否允许省略或 null，以及 object 的字段，均以 event catalog 为准；不能默认给所有订阅补上 `meta: null`。

## Notification 形状

```json
{
  "jsonrpc": "2.0",
  "method": "mcsl.event.instance.log",
  "params": {
    "sequence": 1,
    "timestamp": 1717171717000,
    "meta": {
      "instance_id": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa"
    },
    "data": {
      "log": "[Server thread/INFO]: Done"
    }
  }
}
```

`sequence` 和 `timestamp` 是非负 Int64，timestamp 使用 Unix milliseconds。`data` 与 `meta` 的 presence 和 schema 由 descriptor 决定，不从消息样例推导。

## 顺序、背压与重连

事件按连接写队列顺序投递。慢消费者超过 bounded queue 容量时连接会断开；不能依赖静默丢包或事件持久重放。

重连后重新订阅，重新获取 instance catalog，并按 snapshot/delta 的版本语义进行同步。事件 sequence 不替代 catalog 自身的状态版本；发现状态缺口时应重新同步，而不是假定本地缓存仍然完整。

内置事件见 [](events.md)。`mcsl.file.upload.ack` 是连接所属的控制确认，不进入公共 event 订阅路由，见 [](file-transfer.md)。
