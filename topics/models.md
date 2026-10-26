# 数据模型与序列化

当前 schema 以 `rpc.discover` 返回的 OpenRPC 文档为准，见 [](actions.md)。本站不再维护旧 `ProtoType.Action`/`Event` 模型表，也不把所有 Common 类型都视为远程协议。

## 源码边界

- `MCServerLauncher.Common/Contracts`：共享 wire DTO、JSON-RPC envelope、typed event 和 binary codec。
- `MCServerLauncher.Daemon.API`：传输无关应用/插件契约与 typed protocol descriptor，不是 daemon 的内部实现程序集。
- daemon 启动后冻结的 typed catalog：决定实际注册的 params/result、event data/meta schema 和权限，包括已准入插件。

内置 metadata 使用显式 source-generated `JsonTypeInfo`；协议不通过反射回退或程序集扫描猜测 DTO。

## 主要 contract 组

| Contract 组 | 内容 |
| --- | --- |
| `Contracts.Instances` | instance configuration、report、catalog state 等实例相关值 |
| `Contracts.Files` | contained path、文件 metadata、upload/download session |
| `Contracts.System` | OS、CPU、memory、drive、Java runtime |
| `Contracts.EventRules` | event-rule query/update 和 persisted rule models |
| `Contracts.Protocol` | JSON-RPC envelope、typed event、OpenRPC 和 binary frame |
| 其他已注册 contract | operation、provisioning、backup、automation、monitoring、audit、auth 等；是否可调用以 runtime catalog 为准 |

## Wire 约定

- JSON 字段使用 catalog/context 定义的名称，通常为 snake_case；显式名称和 schema 优先，不从 C# 成员名自行推导。
- UUID 在 JSON 中为字符串；在 binary header 中使用 big-endian GUID 字节序。
- enum 的 wire 字符串以对应 schema 为准，不从 UI 标签推导。
- Int64 request ID 在浏览器中宜使用字符串；其他数字字段仍按 schema 处理。
- 时间字段按各自 schema 编解码。event `timestamp` 是 Unix milliseconds，不能将所有 DateTime/expiry 字段一概解释为毫秒数。
- event `meta`/`data` 的 missing、null 和 object 含义不同，见 [](event.md)。
- 文件和控制台内容走 [](file-transfer.md) 中的 binary session，不推定为 JSON Base64 chunk。

SDK 包版本、产品版本和 wire 版本不是同一个版本轴。客户端应拒绝不支持的 endpoint/frame，并按发现文档使用已注册 capability，而不是自动尝试旧 envelope。
