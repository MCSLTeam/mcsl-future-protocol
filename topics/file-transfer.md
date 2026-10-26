# 文件与控制台二进制传输

V2 文件控制使用 `mcsl.file.*` JSON-RPC method，内容使用同一 WebSocket 连接上的版本化 binary frame。它不使用旧 Base64 chunk action、SHA-1 上传帧或独立 HTTP 上传/下载 endpoint。

单个 session 属于创建它的连接和权限快照。daemon 负责路径边界、offset、长度、SHA-256、超时和清理校验；连接重建后旧 session 不恢复。参数及结果字段以 [](actions.md) 中的 schema 为准。

## 上传

1. 调用 `mcsl.file.upload.open`，提供 daemon-relative path、长度和可选 SHA-256，取得 session ID、chunk 上限和 expiry。
2. 使用 `UploadChunk` binary frame 顺序发送内容；daemon 通过 `mcsl.file.upload.ack` notification 确认 accepted/rejected。
3. 调用 `mcsl.file.upload.close` 完成校验并原子提交，或调用 `mcsl.file.upload.cancel` 清理 staging 文件。

ack 的 `params` 包含 session ID、offset、length、status，以及拒绝时的结构化 error。它是 connection-owned 控制消息，不要求 event subscription，也不等同于最终文件提交成功。

## 下载

1. 调用 `mcsl.file.download.open`，取得 session ID、长度、SHA-256、chunk 上限和 expiry。
2. 调用 `mcsl.file.download.read`；JSON-RPC result 描述 session、offset、length、final，内容紧随为 `DownloadChunk` binary frame。
3. 调用 `mcsl.file.download.close` 释放 session。

客户端应验证 RPC metadata 与收到的 binary frame 相匹配，不把原始字节解释为文本或旧 BigEndianUnicode 字符串。

## Binary header

固定 header 为 **32 bytes**，之后紧跟 payload：

| 字节偏移 | 长度 | 含义 |
| --- | --- | --- |
| 0 | 1 | binary codec version，当前为 `1` |
| 1 | 1 | kind |
| 2 | 2 | reserved，必须全部为零 |
| 4 | 16 | session UUID，big-endian GUID 字节序 |
| 20 | 8 | 非负 offset，signed Int64 little-endian |
| 28 | 4 | payload length，UInt32 little-endian |

| Kind | 值 | 方向 |
| --- | --- | --- |
| `UploadChunk` | 1 | client → daemon |
| `DownloadChunk` | 2 | daemon → client |
| `ConsoleInput` | 3 | client → daemon |
| `ConsoleOutput` | 4 | daemon → client |

默认最大 payload chunk 为 1 MiB；应遵守 session 返回的限制。未知 version/kind、非零 reserved、空 session UUID、负 offset、长度不匹配或超限的帧会被拒绝。这里的 header version `1` 是 V2 内的 binary codec 版本，不是 daemon V1 协议。

## 控制台

通过 `mcsl.instance.console.open`、`mcsl.instance.console.resize` 和 `mcsl.instance.console.close` 管理控制台 session；输入输出复用上述 header，分别使用 `ConsoleInput` 和 `ConsoleOutput`。session 仍由创建它的连接及 daemon 权限校验约束。

字节编码以 Common 的 `Contracts/Protocol/BinaryFrameContracts.cs` 为准，不使用 .NET Guid 的默认混合端序布局。
