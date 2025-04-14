# 实例操作

<tip>
以下请求和响应中省略了 `id` 和 `message` 字段
</tip>

## AddInstance 添加实例 {#add-instance}

通过一系列参数创建mc服务器实例

### 请求

```json
{
  "action": "add_instance",
  "params": {
    "setting": {
      "source": "https://maven.minecraftforge.net/net/minecraftforge/forge/1.12.1-14.22.1.2478/forge-1.12.1-14.22.1.2478-installer.jar",
      "source_type": "core",
      "mc_version": "1.12.1",
      "name": "forge-1.12.1-14.22.1.2478",
      "java_path": "java",
      "target": "forge_server.jar",
      "instance_type": "forge",
      "target_type": "jar"
    }
  }
}
```

| 字段      | 数据类型                                                         | 说明     |
|---------|--------------------------------------------------------------|--------|
| setting | [InstanceFactorySetting](models.md#instance-factory-setting) | 实例工厂设置 |

### 响应

```json
{
  "status": "ok",
  "data": {
    "config": {
      "mc_version": "1.12.1",
      "name": "forge-1.12.1-14.22.1.2478",
      "java_path": "java",
      "target": "server.jar",
      "instance_type": "forge",
      "target_type": "jar",
      "uuid": "fdbf680c-fe52-4f1d-89ba-a0d9d8b857b2",
      "input_encoding": "utf-8",
      "output_encoding": "utf-8",
      "java_args": []
    }
  },
  "message": ""
}
```

| 字段名    | 数据类型                                        | 说明       |
|--------|---------------------------------------------|----------|
| config | [InstanceConfig](models.md#instance-config) | 创建的实例的配置 |

## RemoveInstance 移除实例 {#remove-instance}

使用实例的UUID移除实例

### 请求

```json
{
  "action": "remove_instance",
  "params": {
    "id": "fdbf680c-fe52-4f1d-89ba-a0d9d8b857b2"
  }
}
```

| 字段 | 数据类型             | 说明      |
|----|------------------|---------|
| id | string (uuid v4) | 实例的UUID |

### 响应

```json
{
  "status": "ok",
  "data": {},
  "message": ""
}
```

| 字段名 | 数据类型 | 说明 |
|-----|------|----|

## StartInstance 启动实例 {#start-instance}

使用实例的UUID启动实例

### 请求

```json
{
  "action": "start_instance",
  "params": {
    "id": "fdbf680c-fe52-4f1d-89ba-a0d9d8b857b2"
  }
}
```

| 字段 | 数据类型             | 说明      |
|----|------------------|---------|
| id | string (uuid v4) | 实例的UUID |

### 响应

```json
{
  "status": "ok",
  "data": {},
  "message": ""
}
```

| 字段名 | 数据类型 | 说明 |
|-----|------|----|

## StopInstance 停止实例 {#stop-instance}

使用实例的UUID停止实例, 相当于利用[SendToInstance](#send-to-instance)发送"stop".

### 请求

```json
{
  "action": "stop_instance",
  "params": {
    "id": "fdbf680c-fe52-4f1d-89ba-a0d9d8b857b2"
  }
}
```

| 字段 | 数据类型             | 说明      |
|----|------------------|---------|
| id | string (uuid v4) | 实例的UUID |

### 响应

```json
{
  "status": "ok",
  "data": {},
  "message": ""
}
```

| 字段名 | 数据类型 | 说明 |
|-----|------|----|

## SendToInstance 发送信息到实例控制台 {#send-to-instance}

将message发送到指定实例控制台

### 请求

```json
{
  "action": "send_to_instance",
  "params": {
    "id": "fdbf680c-fe52-4f1d-89ba-a0d9d8b857b2",
    "message": "say Hello World!"
  }
}
```

| 字段      | 数据类型             | 说明         |
|---------|------------------|------------|
| id      | string (uuid v4) | 实例的UUID    |
| message | string           | 要发送到控制台的消息 |

### 响应

```json
{
  "status": "ok",
  "data": {},
  "message": ""
}
```

| 字段名 | 数据类型 | 说明 |
|-----|------|----|

## KillInstance 发送信息到实例控制台 {#kill-instance}

强行关闭实例进程, 非必要不要使用该action, 因为强行关闭实例进程可能会导致数据丢失, 除非实例进程无响应.

### 请求

```json
{
  "action": "kill_instance",
  "params": {
    "id": "fdbf680c-fe52-4f1d-89ba-a0d9d8b857b2"
  }
}
```

| 字段 | 数据类型             | 说明      |
|----|------------------|---------|
| id | string (uuid v4) | 实例的UUID |

### 响应

```json
{
  "status": "ok",
  "data": {},
  "message": ""
}
```

| 字段名 | 数据类型 | 说明 |
|-----|------|----|

## GetInstanceStatus 获取实例状态 {#get-instance-status}

获取实例状态

### 请求

```json
{
  "action": "get_instance_status",
  "params": {
    "id": "fdbf680c-fe52-4f1d-89ba-a0d9d8b857b2"
  }
}
```

| 字段 | 数据类型             | 说明      |
|----|------------------|---------|
| id | string (uuid v4) | 实例的UUID |

### 响应

```json
{
  "status": "ok",
  "data": {
    "status": {
      "status": "running",
      "config": {
        "mc_version": "1.12.1",
        "name": "forge-1.12.1-14.22.1.2478",
        "...": "..."
      },
      "properties": [
        "accepts-transfers=false",
        "allow-flight=false",
        "..."
      ],
      "players": [
        {
          "name": "Ares_Connor",
          "uuid": "759c4726-6d04-4cd4-a4e5-c719416e04ad"
        }
      ]
    }
  },
  "message": ""
}
```

| 字段名    | 数据类型                                        | 说明   |
|--------|---------------------------------------------|------|
| status | [InstanceStatus](models.md#instance-status) | 实例状态 |

## GetAllStatus 获取实例状态 {#get-all-status}

获取所有实例状态

### 请求

```json
{
  "action": "get_all_status",
  "params": {}
}
```

| 字段 | 数据类型 | 说明 |
|----|------|----|

### 响应

```json
{
  "status": "ok",
  "data": {
    "status": {
      "fdbf680c-fe52-4f1d-89ba-a0d9d8b857b2": {
        "status": "running",
        "config": {
          "...": "..."
        },
        "properties": [
          "..."
        ],
        "players": []
      },
      "...": "..."
    }
  },
  "message": ""
}
```

| 字段名    | 数据类型                                                               | 说明   |
|--------|--------------------------------------------------------------------|------|
| status | dict[string (uuid v4),[InstanceStatus](models.md#instance-status)] | 实例状态 |