---
weight: 10
title: 'test page'
---

## text

### plain
**This is Bold**

### color

## link

[home](/) 
```
[home](/)
```  
[external](https://example.com) 
```
[external](https://example.com)
```
<a href="https://example.com" target="_blank" rel="noopener noreferrer">external in new tab</a> 
```
<a href="https://example.com" target="_blank" rel="noopener noreferrer">external in new tab</a>
```

## image

![some image](/cat.jpg)
```
![some image](/cat.jpg)
```

## mermaid

{{< mermaid >}}
sequenceDiagram
    participant Event
    participant Reactor
    participant Demultiplex
    participant EventHandler

    Event ->> Reactor: 注册 Event 和 Handler
    loop 事件集合
        Reactor -> Reactor: Event 集合
    end

    Reactor ->> Demultiplex: 向 Epoll add/mod/del Event (注意没有给它 Handler 的信息)
    Reactor ->> Demultiplex: 启动反应堆
    loop 事件分发器
        Demultiplex -> Demultiplex: 开启事件循环 epoll_wait
    end

    Demultiplex ->> Reactor: 返回发生事件的 Event
    Reactor ->> EventHandler: 调用 Event 对应的事件处理器 EventHandIer
{{< /mermaid >}}

```
{{</* mermaid */>}}
sequenceDiagram
    participant Event
    participant Reactor
    participant Demultiplex
    participant EventHandler

    Event ->> Reactor: 注册 Event 和 Handler
    loop 事件集合
        Reactor -> Reactor: Event 集合
    end

    Reactor ->> Demultiplex: 向 Epoll add/mod/del Event (注意没有给它 Handler 的信息)
    Reactor ->> Demultiplex: 启动反应堆
    loop 事件分发器
        Demultiplex -> Demultiplex: 开启事件循环 epoll_wait
    end

    Demultiplex ->> Reactor: 返回发生事件的 Event
    Reactor ->> EventHandler: 调用 Event 对应的事件处理器 EventHandIer
{{</* /mermaid */>}}
```

## code block

```cpp
#include <iostream>

int main() {
  std::cout << "hello\n";
  return 0;
}
```

