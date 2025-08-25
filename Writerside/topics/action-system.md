# 系统操作

## FileUploadRequest 文件上传-请求 {#file-upload-request}

- 操作说明：请求将文件上传至守护进程，文件存在则覆盖；
- 所需权限：`mcsl.daemon.instance.<实例 ID>.file.write`。

### 请求

- 参数类型：`map<str, any>`；
- 参数字段：

| 字段       | 数据类型                   | 说明                   |
|----------|------------------------|----------------------|
| instance | [uuid](models.md#uuid) | 上传文件存放的实例            |
| path     | str                    | 要存放文件的在实例下的相对路径      |
| sha1     | str                    | 文件SHA-1校验码，可选，缺省时不校验 |

### 响应

- 响应类型：[`uuid`](models.md#uuid)；
- 响应说明：上传 ID，用于使用 [FileUpload](#file-upload) 上传，30秒后失效。

## FileUpload 文件上传-上传 {#file-upload}

<warning>
此接口为 <strong>HTTP(S)</strong> 协议。
</warning>

将文件上传至守护进程，文件存在则覆盖。

### 请求

- 请求地址：`http(s)://<守护进程地址>/api/v1/upload/<上传 ID，UUID 字符串形式>`；
- 请求方法：**POST**；
- 请求类型：`multipart/form-data`；
- 请求主体说明：要上传的文件内容。

### 响应

<tabs>
    <tab title="上传成功">
        <ul>
            <li>状态码： <strong>200</strong> ；</li>
            <li>响应主体：空。</li>
        </ul>
    </tab>
    <tab title="上传 ID 不存在">
        <ul>
            <li>状态码： <strong>404</strong> ；</li>
            <li>响应主体：空。</li>
        </ul>
    </tab>
</tabs>

## FileUploadProgress 文件上传-获取进度 {#file-upload-progress}

- 操作说明：通过上传 ID 获取上传进度；
- 所需权限：`mcsl.daemon.instance.<实例 ID>.file.write`。

### 请求

- 参数类型：[`uuid`](models.md#uuid)；
- 参数说明：上传 ID。

### 响应

- 响应类型：`int64`；
- 响应说明：已上传的字节数。

## GetFileInfo 获取文件消息 {#get-file-info}

- 操作说明：获取指定路径文件的信息；
- 所需权限：`mcsl.daemon.instance.<实例 ID>.file.<read|write>`。

### 请求

- 参数类型：`map<str, any>`；
- 参数字段：

| 字段名      | 数据类型                   | 说明          |
|----------|------------------------|-------------|
| instance | [uuid](models.md#uuid) | 文件所在的实例     |
| path     | str                    | 文件在实例下的相对路径 |

### 响应

- 响应类型：[`FileData`](models.md#file-data)；
- 响应说明：文件或目录的信息。

## FileDownloadRequest 文件下载-请求 {#file-download-request}

- 操作说明：请求将守护进程中的文件下载至本地；
- 所需权限：`mcsl.daemon.instance.<实例 ID>.file.read`。

### 请求

| 字段       | 数据类型                   | 说明          |
|----------|------------------------|-------------|
| instance | [uuid](models.md#uuid) | 文件所在的实例     |
| path     | str                    | 文件在实例下的相对路径 |

### 响应

- 响应类型：`map<str, any>`；
- 响应字段：

| 字段名         | 数据类型                   | 说明                                                   |
|-------------|------------------------|------------------------------------------------------|
| download_id | [uuid](models.md#uuid) | 下载 ID，用于使用 [FileDownload](#file-download) 下载，30秒后失效。 |
| sha1        | str                    | 文件 SHA-1 校验码                                         |

## FileDownload 文件下载-下载 {#file-download}

<warning>
此接口为 <strong>HTTP(S)</strong> 协议。
</warning>

将文件上传至守护进程，文件存在则覆盖。

### 请求

- 请求地址：`http(s)://<守护进程地址>/api/v1/download/<下载 ID，UUID 字符串形式>`；
- 请求方法：**GET**；
- 请求主体说明：要下载的文件内容。

### 响应

<tabs>
    <tab title="下载成功">
        <ul>
            <li>状态码： <strong>200</strong> ；</li>
            <li>响应主体：文件内容。</li>
        </ul>
    </tab>
    <tab title="下载 ID 不存在">
        <ul>
            <li>状态码： <strong>404</strong> ；</li>
            <li>响应主体：空。</li>
        </ul>
    </tab>
</tabs>