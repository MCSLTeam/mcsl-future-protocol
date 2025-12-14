# 数据模型

数据模型是在 [](action.md) 和 [](event.md) 中常用的类型。

## uuid {#uuid}

<primary-label ref="msgpack-ext"/>
<secondary-label ref="1.0"/>

- 数据编号：`0`（`0x00`）
- 数据类型：`fixext16`
- 数据说明：用于存储 UUID v4 数据
- 数据格式：

```
+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+
|  0xd8  |  0x00  | UUID 数据，二进制大端序存储，16 字节长度                                                                                                           |
+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+--------+
```

## FilePath 文件路径 {#file_path}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段名      | 数据类型            | 说明                      |
|----------|-----------------|-------------------------|
| instance | [`uuid`](#uuid) | 实例 ID                   |
| path     | `str`           | 文件路径，表示实例下的相对路径，Unix 风格 |

## FileData 文件数据 {#file-data}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段名               | 数据类型                     | 说明                                    |
|-------------------|--------------------------|---------------------------------------|
| path              | [`FilePath`](#file_path) | 文件路径                                  |
| type              | `str`                    | 文件类型，始终为 `file`                       |
| creation_time     | `timestamp32`            | 创建时间                                  |
| modification_time | `timestamp32`            | 上次修改时间                                |
| permission        | `str` &verbar; 无         | 文件权限，仅在 Unix-like 系统中存在，数字格式，例如 `755` |
| size              | `int64`                  | 文件大小，单位为字节                            |

## DirectoryData 目录元数据 {#directory_data}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段名               | 数据类型                                                   | 说明                                          |
|-------------------|--------------------------------------------------------|---------------------------------------------|
| path              | [`FilePath`](#file_path)                               | 文件路径                                        |
| type              | `str`                                                  | 文件类型，始终为 `directory`                        |
| creation_time     | `timestamp32`                                          | 创建时间                                        |
| modification_time | `timestamp32`                                          | 上次修改时间                                      |
| permission        | `str` &verbar; 无                                       | 文件权限，仅在 Unix-like 系统中存在，数字格式，例如 `755`       |
| hidden            | `bool`                                                 | 是否为隐藏目录                                     |
| link_target       | [`FilePath`](#file_path) &verbar; `str` &verbar; `nil` | 符号链接目标路径，如果不在守护进程中则为 `str`，如果没有符号链接则为 `nil` |

## InstanceBasicInfo 实例基本信息 {#instance_basic_info}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段名  | 数据类型                     | 说明                 |
|------|--------------------------|--------------------|
| id   | [`uuid`](models.md#uuid) | 实例的唯一标识符，也是实例文件夹名称 |
| name | `str`                    | 实例名称               |

## InstanceTypeInfo 实例类型信息 {#instance_type_info}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段名            | 数据类型                     | 说明               |
|----------------|--------------------------|------------------|
| type           | [`Core`](model-cores.md) | 实例类型             |
| core_version   | `str`                    | 核心版本             |
| loader_version | `str` &verbar; 无         | 加载器版本，仅存在于第三方服务端 |
| mirror         | [`Mirror`](#mirror)      | 镜像源              |

## InstanceConfig 实例配置 {#instance_config}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段名             | 数据类型                    | 说明                                             |
|-----------------|-------------------------|------------------------------------------------|
| startup_cmd     | `str`                   | 启动命令                                           |
| stop_cmd        | `str`                   | 停止命令                                           |
| input_encoding  | [`Encoding`](#encoding) | 标准输入编码                                         |
| output_encoding | [`Encoding`](#encoding) | 标准输出编码                                         |
| env             | `map<str, str>`         | 环境变量<br/>可使用 `{NAME}` 占位符，将替换为名为 `NAME` 的环境变量值 |

## InstanceStatusInfo 实例运行状态信息 {#instance_status_info}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段名          | 数据类型                                 | 说明                                 |
|--------------|--------------------------------------|------------------------------------|
| status       | [`InstanceStatus`](#instance_status) | 实例状态                               |
| pid          | `int32` &verbar; `nil`               | 进程 ID，若实例未运行则为 `nil`               |
| cpu_usage    | `float32` &verbar; `nil`             | CPU 占用率，0-100 之间的整数，若实例未运行则为 `nil` | |
| memory_usage | `int64` &verbar; `nil`               | 内存占用率，单位为字节，若实例未运行则为 `nil`         |         |
| disk_usage   | `int64`                              | 磁盘占用率，单位为字节                        |

## MCServerPing 服务器信息 {#mc_server_ping}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段            | 数据类型                                 | 说明                                                                                                                          |
|---------------|--------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| name          | `str`                                | Minecraft 版本名称                                                                                                              |
| protocol      | `int32`                              | Minecraft 协议版本                                                                                                              |
| online_player | `int32`                              | 在线玩家数量                                                                                                                      |
| max_player    | `int32`                              | 最大玩家数量                                                                                                                      |
| motd          | `str`                                | 服务器介绍                                                                                                                       |
| players       | list<[Player](#player)> &verbar; nil | 在线玩家列表，1.7之前的版本由于 [SLP协议](https://minecraft.wiki/w/Minecraft_Wiki:Projects/wiki.vg_merge/Server_List_Ping) 没有玩家列表字段，为 `nil` |

## Player 玩家信息 {#player}

<primary-label ref="type-map"/>
<secondary-label ref="1.0"/>

| 字段   | 数据类型                     | 说明      |
|------|--------------------------|---------|
| name | `str`                    | 玩家名称    |
| uuid | [`uuid`](models.md#uuid) | 玩家 UUID |

## InstanceStatus 实例运行状态 {#instance_status}

<primary-label ref="type-enum"/>
<secondary-label ref="1.0"/>

- **数据类型**：`str`

| 枚举值        | 说明                      |
|------------|-------------------------|
| installing | 实例安装中                   |
| starting   | 实例正在启动                  |
| running    | 实例正在运行                  |
| stopping   | 实例正在停止                  |
| stopped    | 实例未运行                   |
| crashed    | 实例已崩溃，仅在崩溃停止时出现，保持 3 分钟 |

## Encoding 编码 {#encoding}

<primary-label ref="type-enum"/>
<secondary-label ref="1.0"/>

- **数据类型**：`str`

| 枚举值  | 说明       |
|------|----------|
| utf8 | UTF-8 编码 |
| gbk  | GBK 编码   |

### Mirror 镜像类型 {#mirror}

<primary-label ref="type-enum"/>
<secondary-label ref="1.0"/>

- 数据类型：`str`

| 枚举值     | 说明                                             |
|---------|------------------------------------------------|
| vanilla | 官方源                                            |
| bmcl    | [BMCL Api](https://bmclapidoc.bangbang93.com/) |

## 核心类型

<primary-label ref="type-enum"/>
<secondary-label ref="1.0"/>

见 [](model-cores.md)。