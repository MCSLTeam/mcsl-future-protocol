# 数据模型

数据模型是在 [](action.md) 和 [](event.md) 中常用的类型。

## `map<str, any>` 类型

### FileData 文件数据 {#file-data}

| 字段名  | 数据类型                                                                      | 说明                                                                                                    |
|------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| name | str                                                                       | 文件名称                                                                                                  |
| type | str                                                                       | 文件类型，`file` 或 `directory`                                                                             |
| meta | [FileMetadata](#file-metadata) / [DirectoryMetadata](#directory-metadata) | 文件元数据：<br/>- `type` 为 `file` 时为 **FileMetadata**；<br/>- `type` 为 `directory` 时为 **DirectoryMetadata** |

### FileMetadata 文件元数据 {#file-metadata}

| 字段名              | 数据类型        | 说明       |
|------------------|-------------|----------|
| read_only        | bool        | 是否为只读    |
| size             | int64       | 文件大小     |
| creation_time    | timestamp32 | 文件创建时间   |
| last_write_time  | timestamp32 | 文件上次写入时间 |
| last_access_time | timestamp32 | 文件上次访问时间 |

### DirectoryMetadata 目录元数据 {#directory-metadata}

| 字段名              | 数据类型        | 说明       |
|------------------|-------------|----------|
| hidden           | bool        | 是否为隐藏目录  |
| link_target      | str         | 目录路径     |
| creation_time    | timestamp32 | 目录创建时间   |
| last_write_time  | timestamp32 | 目录上次写入时间 |
| last_access_time | timestamp32 | 目录上次访问时间 |

### InstanceConfig 实例配置 {#instance-config}

| 字段名             | 数据类型                           | 说明                                                       |
|-----------------|--------------------------------|----------------------------------------------------------|
| id              | bin8                           | 实例的唯一标识符，类型为 UUID v4                                     |
| name            | str                            | 实例名称                                                     |
| type            | [InstanceType](#instance-type) | 实例类型                                                     |
| startup_cmd     | str                            | 服务器启动命令                                                  |
| arguments       | array<str>                     | Java 参数或可执行文件参数列表，可选，缺省时为空                               |
| input_encoding  | str                            | `stdin` 编码，可选，缺省时默认为 `UTF-8`                             |
| output_encoding | str                            | `stdout` 编码，可选，缺省时默认为 `UTF-8`                            |
| env             | map<str, str>                  | 环境变量，可选，缺省时为空；<br/>可使用 `{NAME}` 占位符，将替换为名为 `NAME` 的环境变量值 |

### InstanceInstallationInfo 实例安装信息 {#instance-installation-info}

| 字段名             | 数据类型                                      | 说明                                                           |
|-----------------|-------------------------------------------|--------------------------------------------------------------|
| name            | str                                       | 实例名称                                                         |
| type            | `minecraft` &verbar; `other`              | 实例类型：<br/>- `minecraft`： Minecraft JE 实例；<br/>- `other`：其他实例 |
| startup_cmd     | str                                       | 服务器启动命令                                                      |
| install_method  | `core` &verbar; `pack` &verbar; `archive` | 实例安装方式                                                       |
| mc_version      | string                                    | Minecraft JE 版本，非 Minecraft JE 实例或非 `core` 安装方式时忽略           |
| loader_version  | string                                    | 模组 / 插件端版本，非 Minecraft JE 实例或非 `core` 安装方式时忽略                |
| mirror          | [Mirror](#mirror)                         | 下载镜像，非 Minecraft JE 实例或非 `core` 安装方式时忽略，可选，缺省时默认使用官方源        |
| arguments       | array<str>                                | Java 参数或可执行文件参数列表，可选，缺省时为空                                   |
| input_encoding  | str                                       | `stdin` 编码，可选，缺省时默认为 `UTF-8`                                 |
| output_encoding | str                                       | `stdout` 编码，可选，缺省时默认为 `UTF-8`                                |
| env             | map<str, str>                             | 环境变量，可选，缺省时为空；<br/>可使用 `{NAME}` 占位符，将替换为名为 `NAME` 的环境变量值     |

### InstanceReport 实例状态 {#instance-report}

| 字段      | 数据类型                                 | 说明                                                                                                                                                |
|---------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| status  | [InstanceStatus](#instance-status)   | 实例运行状态                                                                                                                                            |
| version | str                                  | 实例的 Minecraft 版本，非 Minecraft JE 实例时不存在                                                                                                            |
| motd    | str                                  | 实例的服务器介绍，非 Minecraft JE 实例时不存在                                                                                                                    |
| players | list<[Player](#player)> &verbar; nil | 在线玩家列表，非 Minecraft JE 实例时不存在，1.7之前的版本由于 [SLP协议](https://minecraft.wiki/w/Minecraft_Wiki:Projects/wiki.vg_merge/Server_List_Ping) 没有玩家列表字段，为 `nil` |

### Player 玩家信息 {#player}

| 字段   | 数据类型 | 说明      |
|------|------|---------|
| name | str  | 玩家名称    |
| uuid | bin8 | 玩家 UUID |

## 枚举类型

### InstallationMethod 实例安装方式 {#installation-method}

- 数据类型：`str`

| 枚举值  | 说明                                                        | 
|------|-----------------------------------------------------------|
| core | 从网络下载核心安装                                                 |
| pack | 安装 **MCSL Future 服务器包** 文件（`.mcslpack`），可能会覆盖实例工厂设置中的部分字段 | 
| zip  | 解压归档文件（`.zip`）到实例目录下                                      | 

### InstanceType 实例类型 {#instance-type}

- 数据类型：`str`
- 枚举值：
    - `none` - 非 Minecraft JE 服务器实例
    - 或 `vanilla`, `fabric`, `quilt`, `forge`, `neoforge`, `spigot`, `paper` 等

### Mirror 镜像类型 {#mirror}

- 数据类型：`str`

| 枚举值     | 说明                                             |
|---------|------------------------------------------------|
| vanilla | 官方源                                            |
| bmcl    | [BMCL Api](https://bmclapidoc.bangbang93.com/) |

### InstanceStatus 实例运行状态 {#instance-status}

- 数据类型：`str`

| 枚举值        | 说明     |
|------------|--------|
| installing | 实例安装中  |
| starting   | 实例正在启动 |
| running    | 实例正在运行 |
| stopping   | 实例正在停止 |
| stopped    | 实例未运行  |
