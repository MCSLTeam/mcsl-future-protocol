# 文件传输

MCSL Future 通讯协议中，文件上下传采用 **WebSocket** 和 **HTTP** 协议同时使用的方法。

故本文将详细地介绍文件上下传的请求方法。

## 文件上传

以下图表展示了文件上传的流程：

```mermaid
sequenceDiagram
    客户端 ->> 守护进程（WS Server）: 请求上传文件
    守护进程（WS Server） -->> 客户端: 返回上传 ID
    守护进程（WS Server） -->> 守护进程（HTTP Server）: 创建上传任务
    客户端 ->> 守护进程（HTTP Server）: 通过包含上传 ID 的 URL 使用 POST 请求上传文件数据
    客户端 ->> 守护进程（WS Server）: 获取上传进度
    守护进程（WS Server） -->> 客户端: 返回上传进度
```

### <![CDATA[STEP 1 - 请求上传文件]]>

* 客户端向守护进程发送 [`file_upload_request` 操作](actions.md#file_upload_request)，包含文件信息和保存路径等。
* 守护进程返回上传 ID。
* 上传 ID 如果在 30s 内没有被使用，将被自动删除。
* 上传 ID 在 HTTP 连接打开后将被删除。

### <![CDATA[STEP 2 - 上传文件数据]]>

* 客户端向 `http(s)://<守护进程地址>/upload/<上传 ID>` 发起 **POST** 请求，并使用 `multipart/form-data` 格式上传文件数据。
* 守护进程收到请求后，将文件数据保存到文件中。

### <![CDATA[STEP 3 - 获取上传进度]]>

* 在文件上传过程中，客户端向守护进程发送 [`file_upload_progress` 操作](actions.md#file_upload_progress)，包含上传 ID。
* 守护进程返回上传进度

### 其他

部分操作（例如通过服务端压缩包新建实例）中，客户端无需请求上传文件
（因为无法提供保存路径），而是调用操作时守护进程会直接返回一个上传 ID。

此上传 ID 不指向到任何实例中的路径，而是指向到守护进程中的临时文件，可通过此上传 ID 获取上传进度。

上传完成后，守护进程将通过上传的文件完成操作。

## 文件下载

以下图表展示了文件下载的流程：

```mermaid
sequenceDiagram
    客户端 ->> 守护进程（WS Server）: 请求下载文件
    守护进程（WS Server） -->> 客户端: 返回下载 ID
    守护进程（WS Server） -->> 守护进程（HTTP Server）: 创建下载任务
    客户端 ->> 守护进程（HTTP Server）: 通过包含下载 ID 的 URL 使用 GET 请求下载文件数据
```

### <![CDATA[STEP 1 - 请求下载文件]]>

* 客户端向守护进程发送 [`file_download_request` 操作](actions.md#file_download_request)，包含文件路径等。
* 守护进程返回下载 ID。
* 下载 ID 如果在 30s 内没有被使用，将被自动删除。
* 下载 ID 在 HTTP 连接打开后将被删除。

### <![CDATA[STEP 2 - 下载文件数据]]>

* 客户端向 `http(s)://<守护进程地址>/download/<下载 ID>` 发起 **GET** 请求，即可下载文件数据。
* 守护进程收到请求后，将文件数据返回给客户端。
