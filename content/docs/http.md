---
weight: 3
title: "HTTP 框架"
---

# HTTP 模块

## 技术要点

1. 提供 HTTP 请求的解析，HTTP 请求报文和回复报文的封装；    

2. 提供 HttpServer::onConnection, HttpServer::onMessage 两个回调函数，用于向底层的网络库注册。

3. 路由模块：  
是 HttpServer 的内置模块，HttpServer 利用 Router 来管理和路由用户所注册的路由路径。    
使用方法+路径两个参数唯一地映射一个路由；     

4. 会话管理模块：  
是一个独立模块，由用户根据是否需要会话管理来选用。     
客户端 Cookie 字段存储 SessionId, 服务端存储会话相关数据；    

# 如何实现

从软件开发的分层视角来看，HTTP 框架所做的就是使用下层的网络库提供的接口，然后向上层的用户提供 Web 开发所需的 HTTP 接口。

{{< mermaid >}}
sequenceDiagram
    WebApp(用户) ->> HttpServer: 注册HttpCallback函数,其中有WebApp的业务逻辑, 由Router管理
    HttpServer ->> TcpServer: 注册TcpServer所需要的connectCallback, messageCallback函数
    网络接口 ->> TcpServer: TcpServer上有新的连接到达
    TcpServer ->> TcpServer: 新的TcpConnection对象被创建，<br/>并且HttpServer::onConnection()被调用，新的HttpContext对象被创建并被该TcpConnection使用。
    loop
    网络接口 ->> TcpServer: TcpServer上有数据到达
    TcpServer ->> HttpServer: TcpServer将数据读取到用户空间Buffer<br/>并且调用HttpServer::onMessage()函数解析HTTP报文
    end
    HttpServer ->> HttpServer: HttpServer解析到完整的HttpRequest
    HttpServer ->> HttpServer: 调用 HttpServer::onRequest()，<br/>进一步调用 router_.route()<br/>构造好HttpResponse对象
    HttpServer ->> TcpServer: 将HttpResponse对象序列化到Buffer中交给TcpConnection::send()
    TcpServer ->> 网络接口: 通过socket发送数据
{{< /mermaid >}}
