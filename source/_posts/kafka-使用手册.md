title: kafka 使用手册
author: jelly
date: 2018-05-24 17:16:53
tags:
---
#### 【启动Kafka 服务】
```bash
> bin/kafka-server-start.sh config/server.properties
```
#### 【创建主题】
```bash
> bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```
#### 【查看主题列表】
```
> bin/kafka-topics.sh --list --zookeeper localhost:2181
```
#### 【创建生产者】
```
> bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
```
#### 【创建消费者】
```
> bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
> bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic filebeat-log --from-beginning
```
#### 【创建Kafka集群】
```
> cp config/server.properties config/server-1.properties
> cp config/server.properties config/server-2.properties
```

```cnf
config/server-1.properties:
    broker.id=1
    listeners=PLAINTEXT://172.16.3.212:9093
    log.dir=/tmp/kafka-logs-1

config/server-2.properties:
    broker.id=2
    listeners=PLAINTEXT://172.16.3.212:9094
    log.dir=/tmp/kafka-logs-2
```
####  【分别启动刚刚配置的Kafka节点】
```
> bin/kafka-server-start.sh config/server-1.properties &
> bin/kafka-server-start.sh config/server-2.properties &
```
#### 【集群方式启动Kafka】
```
> bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic my-replicated-topic
```
#### 【查看每个节点信息1】
```
> bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic
```

```out
Topic:my-replicated-topic	PartitionCount:1	ReplicationFactor:3	Configs:
Topic: my-replicated-topic	Partition: 0	Leader: 1	Replicas: 1,2,0	Isr: 1,2,0
```
#### 【查看每个节点信息1】
```
> bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test
> bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic filebeat-log
```

```out
Topic:test	PartitionCount:1	ReplicationFactor:1	Configs:
Topic: test	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
 ```	

#### 【创建生产者】

``` bash
> bin/kafka-console-producer.sh --broker-list 192.168.8.147:9092 --topic my-replicated-topic
```

```out
my test message 1
my test message 2
```
#### 【创建消费者】
```
> bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic my-replicated-topic
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic UCTopic
```
```out
my test message 1
my test message 2
```

#### 【查看容灾性】
```
> ps | grep server-1.properties
7564 ttys002    0:15.91 /System/Library/Frameworks/JavaVM.framework/Versions/1.6/Home/bin/java...
> kill -9 7564
```

#### 【查看Leader变化】
```
> bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic
```
```out
Topic:my-replicated-topic	PartitionCount:1	ReplicationFactor:3	Configs:
Topic: my-replicated-topic	Partition: 0	Leader: 2	Replicas: 1,2,0	Isr: 2,0
```
	
#### 【查看消费者能否收到消息】
```
> bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic my-replicated-topic
```
```out
my test message 1
my test message 2
```
#### 【创建主题创建多个副本及切片】
```
> bin/kafka-topics.sh --zookeeper 192.168.6.89:2181 --topic filebeat-log2 --replication-factor 2 --partitions 1 --create
```

#### 【彻底删除kafka主题】
[link](https://stackoverflow.com/questions/33537950/how-to-delete-a-topic-in-apache-kafka)


Deletion of a topic has been supported since 0.8.2.x version. You have to enable topic deletion (setting delete.topic.enable to true) on all brokers first.

Follow this step by step process for manual deletion of topics

###### 1、Stop Kafka server
###### 2、Delete the topic directory with rm -rf command
###### 3、Connect to Zookeeper instance: zookeeper-shell.sh host:port
###### 4、ls /brokers/topics
###### 5、Remove the topic folder from ZooKeeper using rmr /brokers/topics/yourtopic
###### 6、Restart Kafka server
###### 7、Confirm if it was deleted or not by using this command kafka-topics.sh --list --zookeeper host:port


#### 【kafka 数据写入到hdfs方案】
	数据从Kafka到Hdfs的一些pipeline，如下
##### > Kafka -> Flume –> Hadoop Hdfs

	常用方案,基于配置,需要注意hdfs小文件性能等问题.
GitHub地址:  [link](https://github.com/apache/flume)

##### > Kafka -> Kafka Hadoop Loader ->Hadoop Hdfs

	Kafka Hadoop Loader通过为kafka Topic下每个分区建立对应的split来创建task实现增量的加载数据流到hdfs,上次消费的partition offset是通过zookeeper来记录的.简单易用.
 [link](https://github.com/michal-harish/kafka-hadoop-loader)

##### > Kafka -> KaBoom -> Hadoop Hdfs

	KaBoom是一个借助Krackle(开源的kafka客户端，能极大的减少对象的创建，提高应用程序的性能)来消费kafka的Topic分区数据随后写如hdfs,利用Curator和Zookeeper来实现分布式服务,能够灵活的根据topic来写入不同的hdfs目录.
[link]( https://github.com/blackberry/KaBoom)

##### > Kafka -> Kafka-connect-hdfs -> Hadoop Hdfs

	Confluent的Kafka Connect旨在通过标准化如何将数据移入和移出Kafka来简化构建大规模实时数据管道的过程。可以使用Kafka Connect读取或写入外部系统，管理数据流并扩展系统，而无需编写新代码.

[link](https://github.com/confluentinc/kafka-connect-hdfs)

##### > Kafka -> Gobblin -> Hadoop Hdfs

	Gobblin是LinkedIn开源的一个数据摄取组件.它支持多种数据源的摄取，通过并发的多任务进行数据抽取，转换，清洗，最终加载到目标数据源.支持单机和Hadoop MR二种方式，而且开箱即用，并支持很好的扩展和二次开发.
[link](https://github.com/linkedin/gobblin)

参考资料:

[link1](https://www.confluent.io/blog/how-to-build-a-scalable-etl-pipeline-with-kafka-connect)
[link2](http://gobblin.readthedocs.io/en/latest/Getting-Started/)
[link3](http://gobblin.readthedocs.io/en/latest/case-studies/Kafka-HDFS-Ingestion/)
[link4](https://github.com/confluentinc/kafka-connect-blog)
[link5](http://docs.confluent.io/3.1.1/connect/connect-hdfs/docs/index.html)