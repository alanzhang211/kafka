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