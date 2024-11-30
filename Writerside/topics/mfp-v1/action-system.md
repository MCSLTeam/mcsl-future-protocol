# 系统操作

## FileUploadRequest 文件上传-请求 {#file-upload-request}

文件上传至 Daemon 前的请求，需要提供上传的目标路径，文件校验码和文件大小，由于使用分块传输，故还需提供分块大小。

### 请求

```json
{
  "action": "file_upload_request",
  "params": {
    "path": "path/to/file",
    "sha1": "114514114514114514114514114514",
    "timeout": 120000,
    "size": 1919810
  }
}
```

| 字段      | 数据类型                | 介绍                                                                                        |
|---------|---------------------|-------------------------------------------------------------------------------------------|
| path    | optional&lt;string> | 上传的文件将要存放的位置，不存在则上传到默认文件夹                                                                 |
| sha1    | optional&lt;string> | 文件SHA-1校验码，不存在则不检查SHA-1                                                                   |
| timeout | optional&lt;long>   | 文件上传超时（单位：ms），默认`120000`（即2分钟）<br/>如果两次[FileUploadChunk](#file-upload-chunk)的间隔超过此时长则取消上传 |
| size    | long                | 文件总大小                                                                                     |

#### 请求校验

1. `size` 在正整数范围
2. Daemon 无同名文件正在上传

*注：文件大小不得超过2GB (2^31-1字节)*

### 响应

```json
{
  "status": "ok",
  "data": {
    "file_id": "abcdefg-hijk-lmno-pqrstyvw"
  },
  "message": ""
}
```

| 字段      | 数据类型   | 介绍         |
|---------|--------|------------|
| file_id | string | 文件ID（文件上传） |

## FileUploadChunk 文件上传-区块上传 {#file-upload-chunk}

获取到 `file_id` 后，客户端上传文件的分块数据。

### 请求

```json
{
  "action": "file_upload_chunk",
  "params": {
    "file_id": "abcdefg-hijk-lmno-pqrstyvw",
    "offset": 114514,
    "data": "..."
  }
}
```

| 字段      | 数据类型   | 介绍               |
|---------|--------|------------------|
| file_id | string | 文件ID（文件上传）       |
| offset  | long   | 分块偏移量            |
| data    | string | 字符串形式的 `bytes[]` |

#### 请求校验

1. `file_id` 存在
2. `offset` 大于0且小于文件长度
3. 若上传完毕，校验SHA-1

### 响应

```json
{
  "status": "ok",
  "data": {
    "done": false,
    "received": 1048576
  },
  "message": ""
}
```

| 字段       | 数据类型    | 介绍      |
|----------|---------|---------|
| done     | boolean | 是否上传完毕  |
| received | long    | 已接受的字节数 |

## FileUploadCancel 文件上传-取消 {#file-upload-cancel}

通过 `file_id` 取消上传任务

### 请求

```json
{
  "action": "file_upload_cancel",
  "params": {
    "file_id": "..."
  }
}
```

#### 请求校验

1. `file_id` 存在

| 字段名     | 数据类型   | 介绍         |
|---------|--------|------------|
| file_id | string | 文件ID（文件上传） |

### 响应

```json
{
  "status": "ok",
  "data": {},
  "message": ""
}
```

| 字段名 | 数据类型 | 介绍 |
|-----|------|----|

## GetFileInfo 获取文件消息 {#get-file-info}

获取指定路径文件的信息

### 请求

```json
{
  "action": "get_file_info",
  "params": {
    "path": "path/to/file"
  }
}
```

#### 请求校验

1. 路径存在且是文件

| 字段名  | 数据类型   | 介绍         |
|------|--------|------------|
| path | string | 文件路径（相对路径） |

### 响应

```json
{
  "status": "ok",
  "data": {
    "meta": {
      "read_only": false,
      "size": 114514,
      "creation_time": 17777777777,
      "last_write_time": 17777777777,
      "last_access_time": 17777777777
    }
  },
  "message": ""
}
```

| 字段名  | 数据类型                                    | 介绍    |
|------|-----------------------------------------|-------|
| meta | [FileMetadata](models.md#file-metadata) | 文件元信息 |

## GetDirectoryInfo 获取目录信息 {#get-directory-info}

获取指定路径目录的信息

### 请求

```json
{
  "action": "get_directory_info",
  "params": {
    "path": "path/to/dir"
  }
}
```

#### 请求校验

1. 路径存在且是目录

| 字段名  | 数据类型   | 介绍         |
|------|--------|------------|
| path | string | 目录路径（相对路径） |

### 响应

```json
{
  "status": "ok",
  "data": {
    "parent": "relative/to/daemon/root",
    "files": [
      {
        "name": "file1",
        "type": "file",
        "meta": {
          "read_only": false,
          "size": 114514,
          "link_target": "some/path",
          "hidden": false,
          "creation_time": 17777777777,
          "last_write_time": 17777777777,
          "last_access_time": 17777777777
        }
      }
    ],
    "directories": [
      {
        "name": ".dir1",
        "type": "directory",
        "meta": {
          "hidden": true,
          "link_target": "some/path",
          "creation_time": 17777777777,
          "last_write_time": 17777777777,
          "last_access_time": 17777777777
        }
      }
    ]
  },
  "message": ""
}
```

| 字段名    | 数据类型                                              | 介绍                    |
|--------|---------------------------------------------------|-----------------------|
| parent | string                                            | 相对于 Daemon Root 的相对路径 |
| meta   | [DirectoryMetadata](models.md#directory-metadata) | 目录元数据                 |
| files  | list<[FileData](models.md#file-data)>             | 当前目录下子文件的信息列表         |

## FileDownloadRequest 文件下载-请求 {#file-download-request}

客户端请求下载文件，Daemon打开文件流并发回客户端

### 请求

```json
{
  "action": "file_download_request",
  "params": {
    "path": "path/to/file"
  }
}
```

| 字段名  | 数据类型   | 介绍         |
|------|--------|------------|
| path | string | 文件路径（相对路径） |

#### 请求校验

1. 路径存在且为文件

### 响应

```json
{
  "status": "ok",
  "data": {
    "file_id": "...",
    "size": 1919810,
    "sha1": "..."
  },
  "message": ""
}
```

| 字段名     | 数据类型   | 介绍         |
|---------|--------|------------|
| file_id | string | 文件ID（文件下载） |
| sha1    | string | 文件SHA-1校验码 |
| size    | long   | 文件大小       |

## FileDownloadChunk 文件下载-区块 {#file-download-chunk}

客户端请求下载文件的一部分

### 请求

```json
{
  "action": "file_download_chunk",
  "params": {
    "file_id": "...",
    "range": "0..1024"
  }
}
```

| 字段名     | 数据类型   | 介绍                 |
|---------|--------|--------------------|
| file_id | string | 文件ID（文件下载）         |
| range   | string | 要下载的范围（单位：字节），左闭右开 |

#### 请求校验

1. `file_id` 存在
2. 范围起始≥0 且 范围结束≤文件大小

### 响应

```json
{
  "status": "ok",
  "data": {
    "content": "..."
  },
  "message": ""
}
```

| 字段名  | 数据类型   | 介绍               |
|------|--------|------------------|
| data | string | 字符串形式的 `bytes[]` |

## FileDownloadClose 文件下载-关闭 {#file-download-close}

客户端终止或者完成下载文件时，请求服务端关闭和释放相关文件资源

### 请求

```json
{
  "action": "file_download_close",
  "params": {
    "file_id": "..."
  }
}
```

| 字段名     | 数据类型   | 介绍         |
|---------|--------|------------|
| file_id | string | 文件ID（文件下载） |

#### 请求校验

1. `file_id` 存在

### 响应

```json
{
  "status": "ok",
  "data": {},
  "message": ""
}
```

| 字段名 | 数据类型 | 介绍 |
|-----|------|----|