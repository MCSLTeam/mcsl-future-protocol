# 其他操作

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