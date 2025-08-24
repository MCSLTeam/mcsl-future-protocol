# 连接守护进程

## 信息获取

向 `http(s)://<Daemon 地址>/info` 发起 **GET** 请求，可获取服务器基本信息。

### 响应字段

| 字段      | 类型     | 说明                                              |
|---------|--------|-------------------------------------------------|
| name    | string | 程序名称，例如 `MCServerLauncher Future Daemon CSharp` |
| version | string | 程序版本号，如 `1.0.0.0`                               |
| api     | number | 当前 API 版本，固定为 `1`                               |

### 响应示例

```json
{
  "name": "MCServerLauncher Future Daemon CSharp",
  "version": "1.1.4.5",
  "api": 1
}
```

## 令牌

**令牌（Token）** 是一串随机生成且不可预测的字符串，用于连接守护进程时的身份鉴权。

连接支持两种令牌类型：**主令牌** 与 **子令牌**。

### 主令牌

**主令牌（Main Token）** 由守护进程生成，固定不变。

主令牌可以连接守护进程，亦可用于申请子令牌。

### 子令牌

**子令牌（Sub Token）** 需通过主令牌调用接口申请获得。

子令牌设有过期时间，超时后将无法建立新连接，已建立的连接不受影响。

## 申请子令牌

向 `http(s)://<Daemon 地址>/api/v1/subtoken` 发送 **POST** 请求，请求体为 JSON 格式。

### 请求字段

| 字段          | 类型               | 说明                   |
|-------------|------------------|----------------------|
| token       | string           | 主令牌                  |
| expires     | number           | 过期时间戳                |
| permissions | array&lt;string> | 权限列表，非必需，默认为 `["*"]` |

### 请求示例

```json
{
  "token": "主令牌",
  "expires": 0,
  "permissions": [
    "*"
  ]
}
```

## 建立连接

使用 WebSocket 连接以下地址即可建立通信：

```uri
ws(s)://<Daemon 地址>/api/v1?token=<子令牌>
```

## 通信格式

建立连接后将使用 **MessagePack** 格式进行通信，消息主要分为 [](mfp-action.md) 与 [](mfp-event.md) 两种。