# MCSL Future 通信协议

MCSL Future 采用 [客户端-服务器架构](https://zh.wikipedia.org/wiki/%E5%AE%A2%E6%88%B7%E7%AB%AF-%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9E%B6%E6%9E%84){ignore-vars="true"} ，由守护进程和客户端两部分组成：

- **守护进程（Daemon）**：负责管理 Minecraft 服务器实例；
- **客户端（Client）**：提供用户界面，并通过与守护进程通信实现对服务器的操作。

**MCSL Future 通信协议（MCSL Future Protocol，简称 MFP）** 是用于规范守护进程与客户端之间通信的协议标准。

连接守护进程需要令牌以进行鉴权，详见 **[](connection.md)**。

通讯建立后，使用 **MessagePack** 格式进行数据传输，主要采用基于 **WebSocket** 的通信模式。

通讯包含以下两个核心部分：

- **[](action.md)**：客户端发起请求，服务端响应操作结果；
- **[](event.md)**：服务端主动向客户端推送消息或状态更新。
