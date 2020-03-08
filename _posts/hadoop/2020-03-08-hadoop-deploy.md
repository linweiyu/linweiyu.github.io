---
layout: post
title: Hadoop 部署
date: 2020-03-08
author: linweiyu
tags: [bigdata,hadoop]
comments: true
toc: true
---

# Hadoop 部署

## Single Node

### 配置

etc/hadoop/core-site.xml:

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

etc/hadoop/hdfs-site.xml:

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

### SSH免密登录

```shell
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```

### 启动

1. 格式文件系统

   ```shell
    $ bin/hdfs namenode -format
   ```

2. 启动namenode和datanode进程

   ```shell
     $ sbin/start-dfs.sh
   ```

lsof -i |grep java  可通过该方式找到管理页面端口





以上内容来源自网络整理