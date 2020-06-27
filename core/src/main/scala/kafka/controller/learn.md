## controller
+ 为集群中的所有主题分区选举领导者副本.
+ 承载着集群的全部元数据信息，并负责将这些元数据信息同步到其他Broker上。

### 集群元数据
Broker通过Controller实现和ZK（**社区打算将ZK去掉**）的元数据操作。

#### 元数据的构成
1. topic列表（无法删除、正在删除、待删除的列表）
2. 副本（不可用路径上的副本列表、副本可用性状态汇总）
3. 分区（分区可用状态汇总、正在执行重分配的分区列表、不可用（离线）分区信息）
4. topic（topic分区leader详情、集群topic列表、topic副本详情信息）
5. broker（关闭中broker列表、运行中broker列表、运行中broker的Epoch）
6. controller（controller状态信息、controller epoch、controller epoch znode的版本号）

#### 核型类
##### ControllerContext
