# 事件

**事件（Event）** 是服务器主动发送的数据，用于向客户端。

## 信息格式 {#message-format}

服务器发送的事件会带有**事件名称**、**事件数据**和**消息**的信息。

### 事件示例

```json
{
  "event": "事件名称",
  "data": {
    "事件数据1": "ABC",
    "事件数据2": 123
  },
  "time": 1725000000000
}
```

|  字段   |  数据类型  |              说明               |
|:-----:|:------:|:-----------------------------:|
| event | string | 事件名称，使用下划线命名法，如`instance_log` |
| data  | object |  事件数据，一个JSON对象，内含数据，没有则为`{}`  |
| time  | number |           事件发生的时间戳            |

<seealso>
   <category ref="related">
       <a href="event-system.md"/>
       <a href="event-instance.md"/>
       <a href="event-misc.md"/>
    </category>
</seealso>