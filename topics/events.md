# 事件列表

## 实例日志更新 {#instance-log}
<primary-label ref="instance-event"/>
<secondary-label ref="1.0"/>

- **事件名称**：`instance_log`
- **事件说明**：实例进程向 `stdout` 输出信息时触发；
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.console.read`

### 过滤器

- **过滤器类型**：[`uuid`](models.md#uuid)
- **过滤器说明**：实例 ID。

### 数据

- **数据类型**：`str`；
- **数据说明**：输出的信息。