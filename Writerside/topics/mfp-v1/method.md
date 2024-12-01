# 通讯方式

**通信方式（Communication method）** 规定了客户端连接服务器使用的网络协议。
<tooltip term="mfp">MFP</tooltip> 使用 **WebSocket** 进行通讯。

## 连接 {#connect}

连接 <tooltip term="daemon">Daemon</tooltip> 时，须使用 WebSocket 连接 URL：
**ws(wss)://&lt;IP>/api/v1?token=&lt;令牌>**

如果令牌正确，将连接升级为 WebSocket。

如果令牌错误，返回 HTTP 状态码：400。

## 通讯

<tooltip term="mfp">MFP</tooltip> 使用 **JSON** 格式 进行通讯，分为 [](action.md) 与 [](event.md) 两种类型。