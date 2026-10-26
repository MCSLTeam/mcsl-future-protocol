# 内置 typed events

内置事件由 `MCServerLauncher.Daemon.API/Protocol/BuiltInProtocolDefinitions.cs` 注册。完整 data/meta schema 及权限由 `rpc.discover` 的 `x-mcsl-events` 返回，见 [](actions.md)。以下仅说明各事件的用途，不建立第二份 schema 定义。

| Event method | Meta | 用途 |
| --- | --- | --- |
| `mcsl.event.instance.catalog.changed` | omitted | 推送有序 instance catalog delta |
| `mcsl.event.daemon.report` | omitted | 周期性 daemon/system report |
| `mcsl.event.instance.log` | required object：`instance_id` | 指定 instance 的日志 |
| `mcsl.event.notification` | required object：`source_instance_id`、`rule_id` | 指定来源 instance/rule 的通知 |

上述内置 event descriptor 的权限为 `*`（已认证连接），但建立订阅仍需通过 `mcsl.event.subscribe` RPC 的权限检查。

## Instance catalog

使用 `mcsl.instance.catalog.get` 获取 immutable snapshot，并通过 catalog changed event 更新本地镜像。应核对 snapshot/delta 自身的版本和连续性；重连或发现缺口时重新同步，不能只靠断线前缓存恢复。

## 插件事件

获准插件可向冻结 catalog 注册 typed event。客户端必须使用该运行时 catalog 的完整 event 名称、权限和 metadata，不根据插件名称猜测可订阅事件。

## 与其他 notification 的区别

上传 binary chunk 的确认使用 `mcsl.file.upload.ack`，不是可通过 event subscribe 获取的内置业务事件。不要把所有无 `id` 的消息都当作同一种 event envelope。
