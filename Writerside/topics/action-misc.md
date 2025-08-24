# 其他操作

## Ping {#ping}

测试服务器连通性。

### 请求

无参数。

### 响应

- 响应类型：`timestamp32`；
- 响应说明：守护进程系统时间。

## GetPermissions 获取权限 {#get-permissions}

获取令牌所拥有的权限。

### 请求

无参数。

### 响应

- 响应类型：`array<str>`；
- 响应说明：令牌拥有的权限列表。