# 数据模型

数据模型是在 [](action.md) 和 [](event.md) 中常用的类型。

## `map` 类型

### FileData 文件数据 {#file-data}

| 字段名  | 数据类型                                                                      | 说明                                                                                                    |
|------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| name | string                                                                    | 文件名称                                                                                                  |
| type | string                                                                    | 文件类型，`file` 或 `directory`                                                                             |
| meta | [FileMetadata](#file-metadata) / [DirectoryMetadata](#directory-metadata) | 文件元数据：<br/>- `type` 为 `file` 时为 **FileMetadata**；<br/>- `type` 为 `directory` 时为 **DirectoryMetadata** |

### FileMetadata 文件元数据 {#file-metadata}

| 字段名              | 数据类型  | 说明        |
|------------------|-------|-----------|
| read_only        | bool  | 是否为只读     |
| size             | int64 | 文件大小      |
| creation_time    | int64 | 文件创建时间戳   |
| last_write_time  | int64 | 文件上次写入时间戳 |
| last_access_time | int64 | 文件上次访问时间戳 |

### DirectoryMetadata 目录元数据 {#directory-metadata}

| 字段名              | 数据类型   | 说明        |
|------------------|--------|-----------|
| hidden           | bool   | 是否为隐藏目录   |
| link_target      | string | 目录路径      |
| creation_time    | int64  | 目录创建时间戳   |
| last_write_time  | int64  | 目录上次写入时间戳 |
| last_access_time | int64  | 目录上次访问时间戳 |

### InstanceFactorySetting 实例工厂设置 {#instance-factory-setting}

决定守护进程如何安装实例的设置，继承自 [InstanceConfig](#instance-config)

| 字段名           | 数据类型                                  | 说明                                 |
|---------------|---------------------------------------|------------------------------------|
| source        | string                                | 安装实例所需的源文件, 可以是链接,也可以是相对路径         |
| source_type   | [SourceType (enum)](#source-type)     | 源文件类型 (Core, Archive, Script)      |
| name          | string                                | 实例名称                               |
| instance_type | [InstanceType (enum)](#instance-type) | 实例类型 (vanilla, fabric, forge, ...) |
| target        | string                                | 服务器启动目标文件路径(jar文件名,脚本名)            |
| target_type   | [TargetType (enum)](#target-type)     | 服务器启动目标类型 (Jar, Script)            |

- 可选字段:

| 字段名             | 数据类型                                                     | 缺省时说明                                                                                    |
|-----------------|----------------------------------------------------------|------------------------------------------------------------------------------------------|
| mirror          | [InstanceFactoryMirror (enum)](#instance-factory-mirror) | 镜像，可选，缺省时默认使用官方源                                                                         |
| uuid            | string (uuid v4)                                         | 实例的唯一标识符，可选，缺省时由Daemon自动生成                                                               |
| mc_version      | string                                                   | Minecraft版本 (必须是1.x.x类型的字符串)，可选，当instance_type是InstanceType.None时可缺省, 即代表安装非java版mc服务器实例 |
| java_path       | string                                                   | Java路径，可选，当target_type不为TargetType.Jar时可缺省, 否则在daemon校验时会返回错误                            |
| arguments       | string[]                                                 | Java参数列表/可执行文件参数列表，可选，缺省时为空, 或在Daemon安装时自动生成                                             |
| input_encoding  | string                                                   | stdin编码，可选，缺省时为utf-8                                                                     |
| output_encoding | string                                                   | stdout编码，可选，缺省时为utf-8                                                                    |
| env             | dict[string, [PlaceHolderString](#place-holder-string)]  | 指定的环境变量字典，可选，缺省时为空                                                                       |

## InstanceConfig 实例配置 {#instance-config}

决定Daemon如何启动实例的设置, 包含必选字段和可选字段.

| 字段名           | 数据类型                                  | 说明                                                                                                            |
|---------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------|
| name          | string                                | 实例名称                                                                                                          |
| instance_type | [InstanceType (enum)](#instance-type) | 实例类型 (vanilla, fabric, forge, ...)                                                                            |
| target        | string                                | 服务器启动目标文件路径(jar文件名,脚本名), 可能与[InstanceFactorySetting](#instance-factory-setting)中的target不同(视Daemon内置的实例工厂行为而定) |
| target_type   | [TargetType (enum)](#target-type)     | 服务器启动目标类型 (Jar, Script, Executable)                                                                           |

- 可选字段:

| 字段名             | 数据类型                                                    | 说明                           | 缺省时说明                                                    |
|-----------------|---------------------------------------------------------|------------------------------|----------------------------------------------------------|
| uuid            | string (uuid v4)                                        | 实例的唯一标识符                     | 缺省时由Daemon自动生成                                           |
| mc_version      | string                                                  | Minecraft版本 (必须是1.x.x类型的字符串) | 当instance_type是InstanceType.None时可缺省, 即代表安装非java版mc服务器实例 |
| java_path       | string                                                  | Java路径                       | 当target_type不为TargetType.Jar时可缺省, 否则在daemon校验时会返回错误      |
| arguments       | string[]                                                | Java参数列表/可执行文件参数列表           | 缺省时为空, 或在Daemon安装时自动生成                                   |
| input_encoding  | string                                                  | stdin编码                      | 缺省时为utf-8                                                |
| output_encoding | string                                                  | stdout编码                     | 缺省时为utf-8                                                |
| env             | dict[string, [PlaceHolderString](#place-holder-string)] | 指定的环境变量字典                    | 缺省时为空                                                    |

## 枚举类型

## SourceType 实例工厂设置 - 源文件类型 {#source-type}

| 枚举值     | 说明                                                                                                                | 
|---------|-------------------------------------------------------------------------------------------------------------------|
| core    | 需安装的源文件为核心                                                                                                        |
| archive | 需安装的源文件为归档文件(zip), 应为使用MCSL-Future打包分发的服务器压缩包(可能会覆盖[InstanceFactorySetting](#instance-factory-setting)中的部分字段和可选字段 | 
| script  | 需安装的源文件为脚本文件(bat, sh), 即使用该脚本自动安装服务器实例                                                                            |

## InstanceType 实例工厂设置-实例类型 {#instance-type}

| 枚举值       | 说明                                         |
|-----------|--------------------------------------------|
| none      | 无实例类型, 即非java版mc服务器实例                      |
| universal | 原版服务端的安装方式, 即下载服务器核心就可以直接运行                |
| fabric    | [Fabric](https://fabricmc.net/)            |
| forge     | [Forge](https://files.minecraftforge.net/) |
| neo_forge | [NeoForge](https://neoforged.net/)         |
| cleanroom | [Cleanroom](https://cleanroommc.com/zh/)   |
| quilt     | [Quilt](https://quiltmc.org/)              |

## TargetType 实例工厂设置-目标文件类型 (枚举) {#target-type}

| 枚举值        | 说明                 |
|------------|--------------------|
| jar        | 目标文件是jar文件         |
| script     | 目标文件是脚本文件(bat, sh) |
| executable | 目标文件是可执行文件         |

## TargetType 实例工厂设置-加速镜像 (枚举) {#instance-factory-mirror}

| 枚举值      | 说明                                             |
|----------|------------------------------------------------|
| none     | 官方源(默认)                                        |
| bmcl_api | [BMCL Api](https://bmclapidoc.bangbang93.com/) |

## InstanceReport 实例状态 {#instance-report}

| 字段         | 数据类型                                      | 说明                                                                                                                         |
|------------|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| status     | [InstanceStatus (enum)](#instance-status) | 实例运行状态                                                                                                                     |
| config     | [InstanceConfig](#instance-config)        | 实例配置                                                                                                                       |                     |        |
| properties | list[string]                              | server.properties                                                                                                          |
| players    | list[[Player](#player)]                   | 在线玩家列表(1.7以前的版本由于[slp协议](https://minecraft.wiki/w/Minecraft_Wiki:Projects/wiki.vg_merge/Server_List_Ping)没有玩家列表字段, 故总为空列表) |

## Player 玩家信息{#player}

| 字段   | 数据类型   | 说明     |
|------|--------|--------|
| name | string | 玩家名称   |
| uuid | string | 玩家UUID |

## InstanceStatus 实例运行状态 {#instance-status}

| 枚举值      | 说明     |
|----------|--------|
| starting | 实例正在启动 |
| running  | 实例正在运行 |
| stopping | 实例正在停止 |
| stopped  | 实例未运行  |
| crashed  | 实例崩溃   |
