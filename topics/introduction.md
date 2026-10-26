# MCSL Future V2 通信协议

本文档描述当前 .NET daemon、daemon client、WPF client 和 Web/Tauri 控制台使用的 **V2 协议**。唯一 WebSocket endpoint 为 `/api/v2`：控制请求使用 JSON-RPC 2.0，事件使用 JSON-RPC notification，文件和控制台字节使用版本化 binary frame。

daemon 已删除 V1 endpoint、旧 action/retcode envelope 和兼容解析器。本网站不是 V1 接入指南，也不为实验性 Rust daemon 声明 V2 兼容性。

## 权威来源

协议以 [MCServerLauncher-Future 的实现](https://github.com/MCSLTeam/MCServerLauncher-Future)为准：

- `MCServerLauncher.Common/Contracts/Protocol` 定义 JSON-RPC wire envelope 和 binary codec；其他 `Contracts` 目录定义共享 wire DTO。
- `MCServerLauncher.Daemon.API/Protocol/BuiltInProtocolDefinitions.cs` 定义内置 typed RPC/event descriptor 和 JSON metadata。
- daemon 启动并完成插件准入后冻结 typed catalog，作为 dispatch、权限 metadata 和运行时发现文档的唯一来源。
- 连接后调用 `rpc.discover` 获取 runtime OpenRPC；HTTP `GET /apifox.json` 由同一 catalog 生成，包含已准入插件。仓库中的 checked-in Apifox 基线仅覆盖内置协议。

本站文字说明不能替代运行时 schema，不维护独立的手写方法、权限或 DTO 注册表。本轮对齐的源码提交及维护方式见[仓库 README](https://github.com/MCSLTeam/mcsl-future-protocol/blob/main/README.md)。

## 接入流程

1. 通过 `GET /info` 获取 daemon 的 `api_version: "v2"`，见 [](connection.md)。
2. 使用主令牌或短期 JWT 连接 `ws(s)://<host>:<port>/api/v2?token=<token>`。
3. 按 [](action.md) 发送带 `id` 的 JSON-RPC 请求，先通过 [](actions.md) 获取当前 catalog。
4. 按 [](event.md) 建立 typed subscription；重连后重新订阅并同步 instance catalog。
5. 文件内容按 [](file-transfer.md) 使用 binary session 传输。

主令牌不应交给普通 Web 用户。权限边界见 [](permissions.md)，外部用户认证与短期令牌签发见 [](web-user-system.md)。

## 版本边界

WebSocket 协议版本、binary header 版本、SDK 包版本和扩展子协议版本分别管理。V2 当前使用版本为 `1` 的 binary header；扩展 envelope 的 `mcsl.extension.v1` 也不代表恢复 daemon V1 endpoint。不能把其他服务（例如插件 Registry）的 `/api/v1/...` 当作 daemon V1。
