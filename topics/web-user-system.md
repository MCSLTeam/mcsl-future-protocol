# Web 用户与短期令牌

C# daemon 不内置 Web 用户数据库。Web 应用或控制面负责用户认证、实例归属、撤销策略和令牌分发；daemon 负责本机协议的认证、permission、路径和生命周期校验。两者不能共用一个“前端已授权”的假设。

## V2 子令牌签发

持有 daemon 主令牌的服务通过 `/api/v2` JSON-RPC method `mcsl.auth.token.issue` 签发短期 JWT，不再通过旧 HTTP `/subtoken` 表单接口申请。

必须同时满足：

- 已使用主令牌身份连接；子令牌不能继续签发。
- 具备 `mcsl.auth.token.issue` 权限。
- daemon 配置启用 `security.allow_main_token_issue`。

请求参数为 `subject`、绝对 URI 格式的 `audience`、非空 `permissions` 和正整数 `ttl_seconds`。权限必须是调用方权限的子集且不能包含裸 `*`，有效期不能超过 `security.max_token_ttl_seconds`。精确 schema 通过 [](actions.md) 获取。

成功结果包含 `token`、`subject`、`audience`、`permissions`、`expires_at` 和 `token_id`。持有短期 JWT 的 client 使用 `/api/v2?token=<jwt>` 建立自己的 WebSocket 连接。

## 安全边界

- 子令牌只能缩小可调用 catalog capability，不能绕过 daemon 的路径、instance lifecycle 或 session ownership 校验。
- 主令牌与平台登录凭据分开保存，不交给普通 Web 用户；连接使用 TLS，并对 URL/query 中的 token 脱敏。
- 不能把签发短期 JWT 解释为 daemon 已提供完整 Web 用户系统、多租户授权、即时撤销链路或不可信代码隔离。
- 外部系统仍需对日志、终端、文件及备份等入口校验主体与实例归属。

权限说明见 [](permissions.md)。
