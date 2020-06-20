## 高水位管理操作
### 更新高水位
场景不同：
+ updateHighWatermar：主要用在 Follower 副本从 Leader 副本获取到消息后更新高水位值。一旦拿到新的消息，就必须要更新高水位值。
+ maybeIncrementHighWatermark：主要是用来更新 Leader 副本的高水位值。

### 读取高水位
+ fetchHighWatermarkMetadata

## 日志段管理
日志是日志段的容器。

### 如何承担日志段容器
使用`ConcurrentSkipListMap` 结构保存`LogSegment`。
**优势：**
+ 线程安全：这样 Kafka 源码不需要自行确保日志段操作过程中的线程安全。
+ 键值（Key）可排序的 Map：Kafka 将每个日志段的起始位移值作为 Key。
这样一来，我们就能够很方便地根据所有日志段的起始位移值对它们进行排序和比较，同时还能快速地找到与给定位移值相近的前后两个日志段。

### 核心操作
#### 增加
#### 删除
kafka的留存策略。
+ deletaOldSegments()：三种留存策略。

## 关键位移管理
+ Log Start Offset：
+ LEO：LEO 永远指向下一条待插入消息
+ HW：

### 为什么需要LEO
+ **Log对象初始化时：**

### 读取
+ 核心方法：read(startOffset: Long, maxLength: Int, isolation: FetchIsolation, minOneMessage: Boolean)
#### 主要设计
+ 读取消息的时候，读取LEO没有使用`Monitor`锁，使用本地变量来减少锁的竞争。

---
## 索引管理
### 核心类
+ AbstractIndex:定义了最顶层的抽象类，封装了所有索引类型的公共操作。
+ LazyIndex：定义了AbstractIndex 上的一个包装类，实现索引项延迟加载。这个类主要是为了提高性能。
+ OffsetIndex：定义位移索引，保存“< 位移值，文件磁盘物理位置 >”对。
+ TimeIndex：定义时间戳索引，保存“< 时间戳，位移值 >”对。
+ TransactionIndex：定义事务索引，为已中止事务（Aborted Transcation）保存重要的元数据信息。只有启用 Kafka 事务后，这个索引才有可能出现。

### Kafka 中的索引底层的实现原理
**内存映射文件，即 Java 中的 MappedByteBuffer**
+ 优势
1. 它有很高的 I/O 性能，特别是对于索引这样的小文件来说，由于文件内存被直接映射到一段虚拟内存上，访问内存映射文件的速度要快于普通的读写文件速。
2. 映射的内存区域实际上就是内核的页缓存（Page Cache）。这就意味着，里面的数据不需要重复拷贝到用户态空间，避免了很多不必要的时间、空间消耗。

### 核心设计
#### 索引查找
标准版使用`二分查找`
改进版将索引分为`冷区`、`热区`，优化了索引的页缓存，*有效地提升页缓存的使用率，从而在整体上降低物理 I/O，缓解系统负载瓶颈。*
