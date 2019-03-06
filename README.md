
Kafka是一个分布式流数据系统，使用Zookeeper进行集群的管理。与其他消息系统类似，
整个系统由生产者、Broker Server和消费者三部分组成，生产者和消费者由开发人员编写，通过API连接到Broker Server进行数据操作。

我们重点关注三个概念：
Topic，是Kafka下消息的类别，类似于RabbitMQ中的Exchange的概念。
    这是逻辑上的概念，用来区分、隔离不同的消息数据，屏蔽了底层复杂的存储方式。
    对于大多数人来说，在开发的时候只需要关注数据写入到了哪个topic、从哪个topic取出数据。
Partition，是Kafka下数据存储的基本单元，这个是物理上的概念。
    同一个topic的数据，会被分散的存储到多个partition中，这些partition可以在同一台机器上，也可以是在多台机器上，
    比如下图所示的topic就有4个partition，分散在两台机器上。
    这种方式在大多数分布式存储中都可以见到，比如MongoDB、Elasticsearch的分片技术，
    其优势在于：有利于水平扩展，避免单台机器在磁盘空间和性能上的限制，同时可以通过复制来增加数据冗余性，提高容灾能力。
    为了做到均匀分布，通常partition的数量通常是Broker Server数量的整数倍。
Consumer Group，同样是逻辑上的概念，是Kafka实现单播和广播两种消息模型的手段。同一个topic的数据，会广播给不同的group；
    同一个group中的worker，只有一个worker能拿到这个数据。
    换句话说，对于同一个topic，每个group都可以拿到同样的所有数据，但是数据进入group后只能被其中的一个worker消费。
    group内的worker可以使用多线程或多进程来实现，也可以将进程分散在多台机器上，worker的数量通常不超过partition的数量，
    且二者最好保持整数倍关系，因为Kafka在设计时假定了一个partition只能被一个worker消费（同一group内）。

参考网址：https://blog.csdn.net/zwgdft/article/details/54633105