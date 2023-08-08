---
title: 盘点Mysql中常见的日志
date: 2023-08-04 15:03:41
tags: MySQL
---

# 盘点Mysql中常见的日志

## 错误日志

 错误日志是 MySQL 中最重要的日志之一，它记录了当 mysqld 启动和停止时，以及服务器在运行过 程中发生任何严重错误时的相关信息。当数据库出现任何故障导致无法正常使用时，建议首先查看此日志。 

该日志是默认开启的，默认存放目录 /var/log/，默认的日志文件名为 mysqld.log 。查看日志位置：  ` show variables like '%log_error%';  `

![img](../pic/%E7%9B%98%E7%82%B9Mysql%E4%B8%AD%E5%B8%B8%E8%A7%81%E7%9A%84%E6%97%A5%E5%BF%97/1691131966003-ca74bfd9-6ca9-4ab6-a908-95f6de64895d.png)

##  二进制日志 

**MySQL主从复制的核心就是二进制日志，因为他记录了所有的DDL和DML语句，有了这些语句我们就可以创建数据库表并且添加数据了** 

 二进制日志（BINLOG）记录了所有的 DDL（数据定义语言）语句和 DML（数据操纵语言）语句，但不包括数据查询（SELECT、SHOW）语句。

作用：

1. 灾难时的数据恢复
2. MySQL的主从复制

在MySQL8版本中，默认二进制日志是开启着的 `show variables like '%log_bin%';`

![img](../pic/%E7%9B%98%E7%82%B9Mysql%E4%B8%AD%E5%B8%B8%E8%A7%81%E7%9A%84%E6%97%A5%E5%BF%97/1691132078310-aebee9b5-ae36-432d-a4fd-fe61d6b00384.png)

![img](../pic/%E7%9B%98%E7%82%B9Mysql%E4%B8%AD%E5%B8%B8%E8%A7%81%E7%9A%84%E6%97%A5%E5%BF%97/1691132068271-dbfbae6f-dcc6-4766-8e6b-dc4118d795a1.png)



参数说明：

- log_bin_basename：当前数据库服务器的binlog日志的基础名称(前缀)，具体的binlog文件名需要再该basename的基础上加上编号(编号从000001开始)。
- log_bin_index：binlog的索引文件，里面记录了当前服务器关联的binlog文件有哪些。



 MySQL服务器中提供了多种格式来记录二进制日志，具体格式及特点如下：  ![img](../pic/%E7%9B%98%E7%82%B9Mysql%E4%B8%AD%E5%B8%B8%E8%A7%81%E7%9A%84%E6%97%A5%E5%BF%97/1691132147502-e4faa2b1-d44b-403e-ad2c-775ad41efda0.png)

```
 show variables like '%binlog_format%'  
```

![img](../pic/%E7%9B%98%E7%82%B9Mysql%E4%B8%AD%E5%B8%B8%E8%A7%81%E7%9A%84%E6%97%A5%E5%BF%97/1691132170374-8ec1fc05-d5e1-4ddf-a95f-27543e7c244f.png)

 如果我们需要配置二进制日志的格式，只需要在 /etc/my.cnf 中配置 binlog_format 参数即可。  





 由于日志是以二进制方式存储的，不能直接读取，需要通过二进制日志查询工具 mysqlbinlog 来查 看，具体语法：  

```powershell
mysqlbinlog [ 参数选项 ] logfilename
参数选项：
-d 指定数据库名称，只列出指定的数据库相关操作。
-o 忽略掉日志中的前n行命令。
-v 将行事件(数据变更)重构为SQL语句
-vv 将行事件(数据变更)重构为SQL语句，并输出注释信息
```

 



 对于比较繁忙的业务系统，每天生成的binlog数据巨大，如果长时间不清除，将会占用大量磁盘空 间。可以通过以下几种方式清理日志：  ![img](../pic/%E7%9B%98%E7%82%B9Mysql%E4%B8%AD%E5%B8%B8%E8%A7%81%E7%9A%84%E6%97%A5%E5%BF%97/1691132247982-dbd4e19f-03b2-4492-ba1f-73f4a82ded75.png)

除了这种方式外， 也可以在mysql的配置文件中配置二进制日志的过期时间，设置了之后，二进制日志过期会自动删除。  ` show variables like '%binlog_expire_logs_seconds%'  `

##  查询日志  

 查询日志中记录了客户端的所有操作语句，而**二进制日志不包含查询数据的SQL**语句。默认情况下， 查询日志是未开启的。  

![img](../pic/%E7%9B%98%E7%82%B9Mysql%E4%B8%AD%E5%B8%B8%E8%A7%81%E7%9A%84%E6%97%A5%E5%BF%97/1691132297827-4ac00151-6d05-41bd-836c-1f9dbc8f8f7a.png)

 如果需要开启查询日志，可以修改MySQL的配置文件 /etc/my.cnf 文件，添加如下内容：  

```powershell
#该选项用来开启查询日志 ， 可选值 ： 0 或者 1 ； 0 代表关闭， 1 代表开启
general_log=1
#设置日志的文件名 ， 如果没有指定， 默认的文件名为 host_name.log
general_log_file=mysql_query.log
```

开启了查询日志之后，在MySQL的数据存放目录，也就是 /var/lib/mysql/ 目录下就会出现 mysql_query.log 文件。之后所有的客户端的**增删改查**操作都会记录在该日志文件之中，长时间运行后，该日志文件将会非常大。  

##  慢查询日志  

 慢查询日志记录了所有执行时间超过参数 long_query_time 设置值并且扫描记录数不小于 min_examined_row_limit 的所有的SQL语句的日志，默认未开启。long_query_time 默认为 10 秒，最小为 0， 精度可以到微秒。 

如果需要开启慢查询日志，需要在MySQL的配置文件 /etc/my.cnf 中配置如下参数：  

```powershell
#慢查询日志
slow_query_log=1
#执行时间参数
long_query_time=2
```

默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询。可以使用 log_slow_admin_statements和 更改此行为 log_queries_not_using_indexes，如下所述。  

```powershell
#记录执行较慢的管理语句
log_slow_admin_statements =1
#记录执行较慢的未使用索引的语句
log_queries_not_using_indexes = 1
```
