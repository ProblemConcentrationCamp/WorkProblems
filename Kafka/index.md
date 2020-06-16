
https://www.cnblogs.com/qingyunzong/p/9004509.html        1.kafka简介

https://www.cnblogs.com/qingyunzong/p/9004593.html         2. kafka的架构

https://www.cnblogs.com/qingyunzong/p/9004703.html       3. kafaka的高可用

cnblogs.com/qingyunzong/p/9005062.html                  4. kafka的安装

https://www.cnblogs.com/qingyunzong/p/9007107.html   5.Kafka在zookeeper中的存储

#### 什么是Kafka？ ####
分布式消息系统。

#### 消息传递模式 #####
 * 点对点模式
 
 生产者的消息持久化到一个队列，可以有一个或多个消费者进行消费。但是一条消息只能被消费一次。当某条数据被消费后，
 将会从消息队列中删除。这种模式下，即便有多个消费者，也能保证数据处理的顺序。
 
 生产者发送一条消息到queen，只有一个消费者能收到。
 
 * 发布-订阅模式
 
 发布者的消息持久化到一个topic。一个订阅者可以订阅一个或多个topic。订阅者可以消费topic的所有数据，同一条消息
 可以被多个消费者消费。
 
 发布者发送到topic的消息，只有订阅了该topic的消费者才会收到。
 
#### Kafka 基本概念 ##### 

1. topic 

每条发布到Kafka集群的消息都有一个类别，这个类别称为topic。

2. partition

topic的数据被分别存储在一个或多个partition。

一个topic至少有一个partition。如果一个topic的副本数为3。那么Kafka集群将会为每个partition创建3个相同的副本。
每个partition中的数据使用多个segment文件保存。

partition中的数据是有序的，但是不同partition之间的顺序无法保证。所以在需要严格保证消息的消费顺序时，需要将
partition数目设置为1。

3. broker

Kafka集群包含一个或多个服务器。每个服务器节点称为broker。broker会存储partition的数据。

4. producer

生产者。该角色将消息发布到kafka的topic，broker接受到消息后，将消息追加到某个partition的segment文件中。
也可指定partition。

5. consumer

消费者。从broker读取数据。可以消费多个topic的数据

6. consumer group

每个consumer属于一个特定的consumer group。

7. leader

每个partition有多个副本。有且仅有一个座位leader，负责读写

8. follower

follower跟随leader。两者的数据保持同步。如果leader死亡，会从follwer中选举一个新的leader。
当follower挂掉或同步太慢，leader会将其重ISR同步列表中删除，重新创建一个leader。

#### kafka 相关特性 ####
1. 数据删除

一般的message queue 在消息被消费后会删除数据，而kafka不管消费与否，都不会删除。但是提供了两种删除策略。
一个是基于时间。一个是基于文件大小。

2. 已消费消息的记录

当前消费消息的offest，由Consume自己控制。一般在成功消费一条记录后，递增该offset。所以broker是无状态的，
它不需要标记哪些消息已被消费。也不需要去保证同一个consumer group 只有一个consumer 消费。

3. 消息路由

可以指定Topic的partition数量。通常在保存时会根据key进行hash算法和%机制进行负载均衡的保存

4. Consume group

在高版本的API中。同一个Topic的一条消息只能被同一个Consume group 内的一个 consumer 消费。不同consumer group可以
同时消费。


5. 消息从生产者到broker的保证： ACK机制 

指的是producer的消息发送确认机制，这直接影响到Kafka集群的吞吐量和消息可靠性。而吞吐量和可靠性就像硬币的两面，两者不可兼得，只能平衡。

0 ：
意味着producer不等待broker同步完成的确认，继续发送下一条(批)信息
提供了最低的延迟。但是最弱的持久性，当服务器发生故障时，就很可能发生数据丢失。例如leader已经死亡，producer不知情，还会继续发送消息broker接收不到数据就会数据丢失。

1：
意味着producer要等待leader成功收到数据并得到确认，才发送下一条message。此选项提供了较好的持久性较低的延迟性。
Partition的Leader死亡，follwer尚未复制，数据就会丢失

-1
意味着producer得到follwer确认，才发送下一条数据持久性最好，延时性最差。

6. 消息从 broker到消费者的保证
consumer从broker读取到消息后，需要进行commit，该操作会将该consumer对应的partition的offset递增。

可以设置为autoCommit 自动提交。也可以设置为手动提交。

在实际的应用中，并非是读取到数据就算结束。而是将数据进一步正确处理后才能算结束。所以消息从 broker到消费者的保证，
可以正确处理消息后，手动commit，才算一次成功的消费。



