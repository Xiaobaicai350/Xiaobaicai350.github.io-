---
title: MySql之四种SQL性能分析工具
date: 2023-08-02 14:16:34
tags: MySQL
---

# MySql之四种SQL性能分析工具

本篇我将为大家讲解SQL性能的分析工具，而只有熟练的掌握了性能分析的工具，才可以更好的对SQL语句进行优化。

## SQL执行频率

首先我们可以考虑的是：看看这个数据库增删改查的次数是多少，如果查询多，就根据查询多的sql建索引，如果修改添加删除多，就删除索引。所以首先我们得看看这个数据库增删改查的次数分别是多少。





MySQL 客户端连接成功后，通过 `show [session|global] status `命令可以提供服务器状态信息。

通过如下指令，可以查看当前数据库的INSERT、UPDATE、DELETE、SELECT的访问频次：  

```plsql
-- session 是查看当前会话 ;
-- global 是查询全局数据 ;
-- 其中的Com是固定的
SHOW GLOBAL STATUS LIKE 'Com_______';
```

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690940234133-2a60055d-9c98-4045-92bd-0cce583c58f9.png)



比如我们可以看到上面的查询比较多（一般都是查询多），那么如果我们想优化以下，让我们的mysql响应更快，我们咋知道哪个查询语句耗时长呢？所以就要用到MySQL中的慢查询日志了。

## 慢查询日志

慢查询日志记录了**所有执行时间超过指定参数**（long_query_time，单位：秒，默认10秒）的**所有 SQL语句**的日志。  

简单来说就是看哪一句SQL执行时间长的。



MySQL的慢查询日志默认没有开启，我们可以查看一下系统变量 `slow_query_log`。  

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690940588425-d25f5f0b-4f3f-4d5d-be20-ba9d38d82029.png)

 如果要开启慢查询日志，需要在MySQL的配置文件（/etc/my.cnf）中配置如下信息：  

```powershell
# 开启MySQL慢日志查询开关
slow_query_log=1
# 设置慢日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
long_query_time=2
```

 

配置完毕之后，通过以下指令重新启动MySQL服务器进行测试 `systemctl restart mysqld  `

查看慢日志文件中记录的信息 `/var/lib/mysql/localhost-slow.log`



 然后，再次查看开关情况，慢查询日志就已经打开了。  ![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690940745717-bcee2bbd-5da5-4384-b5b8-1c329cdaf349.png)





测试：

 A. 执行如下SQL语句 ：  

```plsql
select * from tb_user; -- 这条SQL执行效率比较高, 执行耗时 0.00sec
select count(*) from tb_sku; -- 由于tb_sku表中, 预先存入了1000w的记录, count一次,耗时13.35sec
```

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690940791696-15895b41-9a8c-4076-9010-272cf1bdf6a0.png)

 B. 检查慢查询日志 ：  

最终我们发现，在慢查询日志中，只会记录执行时间超多我们预设时间（2s）的SQL，执行较快的SQL是不会记录的。  

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690940858627-8b6bd16f-832e-46cb-ac57-97b20482baf2.png)



 这样，通过慢查询日志，就可以**定位出执行效率比较低的SQL**，从而有针对性的进行优化。  

## profile详情

其实这个东西用的比较少

他是看每一条sql的执行时间详细信息的，比如执行这一条sql哪个阶段花了多少时间啊什么的。



MySQL是否支持profile操作：  `SELECT @@have_profiling ;  `

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690941013722-af1a2162-cd49-4c25-bd13-4c2e6954792c.png)

可以看到，当前MySQL是支持 profile操作的，但是开关是关闭的。可以通过set语句在 session/global级别开启profiling：   `SET profiling = 1; `



开关已经打开了，接下来，我们所执行的SQL语句，都会被MySQL记录，并记录执行时间消耗到哪儿去 了。 我们直接执行如下的SQL语句：  

```plsql
select * from tb_user;
select * from tb_user where id = 1;
select * from tb_user where name = '白起';
select count(*) from tb_sku;
```

 执行一系列的业务SQL的操作，然后通过如下指令查看指令的执行耗时：  

```plsql
-- 查看每一条SQL的耗时基本情况
show profiles;
-- 查看指定query_id的SQL语句各个阶段的耗时情况
show profile for query query_id;
-- 查看指定query_id的SQL语句CPU的使用情况
show profile cpu for query query_id;
```

 查看每一条SQL的耗时情况:  

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690941257379-bfd41c68-77a2-4018-b2d6-2890fd358b15.png) 查看指定SQL各个阶段的耗时情况 :  

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690941271475-e1ef42e3-6be9-4f4e-8dff-da00f647e32a.png)

但是其实这个也只能看到每个阶段的执行时间，可以发现execute的时间长了，也没详细信息

还是得靠下面的这个，比较重要的东西explain

## explain

EXPLAIN 或者 DESC命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表如何连接和连接的顺序。  

但是其实对于查询语句，主要是看他走不走索引，走哪个索引，explain就可以看这些东西，所以他很重要



语法：

```
 EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件 ;  
```

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690941504430-557cd537-4d5a-4083-a5fd-783967b1a89d.png)

![img](../pic/MySql%E4%B9%8B%E5%9B%9B%E7%A7%8DSQL%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7/1690941519411-b9129eea-0cb1-4a7a-85b2-aeb0f8729e93.png)

这个其实对于sql是否走索引，除了根据经验分析，这种是根据实际走没走，更准确。
