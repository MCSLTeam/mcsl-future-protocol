# 事件列表

## 实例日志更新 {#instance-log}

<primary-label ref="instance-event"/>
<secondary-label ref="1.0"/>

- **事件名称**：`instance_log`
- **事件说明**：实例进程向 `stdout` 输出信息时触发；
- **所需权限**：`mcsl.daemon.instance.<实例 ID>.console.read`

### 过滤器 {collapsible="true"}

- **过滤器类型**：[`uuid`](models.md#uuid)
- **过滤器说明**：实例 ID

### 数据 {collapsible="true"}

- **数据类型**：`str`；
- **数据说明**：输出的信息

## 系统信息 {#system_info}

<primary-label ref="system-event"/>
<secondary-label ref="1.0"/>

- **事件名称**：`system_info`
- **事件说明**：系统信息，每 5 秒发送一次；
- **所需权限**：`mcsl.daemon.system_info`

### 过滤器 {collapsible="true"}

- **过滤器类型**：无过滤器

### 数据 {collapsible="true"}

- **数据类型**：`map<str, any>`
- **数据字段**：
    - `os` - `map<str, any>`：系统信息
        - `name` - `str`：系统名称，例如 `Windows`、`macOS`、`Ubuntu` 等
        - `version` - `str`：系统版本
        - `arch` - `str`：系统架构，例如 `x86_64`、`arm64` 等
    - `cpu` - `map<str, any>`：CPU 信息，包含型号、核心数、线程数等
        - `name` - `str`：CPU 型号，例如 `Intel(R) Core(TM) i7-10750H`、`Apple M1` 等
        - `vendor` - `str`：CPU 供应商，例如 `intel`、`amd` 等
        - `count` - `u32`：CPU 线程数
        - `usage` - `f64`：CPU 总占用率
    - `mem` - `map<str, any>`：内存信息，是否包含SWAP由系统API而异
        - `free` - `u64`：可用内存，单位为KB
        - `total` - `u64`：总内存，单位为KB
    - `drive` - `map<str, any>`：守护进程所在磁盘信息
        - `drive_format` - `str`：磁盘格式，例如 `ntfs` 等
        - `free` - `u64`：磁盘可用空间，单位为字节
        - `total` - `u64`：磁盘容量，单位为字节
    - `daemon` - `map<str, any>`：守护进程信息
        - `cpu_usage` - `f64`：守护进程 CPU 占用率
        - `mem_usage` - `f64`：守护进程占用内存，单位为KB
