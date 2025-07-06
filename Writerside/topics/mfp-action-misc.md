# 其他操作

<tip>
以下请求和响应中省略了 `id` 和 `message` 字段
</tip>

## Ping {#ping}

测试服务器连通性

### 请求

```json
{
  "action": "ping"
}
```

### 响应

```json
{
  "status": "ok",
  "data": {
    "time": 1661529600000
  }
}
```

| 字段   | 数据类型 | 说明       |
|------|------|----------|
| time | long | 守护进程系统时间 |

## GetPermissions 获取权限 {#get-permissions}

获取子令牌所拥有的权限。

### 请求

```json
{
  "action": "get_permissions"
}
```

### 响应

```json
{
  "status": "ok",
  "data": {
    "permissions": [
      "my.permission"
    ]
  },
  "message": ""
}
```

| 字段          | 数据类型            | 说明        |
|-------------|-----------------|-----------|
| permissions | list&lt;string> | 子令牌所拥有的权限 |