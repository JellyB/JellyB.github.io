title: zookeeper使用手册
author: jelly
tags:
  - zookeeper
categories:
  - zookeeper
date: 2018-05-24 17:04:00
---
可能需要关闭防火墙

第五步：启动ZooKeeper集群
在ZooKeeper集群的每个结点上，执行启动ZooKeeper服务的脚本，如下所示:
```bash
cd /home/hadoop
cd zk/
```
#查看启动日志情况
```bash
tail -n 200 -f zookeeper.out 
```
###### 启动每个zk节点
```bash
[root@slave-00 zookeeper-3.4.8]# bin/zkServer.sh start
[root@slave-01 zookeeper-3.4.8]# bin/zkServer.sh start
[root@slave-02 zookeeper-3.4.8]# bin/zkServer.sh start
```

第六步：安装验证
可以通过ZooKeeper的脚本来查看启动状态，包括集群中各个结点的角色（或是Leader，或是Follower），如下所示，是在ZooKeeper集群中的每个结点上查询的结果：
```bash
[root@slave-00 zookeeper-3.4.8]# bin/zkServer.sh status 
```
```bash_out
ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper-3.4.8/bin/../conf/zoo.cfg
Mode: leader
```
```bash
[root@slave-01 zookeeper-3.4.8]# bin/zkServer.sh status
```
```bash_out
ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper-3.4.8/bin/../conf/zoo.cfg
Mode: follower
```
```bash
[root@slave-02 zookeeper-3.4.8]# bin/zkServer.sh status 
```
```bash_out
ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper-3.4.8/bin/../conf/zoo.cfg
Mode: follower
```