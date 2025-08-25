# 实例操作

## CreateInstance 添加实例 {#create-instance}

- 操作说明：创建服务器实例；
- 所需权限：`mcsl.daemon.instance.create`。

### 请求

- 参数类型：[`InstanceInstallationInfo`](models.md#instance-installation-info)；
- 参数说明：实例工厂设置。

### 响应

- 响应类型：[`uuid`](models.md#uuid) | 无响应内容；
- 响应说明：若安装方式为 `pack` 或 `zip`时，返回文件上传 ID，用于使用 [FileUpload](action-system.md#file-download) 上传，30秒后失效并取消安装。

## RemoveInstance 移除实例 {#remove-instance}

- 操作说明：移除服务器实例；
- 所需权限：`mcsl.daemon.instance.remove`。

### 请求

- 参数类型：[`uuid`](models.md#uuid)；
- 参数说明：实例 ID。

### 响应

无响应内容。

## StartInstance 启动实例 {#start-instance}

- 操作说明：启动服务器实例；
- 所需权限：`mcsl.daemon.instance.<实例 ID>.operation.start`。

### 请求

- 参数类型：[`uuid`](models.md#uuid)；
- 参数说明：实例 ID。

### 响应

无响应内容。

## StopInstance 停止实例 {#stop-instance}

- 操作说明：停止服务器实例，相当于使用 [ExecuteCmd](#execute-cmd) 发送 `stop`；
- 所需权限：`mcsl.daemon.instance.<实例 ID>.operation.stop` 或 `mcsl.daemon.instance.<实例 ID>.command`。

### 请求

- 参数类型：[`uuid`](models.md#uuid)；
- 参数说明：实例 ID。

### 响应

无响应内容。

## KillInstance 发送信息到实例控制台 {#kill-instance}

- 操作说明：强制关闭服务器实例，杀死实例进程；
- 所需权限：`mcsl.daemon.instance.<实例 ID>.operation.kill`。

### 请求

- 参数类型：[`uuid`](models.md#uuid)；
- 参数说明：实例 ID。

### 响应

无响应内容。

## ExecuteCmd 执行命令 {#execute-cmd}

- 操作说明：将消息发送到指定实例的 `stdin`；
- 所需权限：`mcsl.daemon.instance.<实例 ID>.console.write`。

### 请求

- 参数类型：`map<str, any>`
- 参数字段：

| 字段  | 数据类型                   | 说明               |
|-----|------------------------|------------------|
| id  | [uuid](models.md#uuid) | 实例 ID            |
| msg | str                    | 要发送到 `stdin` 的消息 |

### 响应

无响应内容。

## GetInstanceReport 获取实例报告 {#get-instance-report}

获取实例报告

### 请求

- 参数类型：[`uuid`](models.md#uuid)；
- 参数说明：实例 ID。

### 响应

- 响应类型：[`InstanceReport`](models.md#instance-report)；
- 响应说明：实例报告。

## GetAllReports获取实例状态 {#get-all-reports}

获取所有实例状态

### 请求

无参数。

### 响应

- 响应类型：<code>map<[uuid](models.md#uuid), [InstanceReport](models.md#instance-report)></code>；
- 响应说明：所有实例的报告，键为实例 ID。
