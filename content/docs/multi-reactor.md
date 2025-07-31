---
weight: 2
title: "multi-Reactor 网络库模块"
---

# multi-Reactor 网络库模块

## 技术要点

基于muduo网络库改写，用C++11特性取代boost依赖，  
使用非阻塞IO + epoll 的IO模型，  
使用线程池维护若干子事件循环，  
使用eventfd作为主线程到子线程的通知机制，  
利用面向对象的编程思想封装TCP连接，需要监听的事件等;  

## 时序图

{{< mermaid >}}
sequenceDiagram
    participant TcpServer
    participant TcpConnection (or Acceptor)
    participant EventLoop
    participant Poller
    participant Channel

    TcpServer ->> TcpConnection (or Acceptor): 注册自定义 Handler

    TcpConnection (or Acceptor) ->> EventLoop: 注册 fd 和 Handler (即 Channel)
    loop 事件集合
        EventLoop -> EventLoop: Channel 集合
    end

    EventLoop ->> Poller: 向 Epoll add/mod/del Event (注意没有给它 Handler 的信息)
    EventLoop ->> Poller: 启动反应堆
    loop 事件分发器
        Poller -> Poller: 开启事件循环 epoll_wait
    end

    Poller ->> EventLoop: 返回发生事件的 Channel
    EventLoop ->> Channel: 调用 Channel 对应的事件处理器
{{< /mermaid >}}

