---
weight: 2
title: "Multi-Reactor 网络库"
---

# Multi-Reactor 网络库

## 技术要点

1. 基于 muduo 网络库改写，用 C++11 特性取代 boost 依赖，  

2. 使用非阻塞 socket + epoll 的 IO 模型，  

3. 使用线程池维护若干子事件循环，  

4. 使用 eventfd 作为主线程到子线程的通知机制，  

5. 利用面向对象的编程思想封装了相关联的数据和函数  
如  
Channel 封装了一个文件描述符、该文件描述符所感兴趣的事件、事件到达时的处理函数 `Channel::handleEvents`  
TcpConnection 封装了一个 TCP 连接相关的数据：inputBuffer, outputBuffer, connChannel.  
   

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

