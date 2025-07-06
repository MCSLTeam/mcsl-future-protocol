# 通讯方式

**通信方式（Communication method）** 规定了客户端连接服务器使用的网络协议。
<tooltip term="mfp">MFP</tooltip> 使用 **HTTP(S)** 和 **WebSocket** 进行通讯。

## 获取信息

使用 HTTP 向 `http(s)://<Daemon 地址>/subtoken` 发送 **GET** 请求以获取信息。

响应为 **JSON**，包含：

* `name` - 程序名称，如 `MCServerLauncher Future Daemon CSharp`
* `version` - 程序版本，如 `1.0.0.0`
* `apiVersion` - API 版本，用于确定 **WebSocket** 连接地址，如 `v1`

## 令牌

令牌（Token）是一串用来鉴权的字符串，可用于连接 <tooltip term="daemon">Daemon</tooltip>。

<tooltip term="mfp">MFP</tooltip> 应有两种令牌，**主令牌** 和 **子令牌**。

### 主令牌

主令牌（Main Token）是一个 32 位的随机字母与数字的字符串，可用于申请子令牌。

主令牌会在 <tooltip term="daemon">Daemon</tooltip> 首次运行时生成并在控制台输出。

主令牌作为令牌连接 <tooltip term="daemon">Daemon</tooltip> 时拥有全部 [](mfp-permissions.md)。

<tip>
主令牌正则表达式为 <code>/^[a-zA-Z0-9]{32}$/</code>
</tip>

### 子令牌

子令牌（Sub Token）是一个 [JWT](https://jwt.io/)，需通过主令牌申请获得。

## 申请子令牌

使用 HTTP 向 `http(s)://<Daemon 地址>/subtoken` 发送 **POST** 请求以申请子令牌。

请求表单内容：

* `token` - 主令牌，必需
* `expires` - 令牌过期时长（单位：秒），可选，默认 `30`
* `permissions` - 子令牌的权限，使用 `,` 分隔，可选，默认 `*`

响应主体为一串 JWT，即子令牌

## 连接

使用 WebSocket 连接 `ws(s)://<Daemon 地址>/api/<API版本>?token=<子令牌>` 即可。

## 通讯

<tooltip term="mfp">MFP</tooltip> 使用 **JSON** 格式 进行通讯，分为 [](action.md) 与 [](event.md) 两种类型。