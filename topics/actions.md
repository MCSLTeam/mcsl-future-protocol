# 操作列表

## 获取系统信息 {#get_system_info}

<primary-label ref="system-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`get_system_info`
- **所需权限**：`mcsl.daemon.system_info`

### 请求 {collapsible="true"}

- **参数类型**：无参数

### 响应 {collapsible="true"}

- **参数类型**：`map<str, any>`
- **参数字段**：
  - `os` - `map<str, any>`：系统信息，包含系统类型、版本、架构等。
    - `name` - `str`：系统名称，例如 `Windows`、`macOS`、`Ubuntu` 等。
    - `version` - `str`：系统版本。
    - `arch` - `str`：系统架构，例如 `x86_64`、`arm64` 等。
  - `cpu` - `map<str, any>`：CPU 信息，包含型号、核心数、线程数等。
    - `model` - `str`：CPU 型号，例如 `Intel(R) Core(TM) i7-10750H`、`Apple M1` 等。
    - `cores` - `u64`：CPU 核心数。
    - `daemon_usage` - `u64`：守护进程 CPU 占用率，0 - 100。
    - `other_usage` - `u64`：其他应用 CPU 占用率，0 - 100。
  - `memory` - `map<str, any>`：内存信息，包含总内存、可用内存等。
      - `daemon_used` - `u64`：守护进程已用内存，单位为字节。
      - `other_used` - `u64`：其他应用已用内存，单位为字节。
      - `total_mem` - `u64`：总内存，单位为字节。
      - `total_swap` - `u64`：总交换空间，单位为字节。
  - `disks` - `array<map<str, any>>`：磁盘信息，包含每个磁盘的型号、容量、挂载点等。
    - `capacity` - `u64`：磁盘容量，单位为字节。
    - `daemon_used` - `u64`：守护进程已用磁盘空间，单位为字节，如果不在磁盘上则为 0。
    - `other_used` - `u64`：其他应用已用磁盘空间，单位为字节。
    - `mount_point` - `str`：磁盘挂载点，在 Windows 上为 `C:\` 等。

## 获取已安装的 Java 环境 {#get_java_info}

<primary-label ref="system-action"/>
<secondary-label ref="1.0"/>

- **操作名称**：`get_java_info`
- **所需权限**：`mcsl.daemon.java_info`

### 实现方法 {collapsible="true"}

**一般搜索时：**
- 搜索 `JAVA_HOME`、`PATH` 等环境变量中包含的路径。
- 搜索 `/usr/lib/jvm` 等 Java 默认安装目录。
- Windows 上搜索 `SOFTWARE\\JavaSoft\\JRE` 等注册表。

**深度搜索时：**

通过遍历文件系统，筛选所有包含关键词的目录，例如包含
`oracle` `jre` `jdk` 等关键词的目录，并查找目录下的 Java 环境。

### 请求 {collapsible="true"}

- **参数类型**：`boolean`
- **参数说明**：是否深度搜索。

### 响应 {collapsible="true"}

- **参数类型**：`array<map<str, any>>`
- **参数字段**：
  - `path` - `str`：Java 环境路径。
  - `type` - `str`：Java 类型，`jre` 或 `jdk`。
  - `version` - `str`：Java 版本。
  - `vendor` - `str`：Java 供应商，例如 `Oracle Corporation`、`AdoptOpenJDK` 等。

## 