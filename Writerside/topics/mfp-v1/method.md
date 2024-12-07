# 通讯方式

**通信方式（Communication method）** 规定了客户端连接服务器使用的网络协议。
<tooltip term="mfp">MFP</tooltip> 使用 **WebSocket** 进行通讯。

## 连接 {#connect}

1. 使用 HTTP 向 `http(s)://<Daemon 地址>/login` 发生 **POST** 请求以获取令牌
    * 请求表单内容：
        * `usr` - 用户名
        * `pwd` - 密码
        * `expired` - 令牌过期时长（单位：秒），可选，默认 `30`
    * 响应主体为一串文本，即 JWT 令牌
2. 使用令牌连接 WebSocket：`ws(s)://<Daemon 地址>/api/v1?token=<令牌>`

## 通讯

<tooltip term="mfp">MFP</tooltip> 使用 **JSON** 格式 进行通讯，分为 [](action.md) 与 [](event.md) 两种类型。