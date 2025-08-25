# 事件

**事件（Event）** 是指由服务器主动向客户端推送的数据消息。

## 事件订阅 {#subscribe}

要接收事件，需先进行订阅。部分事件已默认订阅。

订阅事件请参见：[](action-misc.md#subscribe-event)。

订阅事件通常需要相应权限。如权限不足，则无法订阅该类事件，默认情况下也不会被订阅。

部分事件在订阅时需提供 **过滤器** 以实现消息筛选，例如 [InstanceLog](event-instance.md#instance-log) 事件需指定实例 ID 作为过滤条件。

订阅成功后将返回一个 **监听器 ID**，用于标识该订阅关系，并可用于后续取消订阅。

取消事件订阅请参见：[](action-misc.md#unsubscribe-event)。

## 事件字段

|     字段     |          数据类型          |               说明               |
|:----------:|:----------------------:|:------------------------------:|
|   event    |          str           | 事件名称，使用下划线命名法，如 `instance_log` |
|    data    |          any           |        事件数据，因事件而异，可能不存在        |
|    time    |      timestamp32       |            事件发生的时间             |
| subscriber | [uuid](models.md#uuid) |            事件监听器 ID            |
