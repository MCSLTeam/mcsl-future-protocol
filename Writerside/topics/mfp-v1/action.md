# 操作

**操作（Action）** 是客户端主动发起的请求，服务器在收到请求后将会将结果返回给客户端。

## 信息格式 {#message-format}

客户端要执行操作时，会请求带有**操作名称**和**参数**的信息。
服务端接收到请求时，会响应带有**操作结果**、**返回数据**和**消息**的信息。

### 请求示例 {#request-example}

```json
{
  "action": "操作名称",
  "params": {
    "参数1": "ABC",
    "参数2": 123
  },
  "echo": "echo"
}
```

|   字段   |        数据类型         |                          介绍                          |
|:------:|:-------------------:|:----------------------------------------------------:|
| action |       string        |         操作名称，使用下划线命名法，如`file_upload_request`         |
| params |       object        |             操作参数，一个JSON对象，内含参数，没有则为`{}`              |
|  echo  | optional&lt;string> | 回声，如果指定了 `echo` 字段, 那么响应也会同时包含一个 `echo` 字段, 它们会有相同的值 |

### 响应示例 {#response-example}

```json
{
  "status": "ok",
  "data": {
    "返回数据1": "ABC",
    "返回数据2": 123
  },
  "message": "",
  "echo": "echo"
}
```

|   字段    |        数据类型         |                                                                                                               介绍                                                                                                                |
|:-------:|:-------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| status  |       string        | 操作结果，仅可能为以下值：<br/>`ok` - 操作成功<br/>`error` - 操作失败，此时 `data` 为空，`message` 为错误消息<br/>`async` - 异步操作，状态未知，详见[异步操作](#async-action)，此时 `data` 和 `message` 都为空<br/>`unauthorized` - 未进行鉴权，详见 [鉴权](method.md)，此时 `data` 和 `message` 都为空 |
|  data   |       object        |                                                                                                   返回数据，一个JSON对象，内含数据，没有则为`{}`                                                                                                   |
| message |       string        |                                                                                               消息，常用于在操作结果为`error`时返回错误原因，没有则为`""`                                                                                               |
|  echo   | optional&lt;string> |                                                                                                     回声，与请求中的 `echo` 字段的值相同                                                                                                      |

## 异步操作 {#async-action}

异步操作是在执行操作的同时返回数据的操作，常用于**无需知道操作返回数据等的情况**，例如实例执行命令等。

在操作名称之后添加`_async`后将进行异步操作，例如`file_upload_request_async`。
此时服务端将直接返回响应并异步执行操作。

由于异步执行操作，服务器将无法获取操作状态等，所以**在异步操作的响应中，`status` 字段为 `async`，`data`
字段为空（`{}`），`message` 字段为 `""`**。

<seealso>
   <category ref="related">
       <a href="action-system.md"/>
       <a href="action-token.md"/>
       <a href="action-instance.md"/>
       <a href="action-config.md"/>
       <a href="action-misc.md"/>
    </category>
</seealso>