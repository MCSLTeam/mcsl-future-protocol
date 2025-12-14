# MCSL Future Web 的用户系统

在 MCSL Future Web 中，有一套用户系统，用于管理用户权限。

## 用户模型

用户模型包含以下字段：

|     字段      |          数据类型          |             说明             |
|:-----------:|:----------------------:|:--------------------------:|
|     id      | [uuid](models.md#uuid) |       用户 ID，唯一标识一个用户       |
|    name     |          str           |         用户名称，用于显示          |
|  password   |          str           | 用户密码，用于登录，使用 SHA-256 哈希值存储 |
| permissions |          list          |   用户权限，用于申请子令牌和使用 Web 功能   |

## 用户鉴权

在初次运行 MCSL Future Web 时，系统将开放权限，允许直接注册一个管理员账户。

此后可以通过管理员账户登录 MCSL Future Web。

以下图表展示了用户使用 MCSL Future Web 的流程：

```mermaid
sequenceDiagram
    participant 用户
    participant Web 前端
    participant Web 后端
    participant 守护进程
    用户 ->> Web 前端: 未登录时访问
    Web 前端 -->> 用户: 显示登录页面
    用户 ->> Web 前端: 输入登录信息
    Web 前端 ->> Web 后端: 请求登录
    Web 后端 -->> Web 前端: 返回登录令牌、刷新令牌
    用户 ->> Web 前端: 登录后访问
    Web 前端 ->> Web 后端: 请求获取守护进程信息
    Web 后端 ->> 守护进程: 通过主令牌以及用户权限申请子令牌
    守护进程 -->> Web 后端: 返回子令牌
    Web 后端 -->> Web 前端: 返回守护进程信息与子令牌
    Web 前端 ->> 守护进程: 连接守护进程，请求实例列表等信息
    守护进程 -->> Web 前端: 返回实例列表等信息
    Web 前端 -->> 用户: 显示实例列表等信息
    
    用户 ->> Web 前端: 登录令牌过期时访问
    Web 前端 -->> Web 后端: 执行操作
    Web 后端 -->> Web 前端: 提示登录令牌过期
    Web 前端 ->> Web 后端: 通过刷新令牌请求新令牌
    alt 刷新令牌有效
        Web 后端 -->> Web 前端: 返回新登录令牌、刷新令牌
        Web 前端 -->> Web 后端: 重新执行操作
    else 刷新令牌过期
        Web 后端 -->> Web 前端: 提示刷新令牌过期
        Web 前端 -->> 用户: 提示重新登录
    end
```

## 用户管理

WIP