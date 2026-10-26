# RPC 发现与 schema

完整 RPC 清单由启动后冻结的 typed catalog 生成，本站不再维护旧 `ActionType` enum 或一份独立手写方法表。

## 获取当前运行时定义

在 `/api/v2` 已认证连接上发送：

```json
{
  "jsonrpc": "2.0",
  "id": "discover-1",
  "method": "rpc.discover",
  "params": {}
}
```

成功响应的 `result` 为 OpenRPC document：

- `methods` 描述当前 RPC、params、result schema 和 `x-mcsl-permission` 等 metadata。
- `components.schemas` 提供 catalog 导出的共享 schema。
- `x-mcsl-events` 描述 typed event 的 data/meta schema 及字段 presence。

客户端应读取实际运行 daemon 的文档，而不是假设某个 SDK 产品版本包含全部方法。获准插件可扩展 runtime catalog；未知 method 会被拒绝，不能通过任意字符串绕过注册和权限检查。

## 调试与内置基线

HTTP `GET /apifox.json` 返回当前 catalog 生成的 Apifox 项目。内置协议的 checked-in 基线位于主仓：

```text
src/MCServerLauncher.Daemon/.Resources/Docs/apifox.json
```

它不包含特定部署准入后的插件。维护者在 `MCServerLauncher-Future` 根目录运行以下只校验、不覆盖生成物的命令：

```bash
dotnet run --project tools/MCServerLauncher.ProtocolDocs/MCServerLauncher.ProtocolDocs.csproj -- --check
```

不要手工修改生成 JSON，也不要把 RPC 描述为虚构的 HTTP GET/POST 接口。

## 按用途阅读

- 通用请求和响应：[](action.md)。
- 文件 session 和 binary 内容：[](file-transfer.md)。
- 事件订阅及 catalog 同步：[](event.md)、[](events.md)。
- schema 的源码边界：[](models.md)。
- 权限及主令牌限制：[](permissions.md)、[](web-user-system.md)。

执行创建、启动、停止、备份恢复等操作时，应按所选 method 的实际结果和后续 operation/report 判断完成状态，不把一次 RPC 成功一概解释为业务流程已经完成。
