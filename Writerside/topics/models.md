# 数据模型

数据模型是在 [](action.md) 和 [](event.md) 中常用的一组 `object` 类型数据

## FileData 文件数据 {#file-data}

| 字段名  | 数据类型                                                                      | 介绍                                                                                                      |
|------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| name | string                                                                    | 文件名称                                                                                                    |
| type | string                                                                    | 文件类型，`file` 或 `directory`                                                                               |
| meta | [FileMetadata](#file-metadata) / [DirectoryMetadata](#directory-metadata) | 文件元数据<br/>  - `type` 为 `file` 时为 **FileMetadata**<br/>  - `type` 为 `directory` 时为 **DirectoryMetadata** |

## FileMetadata 文件元数据 {#file-metadata}

| 字段名              | 数据类型    | 介绍        |
|------------------|---------|-----------|
| read_only        | boolean | 是否为只读     |
| size             | long    | 文件大小      |
| creation_time    | long    | 文件创建时间戳   |
| last_write_time  | long    | 文件上次写入时间戳 |
| last_access_time | long    | 文件上次访问时间戳 |

## DirectoryMetadata 文件元数据 {#directory-metadata}

| 字段名              | 数据类型    | 介绍        |
|------------------|---------|-----------|
| hidden           | boolean | 是否为隐藏目录   |
| link_target      | string  | 目录路径      |
| creation_time    | long    | 目录创建时间戳   |
| last_write_time  | long    | 目录上次写入时间戳 |
| last_access_time | long    | 目录上次访问时间戳 |