# 权限

每个 RPC/event descriptor 在 frozen typed catalog 中声明 permission。`*` 表示任意已认证连接；其他值必须由当前 token 的 permission 集合满足。实际权限 metadata 随 `rpc.discover` 导出，见 [](actions.md)，不从旧 action 名称或页面目录推导。

## 校验边界

daemon 在执行 application method、建立 subscription 和创建 file/console session 前进行权威校验。前端或外部控制面的预检查仅用于交互，不能替代 daemon-side permission、路径、instance lifecycle 和 session ownership 校验。

事件订阅不仅受目标 event 的 permission 约束，还需要调用者具备订阅 RPC 本身的权限。event meta 也必须符合 catalog 的 presence/schema 规则。

## 查询当前权限

具有 `mcsl.auth.permissions.get` 权限的连接可调用：

```json
{
  "jsonrpc": "2.0",
  "id": "permissions-1",
  "method": "mcsl.auth.permissions.get",
  "params": {}
}
```

返回结果中的 `permissions` 描述当前连接的权限；它不是绕过其他 method 检查的能力。

## 主令牌与子令牌

主令牌属于 daemon 管理信任边界，不应传入普通租户浏览器、日志或诊断包。外部用户系统可按 [](web-user-system.md) 使用主令牌签发受限短期 JWT。

签发需要额外的 daemon 配置允许、主令牌身份和 `mcsl.auth.token.issue` 权限。子令牌只能缩小 capability，不等于提供租户隔离或操作系统沙箱。不能根据旧文档中宽泛的 handler 权限推断当前 RPC 可调用。
