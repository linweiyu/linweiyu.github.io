---
layout: post
title: Linux常用命令(长期更新)
date: 2020-03-08
author: linweiyu
tags: [Linux]
comments: true
toc: true
pinned: true

---

# Linux常用命令

## 配置免密登录

```shell
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```



## Spark

### Spark env 配置

```shell
-- spark-env.sh
export SPARK_MASTER_HOST=10.10.86.236
export SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=10.0.83.99:2181,10.10.86.236:2181,10.10.86.237:2181 -Dspark.deploy.zookeeper.dir=/home/spark/spark"
```

### 启动从节点

```shell
./start-slave.sh spark://10.10.86.236:7077,10.10.86.237:7077,10.10.83.99:7077
```

### 启动主节点

```shell
./start-master.sh
```

## 端口进程查询

```shell
netstat -tunlp|grep port
lsof -i
ps -aux |grep pid
```

