# 简介

MCSL Future Protocol（简称MFP），是 MCSL Future 中 <tooltip term="wpf">WPF</tooltip>、<tooltip term="web">Web</tooltip> 和 <tooltip term="tauri">Tauri</tooltip> 与 <tooltip term="daemon">Daemon</tooltip> 通讯使用的一种协议。
协议的数据传输全部为 **JSON** 格式。
协议使用 **WebSocket** 作为通讯方式，见[](communication.md)。

协议主要包括 **[操作（Action）](action.md)** 与 **[事件（Event）](event.md)** 两个部分。
操作是客户端主动发起的请求，服务器在收到请求后将会将结果返回给客户端。
而事件则是服务端主动向客户端发送的数据。
