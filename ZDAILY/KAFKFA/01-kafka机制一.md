
## kafka中的topic
每个 Topic 又被分为多个 Partition，即物理分区。出于负载均衡的考虑，同一个 Topic 的 Partition 分别存储于 Kafka 集群的多个 Broker 上。而为了提高可靠性，这些 Partition 可以由 Kafka 机制中的 Replicas 来设置备份的数量

topic -> patitions 

为了负载均衡、增强可扩展性，Topic 又被分为多个 Partition。从存储层面来看，每一个 Partition 都是一个有序的、不可变的记录序列，通俗点就是一个追加日志（Append Log）文件。每个 Partition 中的记录都会被分配一个称为偏移量（Offset）的序列 ID 号，该序列 ID 号唯一地标识 Partition 内的每个记录。

##  Kafka 为什么要将 Topic 分区？

负载均衡 + 水平扩展。

Topic 只是逻辑概念，面向的是 Producer 和 Consumer，而 Partition 则是物理概念。可以想象，如果 Topic 不进行分区，而将 Topic 内的消息存储于一个 Broker，那么该 Topic 的所有读写请求都将由这个 Broker 处理，吞吐量很容易陷入瓶颈，这显然不适合高吞吐量应用场景。

有了 Partition 概念，假设一个 Topic 被分为 10 个 Partition，Kafka 会根据一定的算法将 10 个 Partition 尽可能均匀地分布到不同的 Broker（服务器）上。当 Producer 发布消息时，Producer 客户端可以采用 Random、Key-Hash 及轮询等算法选定目标 Partition。若不指定，Kafka 也将根据一定算法将其置于某一分区上。Partiton 机制可以极大地提高吞吐量，并且使系统具备良好的水平扩展能力。


> 负载均衡 nginx dubbo kafka 都用了 1 随机，2 hash-key，3 轮训，4 一致性哈希 。水平扩展

实现了负载均衡就等以实现了水平扩展。


### leader选举

复方机制中的leader选举

一个有4个broker  topicA 有 3个patition 每个patition 有3个副本。

patition的3个副本中哪一个是最优秀的

### 复制原理和同步方式

https://gitchat.csdn.net/columnTopic/5b876159ea7bc512755e5ffe

kafka中的分布式一致性算法leader选举。redis的哨兵机制。

###  Kafka 架构中 ZooKeeper 以怎样的形式存在？

ZooKeeper 是一个分布式、开放源码的分布式应用程序协调服务，是 Google 的 Chubby 开源实现。分布式应用程序可以基于它实现统一命名服务、状态同步服务、集群管理、分布式应用配置项的管理等工作。

在基于 Kafka 的分布式消息队列中，ZooKeeper 的作用有 Broker 注册、Topic 注册、Producer 和 Consumer 负载均衡、维护 Partition 与 Consumer 的关系、记录消息消费的进度以及 Consumer 注册等。

1. Broker 在 ZooKeeper 中的注册
2. Topic 在 ZooKeeper 中的注册
3. Consumer 在 ZooKeeper 中的注册
4. Producers 负载均衡
5. Consumer 负载均衡
6. 记录消费进度 Offset
7. 记录 Partition 与 Consumer 的关系
