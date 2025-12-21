# 操作列表

## 订阅事件 {#subscribe_event}

## 取消订阅事件 {#unsubscribe_event}

## 获取系统信息 {#get_system_info}

<primary-label ref="system-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`get_system_info`
- **所需权限**：`mcsl.daemon.system_info`

### 请求 {collapsible="true"}

- **参数类型**：无参数

### 响应 {collapsible="true"}

## 获取 Java 环境 {#get_java_list}

<primary-label ref="system-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`get_java_list`
- **所需权限**：`mcsl.daemon.java_list`

### 请求 {collapsible="true"}

- **参数类型**：无参数

### 响应 {collapsible="true"}

- **响应类型**：`array<map<str, any>>`
- **响应字段**：
  - `path` - `str`：Java 环境路径
  - `type` - `str`：Java 类型，`jre` 或 `jdk`
  - `version` - `str`：Java 版本
  - `arch` - `str`：Java 架构，例如 `x86_64` 等
  - `vendor` - `str`：Java 供应商，例如 `Oracle Corporation` 等

## 请求上传文件 {#file_upload_request}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`file_upload_request`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.write`

详见 [文件传输 - 本地文件上传](file-transfer.md#file_upload)

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `path` - [`FilePath`](models.md#file_path)：文件路径
  - `size` - `uint64`：文件大小
  - `sha1` - `str`：可选，文件 SHA-1 哈希值

### 响应 {collapsible="true"}

- **响应类型**：`str`
- **响应说明**：上传 ID
  - 

## 请求下载文件 {#file_download_request}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`file_download_request`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.read`

详见 [文件传输 - 文件下载](file-transfer.md#file_download)

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `path` - [`FilePath`](models.md#file_path)：文件路径

### 响应 {collapsible="true"}

- **响应类型**：`str`
- **响应说明**：下载 ID

## 创建目录 {#create_dir}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`create_dir`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.create_dir`

如果目录已存在，操作将失败。将创建目录的所有父目录。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `path` - [`FilePath`](models.md#file_path)：目录路径

### 响应 {collapsible="true"}

- **响应类型**：无数据

## 创建文件 {#create_file}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`create_file`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.create`

如果文件已存在，操作将失败。如果父目录不存在，操作将失败。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `path` - [`FilePath`](models.md#file_path)：文件路径

### 响应 {collapsible="true"}

- **响应类型**：无数据

## 创建符号链接 {#create_link}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`create_file_link`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.link`

如果链接文件已存在，操作将失败。如果父目录不存在，操作将失败。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `target` - [`FilePath`](models.md#file_path)：源文件路径
  - `link` - [`FilePath`](models.md#file_path)：链接文件路径

### 响应 {collapsible="true"}

- **响应类型**：无数据

## 移动文件或目录 {#move_file}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`move_file`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.move`

如果目标文件或目录已存在，操作将失败。如果父目录不存在，操作将失败。重命名也通过此操作实现。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `from` - [`FilePath`](models.md#file_path)：源文件路径
  - `dest` - [`FilePath`](models.md#file_path)：目标文件路径

### 响应 {collapsible="true"}

- **响应类型**：无数据

## 复制文件或目录 {#copy_file}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`copy_file`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.copy`

如果目标文件或目录已存在，操作将失败。如果父目录不存在，操作将失败。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `from` - [`FilePath`](models.md#file_path)：源文件路径
  - `dest` - [`FilePath`](models.md#file_path)：目标文件路径

### 响应 {collapsible="true"}

- **响应类型**：无数据

## 删除文件或目录 {#delete_file}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`delete_file`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.delete`

如果文件或目录不存在，操作将失败。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `path` - [`FilePath`](models.md#file_path)：文件路径

### 响应 {collapsible="true"}

- **响应类型**：无数据

## 更改文件或目录权限 {#chmod}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>
<secondary-label ref="unix"/>

- **操作名称**：`chmod`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.chmod`

如果文件或目录不存在，操作将失败。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `path` - [`FilePath`](models.md#file_path)：文件路径
  - `mode` - `str`：权限模式，例如 `755`

### 响应 {collapsible="true"}

- **响应类型**：无数据

## 获取文件信息 {#get_file_info}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`get_file_info`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.read`

如果文件不存在，操作将失败。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `path` - [`FilePath`](models.md#file_path)：文件路径

### 响应 {collapsible="true"}

- **响应类型**：[`FileData`](models.md#file_data)

## 获取目录信息 {#get_directory_info}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`get_directory_info`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.read`

如果目录不存在，操作将失败。

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `path` - [`FilePath`](models.md#file_path)：目录路径

### 响应 {collapsible="true"}

- **响应类型**：[`DirectoryData`](models.md#directory_data)

## 压缩文件或目录 {#compress_file}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`compress_file`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.compress`

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `type` - [`CompressionType`](models.md#compression_type)：压缩算法
  - `dest` - [`FilePath`](models.md#file_path)：压缩包存放路径

### 响应 {collapsible="true"}

- **响应类型**：无数据

## 解压文件 {#decompress_file}

<primary-label ref="file-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`decompress_file`
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.file.decompress`

### 请求 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `type` - [`CompressionType`](models.md#compression_type)：压缩算法
  - `dest` - [`FilePath`](models.md#file_path)：解压路径

### 响应 {collapsible="true"}

- **响应类型**：无数据

