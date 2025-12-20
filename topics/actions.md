# 操作列表

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
    - `path` - `str`：Java 环境路径。
    - `type` - `str`：Java 类型，`jre` 或 `jdk`。
    - `version` - `str`：Java 版本。
    - `arch` - `str`：Java 架构，例如 `x86_64` 等。
    - `vendor` - `str`：Java 供应商，例如 `Oracle Corporation` 等。

## 请求上传文件 {#file_upload_request}

## 获取上传进度 {#file_upload_progress}

## 请求下载文件 {#file_download_request}

## 订阅事件 {#subscribe_event}

## 取消订阅事件 {#unsubscribe_event}
