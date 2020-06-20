## 请求处理模块
### 请求队列（requestChannel）
#### Request
##### 主要参数
###### processor
Processor线程的序号，即这个请求是由哪个Processor线程接收处理的。Broker 端参数 `num.network.threads` 控制了 Broker 每个监听器上创建的 Processor 线程数。

###### context
context 是用来标识请求上下文信息的

###### memoryPool
定义的一个非阻塞式的内存缓冲区，主要作用是避免 Request 对象无限使用内存

###### buffer
buffer 是真正保存 Request 对象内容的字节缓冲区

#### Response


#### RequestChannel

### SocketServer
主要实现了 Reactor 模式，用于处理外部多个 Clients（这里的 Clients 指的是广义的 Clients，可能包含 Producer、Consumer 或其他 Broker）的并发请求，并负责将处理结果封装进 Response 中，返还给 Clients。

