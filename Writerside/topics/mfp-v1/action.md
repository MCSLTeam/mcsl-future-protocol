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

|   字段   |        数据类型         |                  说明                  |
|:------:|:-------------------:|:------------------------------------:|
| action |       string        | 操作名称，使用下划线命名法，如`file_upload_request` |
| params | optional&lt;object> |    操作参数，一个JSON对象，内含参数，可选，默认为`{}`     |
|  echo  |       string        |     回声，响应也包含一个 `echo` 字段，它们的值相同      |

### 响应示例 {#response-example}

<tabs>
    <tab title="成功">
        操作成功，返回数据和消息<br/>
        <code-block lang="json">
            {
              "status": "ok",
              "retcode": 0,
              "data": {
                "返回数据1": "ABC",
                "返回数据2": 123
              },
              "message": "消息",
              "echo": "echo"
            }
        </code-block>
        <table>
            <tr>
                <td>字段</td>
                <td>数据类型</td>
                <td>说明</td>
            </tr>
            <tr>
                <td>status</td>
                <td>string</td>
                <td>操作结果，操作成功时始终是 <code>ok</code></td>
            </tr>
            <tr>
                <td>retcode</td>
                <td>number</td>
                <td>响应码，操作成功时始终是 <code>0</code></td>
            </tr>
            <tr>
                <td>data</td>
                <td>object</td>
                <td>返回数据，内含数据，没有数据则为 <code>{}</code></td>
            </tr>
            <tr>
                <td>message</td>
                <td>string</td>
                <td>消息，操作成功时始终是 <code>""</code></td>
            </tr>
            <tr>
                <td>echo</td>
                <td>string</td>
                <td>回声，与请求中的 <code>echo</code> 字段的值相同</td>
            </tr>
        </table>
    </tab>
    <tab title="错误">
        操作失败，返回错误的 <code>retcode</code> 和消息<br/>
        <code-block lang="json">
            {
              "status": "error",
              "data": {
                "retcode": 1400
              },
              "message": "File not found!",
              "echo": "echo"
            }
        </code-block>
        <table>
            <tr>
                <td>字段</td>
                <td>数据类型</td>
                <td>说明</td>
            </tr>
            <tr>
                <td>status</td>
                <td>string</td>
                <td>操作结果，操作失败时始终是 <code>error</code></td>
            </tr>
            <tr>
                <td>retcode</td>
                <td>number</td>
                <td>响应码，用于区分错误类型，详见 <a href="action-retcode.md"/></td>
            </tr>
            <tr>
                <td>data</td>
                <td>object</td>
                <td>返回数据，操作失败时始终是 <code>{}</code></td>
            </tr>
            <tr>
                <td>message</td>
                <td>string</td>
                <td>消息，操作失败时将显示错误信息</td>
            </tr>
            <tr>
                <td>echo</td>
                <td>string</td>
                <td>回声，与请求中的 <code>echo</code> 字段的值相同</td>
            </tr>
        </table>
    </tab>
</tabs>

<seealso>
   <category ref="related">
       <a href="action-system.md"/>
       <a href="action-token.md"/>
       <a href="action-instance.md"/>
       <a href="action-config.md"/>
       <a href="action-misc.md"/>
    </category>
</seealso>