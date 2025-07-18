# 响应码

## `0` OK - 成功

当 [](mfp-action.md) 请求有效、操作执行成功时，响应码为 `0`。

## `10000` Request Error - 请求错误

| 响应码     | 名称                  | 中文   | 说明          |
|---------|---------------------|------|-------------|
| `10001` | Bad Request         | 无效请求 | 无法解析请求或缺失内容 |
| `10002` | Unknown Action      | 未知操作 | 客户端调用未知操作   |
| `10003` | Permission Denied   | 权限不足 | 令牌权限不足      |
| `10004` | Action Unavailable  | 暂不可用 | 请求的操作当前不可用  |
| `10005` | Rate Limit Exceeded | 频率过快 | 请求频率超出了速率限制 |
| `10006` | Param Error         | 参数错误 | 参数缺失或类型错误等  |

## `20000` Server Error - 服务器错误

| 响应码     | 名称               | 中文   | 说明          |
|---------|------------------|------|-------------|
| `20001` | Unexpected Error | 意外错误 | 服务器内部发生未知错误 |

### `21000` File Error - 文件错误

| 响应码     | 名称                  | 中文     | 说明               |
|---------|---------------------|--------|------------------|
| `21001` | File Not Found      | 文件不存在  | 访问的文件不存在         |
| `21002` | File Already Exists | 文件已存在  | 访问的文件已存在         |
| `21003` | File In Use         | 文件已占用  | 文件正在被使用，无法再次打开   |
| `21004` | It's A Directory    | 那是目录   | 访问的路径是目录而不是文件    |
| `21005` | It's A File         | 那是文件   | 访问的路径是文件而不是目录    |
| `21006` | File Access Denied  | 无法访问文件 | 无法读/写/创建/删除/移动文件 |
| `21007` | Disk Full           | 磁盘已满   | 服务器储存空间已满        |

#### `21100` Upload / Download Error - 上传 / 下载错误

| 响应码     | 名称                  | 中文   | 说明       |
|---------|---------------------|------|----------|
| `21101` | Already Uploading   | 已在上传 | 文件已在上传   |
| `21101` | Already Downloading | 已在下载 | 文件已在下载   |
| `21102` | Not Uploading       | 不在上传 | 文件不在上传   |
| `21102` | Not Downloading     | 不在下载 | 文件不在下载   |
| `21103` | File Too Big        | 文件过大 | 要上传的文件过大 |

## `30000` Instance Error - 实例错误

| 响应码     | 名称                      | 中文     | 说明           |
|---------|-------------------------|--------|--------------|
| `30001` | Instance Not Found      | 实例不存在  | 访问的实例不存在     |
| `30002` | Instance Already Exists | 实例已存在  | 访问的实例已存在     |
| `30003` | Bad Instance State      | 实例状态错误 | 实例当前状态不支持该操作 |
| `30004` | Bad Instance Type       | 实例类型错误 | 实例类型不支持该操作   |

### `31000` Instance Action Error - 实例操作错误

| 响应码     | 名称                 | 中文   | 说明           |
|---------|--------------------|------|--------------|
| `31001` | Installation Error | 安装错误 | 实例安装错误       |
| `31002` | Process Error      | 执行错误 | 实例启动/关闭/重启错误 |


## `40000` ~ `59999` 保留错误段

这两段响应码为保留段，<tooltip term="daemon">Daemon</tooltip> 实现不应该使用。

## `60000` ~ `99999` 其它错误段

对于其他无法涵盖的错误，<tooltip term="daemon">Daemon</tooltip> 实现可以自由使用来表示。