---
title: Redis的两种持久化方式
date: 2023-08-06 18:28:51
tags: Redis
---

# Redis持久化

Redis有两种持久化方案：

- RDB持久化
- AOF持久化

## RDB持久化

RDB全称Redis Database Backup file（Redis数据备份文件），也被叫做Redis数据快照。简单来说就是把内存中的所有数据都记录到磁盘中。当Redis实例故障重启后，从磁盘读取快照文件，恢复数据。快照文件称为RDB文件，默认是保存在当前运行目录。

### 执行时机

RDB持久化在四种情况下会执行：

- 执行save命令
- 执行bgsave命令
- Redis停机时(会默认保存数据)
- 触发RDB条件时

并且在redis服务器启动时，会自动去读取之前存储的RDB文件加载到redis中

**1）save命令**

执行下面的命令，可以立即执行一次RDB：

![img](../pic/Redis%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%8C%81%E4%B9%85%E5%8C%96%E6%96%B9%E5%BC%8F/1689684119804-8b8a1076-13d3-4d07-b2dc-49634f587d55.png)

save命令会导致主进程执行RDB，这个过程中其它所有命令都会被阻塞。只有在数据迁移时可能用到。

**2）bgsave命令**

下面的命令可以异步执行RDB：

![img](../pic/Redis%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%8C%81%E4%B9%85%E5%8C%96%E6%96%B9%E5%BC%8F/1689684119883-6b15ff22-fde6-4f68-a927-e5e4c7546e33.png)

这个命令执行后会开启独立进程完成RDB，主进程可以持续处理用户请求，不受影响。

**3）停机时**

Redis停机时会执行一次save命令，实现RDB持久化。

**4）触发RDB条件**

Redis内部有触发RDB的机制，可以在redis.conf文件中找到，格式如下：

```properties
# 900秒内，如果至少有1个key被修改，则执行bgsave ， 如果是save "" 则表示禁用RDB
save 900 1  
save 300 10  
save 60 10000
```

RDB的其它配置也可以在redis.conf文件中设置：

```properties
# 是否压缩 ,建议不开启，压缩也会消耗cpu，磁盘的话不值钱
rdbcompression yes

# RDB文件名称
dbfilename dump.rdb  

# 文件保存的路径目录
dir ./
```

### RDB原理

bgsave开始时会fork**主进程**得到**子进程**(注意这里都是进程，而不是线程),子进程共享主进程的内存数据（其实就是页表，因为他们不可能直接操作内存，操作的是虚拟内存，也就是页表中的数据，然后操作系统会帮我们把页表中的数据写入到真实地址中）。完成fork后读取内存数据并写入 RDB 文件。

fork采用的是copy-on-write技术：

- 当主进程执行读操作时，访问共享内存(这里面的内存数据是只读的)；
- 当主进程执行写操作时，则会拷贝一份数据（这里可能会占用双倍的内存空间，这种可能发生的情况是所有的readonly数据都被修改了，但是这种情况很极端，基本不可能发生），执行写操作。

![img](../pic/Redis%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%8C%81%E4%B9%85%E5%8C%96%E6%96%B9%E5%BC%8F/1689684119959-72152cd0-89fa-4968-bbc4-6f882a6d72fe.png)

### 小结

RDB方式bgsave的基本流程？

- fork主进程得到一个子进程，共享内存空间
- 子进程读取内存数据并写入新的RDB文件
- 用新RDB文件替换旧的RDB文件

RDB会在什么时候执行？save 60 1000代表什么含义？

- 默认是服务停止时
- 代表60秒内至少执行1000次修改则触发RDB

RDB的缺点？

- RDB执行间隔时间长，两次RDB之间写入数据有丢失的风险（如果之间宕机了，之间的操作不就没保存下来嘛）
- fork子进程、压缩、写出RDB文件都比较耗时

## AOF持久化

### AOF原理

AOF全称为Append Only File（追加文件）。Redis处理的每一个写命令都会记录在AOF文件，可以看做是命令日志文件（很像数据库的日志文件）。

![img](../pic/Redis%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%8C%81%E4%B9%85%E5%8C%96%E6%96%B9%E5%BC%8F/1689684120037-816a79ee-9170-44c5-bd1a-37a3d6582b43.png)

### 1.2.2.AOF配置

AOF默认是关闭的，需要修改redis.conf配置文件来开启AOF：

```properties
# 是否开启AOF功能，默认是no
appendonly yes
# AOF文件的名称
appendfilename "appendonly.aof"
```

AOF的命令记录的频率也可以通过redis.conf文件来配：

```properties
# 表示每执行一次写命令，立即记录到AOF文件
appendfsync always 
# 写命令执行完先放入AOF缓冲区，然后表示每隔1秒将缓冲区数据写到AOF文件，是默认方案
appendfsync everysec 
# 写命令执行完先放入AOF缓冲区，由操作系统决定何时将缓冲区内容写回磁盘
appendfsync no
```

三种策略对比：

![img](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/1689684120121-575dd159-bdb3-4bf9-a15a-8e0ce1d721f0.png)

### 1.2.3.AOF文件重写

因为是记录命令，AOF文件会比RDB文件大的多。因为**AOF会记录对同一个key的多次写操作，但只有最后一次写操作才有意义**。所以可以通过执行bgrewriteaof命令，让AOF文件执行重写功能，用最少的命令达到相同效果。

![img](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/1689684120202-6d0dc601-628b-4ac9-8bec-4694aa78938e.png)

如图，AOF原本有三个命令，但是set num 123 和 set num 666都是对num的操作，第二次会覆盖第一次的值，因此第一个命令记录下来没有意义。

所以重写命令后，AOF文件内容就是：mset name jack num 666

Redis也会在触发阈值时自动去重写AOF文件。阈值也可以在redis.conf中配置：

```properties
# AOF文件比上次文件 增长超过多少百分比则触发重写
auto-aof-rewrite-percentage 100
# AOF文件体积最小多大以上才触发重写 
auto-aof-rewrite-min-size 64mb
```

## RDB与AOF对比

RDB和AOF各有自己的优缺点，如果对数据安全性要求较高，在实际开发中往往会**结合**两者来使用。

![img](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/1689684120284-028e12cb-369d-436d-bb83-5972550357c3.png)

Redis的持久化虽然可以保证数据安全，但也会带来很多额外的开销，因此持久化请遵循下列建议：

- 用来做缓存的Redis实例尽量不要开启持久化功能
- 建议关闭RDB持久化功能，使用AOF持久化
- 利用脚本定期在slave节点做RDB，实现数据备份
- 设置合理的rewrite阈值，避免频繁的bgrewrite
- 配置no-appendfsync-on-rewrite = yes，禁止在rewrite期间做aof，避免因AOF引起的阻塞



部署有关建议： 

- Redis实例的物理机要预留足够内存，应对fork和rewrite
- 单个Redis实例内存上限不要太大，例如4G或8G。可以加快fork的速度、减少主从同步、数据迁移压力
- 不要与CPU密集型应用部署在一起（es）
- 不要与高硬盘负载应用一起部署。例如：数据库、消息队列
