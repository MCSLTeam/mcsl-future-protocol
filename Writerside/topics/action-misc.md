# 其他操作

## Ping {#ping}

- 操作说明：测试服务器连通性；
- 所需权限：无。

### 请求

无参数。

### 响应

- 响应类型：`timestamp32`；
- 响应说明：守护进程系统时间。

## GetPermissions 获取权限 {#get-permissions}

- 操作说明：获取令牌所拥有的权限；
- 所需权限：无。

### 请求

无参数。

### 响应

- 响应类型：`array<str>`；
- 响应说明：令牌拥有的权限列表。

## SubscribeEvent 订阅事件 {#subscribe-event}

- 操作说明：订阅事件，详见 [](event.md)；
- 所需权限：因事件而异。

### 请求

- 参数类型：`map<str, any>`
- 参数字段：

| 字段       | 数据类型 | 说明                |
|----------|------|-------------------|
| name     | str  | 要订阅的事件名称          |
| filterer | any  | 事件过滤器，因事件而异，可能不存在 |

### 响应

- 响应类型：[`uuid`](models.md#uuid)；
- 响应说明：监听器 ID。

## UnsubscribeEvent 订阅事件 {#unsubscribe-event}

- 操作说明：取消订阅事件，详见 [](event.md)；
- 所需权限：无。

### 请求

- 参数类型：[`uuid`](models.md#uuid)；
- 参数字段：监听器 ID。

### 响应

无响应内容。
