---
layout: post
title: Linux常用命令(长期更新)
date: 2020-03-10
author: linweiyu
tags: [Linux]
comments: true
toc: true

---

# Linux常用命令

## 配置SSH免密登录

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
```

### netstat

-a 列出所有链接

-t 只列出tcp协议的链接 -u 只列出UDP协议的链接

-n 禁用反向域名解析，加快查询速度

-l 只列出监听中的链接（后台进程打开的端口）

-p 获取进程名，进程号和进程id

-s 打印网络统计数据

-r 打印内核路由信息

-i 打印网络接口 该项输出信息和ifconfig一致

-c 持续输出信息

example

```
#监听处于active状态的连接
netstat -atnp | grep ESTABLISHED
watch -d -n0 "netstat -atnp | grep ESTABLISHED"
```



### ps

列出所有正在运行的进程

```
ps -aux 
```

列举所有进程且显示完整命令字符

```
ps -auxww
```



## find

1. 选择搜索路径的起点

2. 搜索策略

   -name 按名称搜索文件 （支持模糊匹配 * （需加单引号或双引号））

   -ls 使搜索结果带有文件的详细信息例如长度权限等

   -size 按文件大小来搜索 支持k, M, G 等 

   ```shell
   查找大于1G的文件
   find / -size +1G
   ```

   -inum 通过索引节点查找文件

   -user -group 查找文件所有者或组的文件 

   -nouser -nogroup查找没有所有者或组的文件

   -mtime 按上次更新时间查找

   ```shell
   查找过去24小时更新的文件 （若未指定unit 默认为24小时）
   find / -mtime -1
   ```

   -ctime 查询上次修改过文件状态的文件(同上时间单位默认为24小时)

   -atime 查询上次访问过文件(同上时间单位默认为24小时)

   -newer 查询比某文件更新的文件  !newer为更旧

   -type 按文件类型查找文件 例如 d:目录 f: 常规文件 l:符号链接

   -maxdepth -mindepth 限制查找的深度

   -empty 查找空文件

   -perm 按权限查找文件

   ```shell
   find / -perm 666 -type f -ls
   ```

   -exec -ok(会在每个文件操作前做确认)对查找到的文件执行其他命令

   ```shell
   find . -name test -exec rm {} \;
   ```



## Java相关

### jps

-q 列出所有JVM进程的pid

-m 列出全部JVM进程的参数

-l 列出所有进程的完整的包名称

-v 列出所有JVM进程的JVM参数

### jstack

jstack {pid} 为java进程中的所有线程打印Java堆栈跟踪

jstack -m {pid} 为java进程中的所有线程打印混合代码的（Java/C++）堆栈跟踪



## MD5

Linux: md5sum [filename]

windows: Get-FileHash C:\Windows\notepad.exe -Algorithm MD5| Format-List （SHA1、SHA256、SHA384、SHA512、MACTripleDES、MD5、RIPEMD160）



## TOP

version

```
top -v
```

Process State

- Runnable (R): A process in this state is either executing on the CPU, or it is present on the run queue, ready to be executed.
- Interruptible sleep (S): Processes in this state are waiting for an event to complete.
- Uninterruptible sleep (D): In this case, a process is waiting for an I/O operation to complete.
- Stopped (T): These processes have been stopped by a [job control signal](https://www.gnu.org/software/libc/manual/html_node/Job-Control-Signals.html) (such as by pressing Ctrl+Z) or because they are being traced.
- Zombie (Z): The kernel maintains various data structures in memory to keep track of processes. A process may create a number of child processes, and they may exit while the parent is still around. However, these data structures must be kept around until the parent obtains the status of the child processes. Such terminated processes whose data structures are still around are called zombies.

CPU usage

`us` the time the CPU spends executing processes in userspace

`sy` the time spent on running kernelspace processes

`ni` nice value, priority in CPU

load average

represents the average “load” over one, five and fifteen minutes.

PID

This is the process ID, a unique positive integer that identifies a process.

USER

This is the “effective” username (which maps to a user ID) of the user who started the process. Linux assigns a real user ID and an effective user ID to processes; the latter allows a process to act on behalf of another user. (For example, a non-root user can elevate to root in order to install a package.)

PR and NI

The “NI” field shows the “nice” value of a process. The “PR” field shows the scheduling priority of the process from the perspective of the kernel. The nice value affects the priority of a process.

VIRT, RES, SHR and %MEM

These three fields are related with to memory consumption of the processes. “VIRT” is the total amount of memory consumed by a process. This includes the program’s code, the data stored by the process in memory, as well as any regions of memory that have been swapped to the disk. “RES” is the memory consumed by the process in RAM, and “%MEM” expresses this value as a percentage of the total RAM available. Finally, “SHR” is the amount of memory shared with other processes.

kill process

'k' -> enter process id -> enter signal(SIGTERM(15:  allows processes to terminate gracefully); SIGKILL(9:kill a process forcefully))

sort 

- ‘M’ to sort by memory usage

- ‘P’ to sort by CPU usage

- ‘N’ to sort by process ID

- ‘T’ to sort by the running time

- or 

  ```
  top -o %CPU
  ```

show threads

'H' or top -H 

show full path

'c' or top -c

## 时间校对

```
ntpdate cn.pool.ntp.org
```
cn.pool.ntp.org是ntp网络授时组织的中国授时源