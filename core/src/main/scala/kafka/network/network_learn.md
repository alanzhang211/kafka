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

#### AbstractServerThread

> Kafka中的线程控制代码大量使用了基于 CountDownLatch 的编程技术，依托于它来实现优雅的线程启动、线程关闭等操作。因此，我建议你熟练掌握它们，并应用到你日后的工作当中。

#### Acceptor
这是接收和创建外部 TCP 连接的线。每个SocketServer实例只会创建一个Acceptor。

#### Processor
处理单个 TCP 连接上所有请求的线程。每个 SocketServer 实例默认创建若干个（num.network.threads）Processor 线程.

#### Processor
Processor 伴生对象类：仅仅定义了一些与 Processor 线程相关的常见监控指标和常量等。

#### ConnectionQuotas
是控制连接数配额的类。我们能够设置单个 IP 创建 Broker 连接的最大数量，以及单个 Broker 能够允许的最大连接数。

