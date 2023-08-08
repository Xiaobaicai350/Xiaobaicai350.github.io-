---
title: SQL优化最干货总结
date: 2023-08-04 15:03:16
tags: MySQL
---

# SQL优化最干货总结

## 插入数据

### insert

如果我们需要一次性往数据库表中插入**多条记录**，可以从以下三个方面进行优化。  

1. 进行批量插入数据 `  Insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');  `
2. 手动控制事务

```plsql
start transaction;
insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
insert into tb_test values(4,'Tom'),(5,'Cat'),(6,'Jerry');
insert into tb_test values(7,'Tom'),(8,'Cat'),(9,'Jerry');
commit;
```

1.  主键顺序插入，性能要高于乱序插入  

```plsql
主键乱序插入 : 8 1 9 21 88 2 4 15 89 5 7 3
主键顺序插入 : 1 2 3 4 5 7 8 9 15 21 88 89
```

###  大批量插入数据  

如果一次性需要插入大批量数据(比如: 几百万的记录)，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令进行插入。操作如下：  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1690958309817-20d48651-64e6-4837-9ca9-cedca8431f79.png)

 可以执行如下指令，将数据脚本文件中的数据加载到表结构中：  

```plsql
-- 客户端连接服务端时，加上参数 -–local-infile
mysql –-local-infile -u root -p
-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;
-- 执行load指令将准备好的数据，加载到表结构中
load data local infile '/root/sql1.log' into table tb_user fields terminated by ',' lines terminated by '\n' ;
```

 主键**顺序（文件里面就是有序的）插入**性能高于乱序插入  

那么为什么主键顺序插入的性能比乱序插入的高呢？其实是主键优化的原因

## 主键优化

 在上一小节，我们提到，主键顺序插入的性能是要高于乱序插入的。 这一小节，就来介绍一下具体的原因，然后再分析一下主键又该如何设计。 

### 1). 数据组织方式

在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表 (index organized table IOT)。  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1690958634292-d29aa880-857e-48d3-83e6-92c98cce80d1.png)

 行数据，都是存储在聚集索引的叶子节点上的。而我们之前也讲解过InnoDB的逻辑结构图：  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1690958699125-ba1762cb-9f96-4340-b775-9cdcce17f135.png)

在InnoDB引擎中，数据行是记录在逻辑结构 page 页中的，而每一个页的大小是固定的，默认16K。那也就意味着， 一个页中所存储的行也是有限的，如果插入的数据行row在该页存储不小，将会存储到下一个页中，页与页之间会通过指针连接。  

###  2). 页分裂  

 页可以为空，也可以填充一半，也可以填充100%。每个**页**包含了2到N**行**数据(如果一行数据过大，会行溢出)，根据主键排列。  

 **A. 主键顺序插入效果** 

①. 从磁盘中申请页， 主键顺序插入  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051637015-b693e976-d0d7-4b90-b652-31337f52c650.png)

 ②. 第一个页没有满，继续往第一页插入  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051647953-968c758c-f09b-4f74-bb57-51a2578fadf0.png)

 ③. 当第一个也写满之后，再写入第二个页，页与页之间会通过指针连接  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051658101-27773937-4a4d-4d52-ad88-c9eca43b57ad.png)

 ④. 当第二页写满了，再往第三页写入  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051669094-01ef9a56-8bb0-45d4-b095-ab9700bd53a9.png)

 **B. 主键乱序插入效果** 

①. 假如第一二页都写满了，并且存放了如图所示的数据  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051682464-dbf769b4-964e-495a-a157-8c700ba8af96.png)

 ②. 此时再插入id为50的记录，我们来看看会发生什么现象

会再次开启一个页，写入新的页中吗？  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051702471-25eb030f-13fa-47ab-b278-aa208c97c134.png)

 不会。因为，索引结构的叶子节点**是有顺序的**。按照顺序，应该存储在47之后。  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051712583-196843c5-790a-48d0-8c10-b338abf64e32.png)

 但是47所在的1#页，已经写满了，存储不了50对应的数据了。 那么此时会开辟一个新的页 3#。  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051724189-35fe6466-cd57-4bff-85bc-b38fc59b082f.png)

 但是并不会直接将50存入第三页，而是会将第一页后一半的数据，移动到第三页，然后在第三页，插入50。  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051734624-ab27b937-4931-4d33-8ad6-564c2d0a2e91.png)

 移动数据，并插入id为50的数据之后，那么此时，这三个页之间的数据顺序是有问题的。 第一页的下一个页，应该是第三页， 第三页的下一个页是第二页。 **所以，此时，需要重新设置链表指针**。  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051756233-94b040ad-32b8-4286-a11b-f90d1b8f40fe.png)

 上述的这种现象，称之为 "页分裂"，是比较耗费性能的操作。  

其实这个50刚好有些特殊了，这个数字刚好在第一页和第二页之间可以插入了。但是页分裂同时也会发生在其他情况，也会导致性能变差。

### 3). 页合并

目前表中已有数据的索引结构(叶子节点)如下：

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051801757-eb69b5a3-529e-4872-8244-624cde993062.png)

当我们对已有数据进行删除时，具体的效果如下:

当删除一行记录时，实际上记录并没有被物理删除，只是记录被**标记（flaged）为删除**并且**它的空间变得允许被其他记录使用，其实就是可以被覆盖了**。

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051815392-be1c6177-7ae1-4496-8ca2-4391b834ac2c.png)

当我们继续删除2#的数据记录

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051821217-5be680f0-74a7-463d-acc8-3325819bcc09.png)

当页中删除的记录达到 MERGE_THRESHOLD（默认为页的50%），InnoDB会开始寻找最靠近的页（前或后）看看是否可以将两个页合并以优化空间使用。

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051832792-d74f75fd-bd84-4715-9791-38c00d3fbd7e.png)

删除数据，并将页合并之后，再次插入新的数据20，则直接插入第三页

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691051839898-b467f7ad-91a6-40e8-be39-08dafc02496d.png)

这个里面所发生的合并页的这个现象，就称之为 "页合并"。

 知识小贴士： MERGE_THRESHOLD：合并页的阈值，可以自己设置，在创建表或者创建索引时指定。  

### 4). 索引设计原则

- 满足业务需求的情况下，尽量降低主键的长度。
- 插入数据时，尽量选择顺序插入，选择使用AUTO_INCREMENT自增主键。
- 尽量不要使用UUID做主键或者是其他自然主键，如身份证号。
- 业务操作时，避免对主键的修改。

## order by优化

MySQL的排序，有两种方式：

1. Using filesort : 通过表的索引或全表扫描，读取满足条件的数据行，然后在**排序缓冲区**sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序。
2. Using index : 通过**有序索引**顺序扫描直接返回有序数据，这种情况即为 using index，不需要额外排序，操作效率高。

对于以上的两种排序方式，Using index的性能高，而Using filesort的性能低，我们**在优化排序操作时，尽量要优化为 Using index**。

接下来，我们来做一个测试：



explain select id,age,phone from tb_user order by age ;

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691053722981-ddcc4142-13bf-4bb4-ab27-b894c4b2ff61.png)

 explain select id,age,phone from tb_user order by age, phone ;  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691053732513-3b9158b9-d3ec-43b3-8fc5-5182671ddffc.png)

 由于 age, phone 都没有索引，所以此时再排序时，出现Using filesort， 排序性能较低。  



所以我们可以根据age和phone建立联合索引

 create index idx_user_age_phone_aa on tb_user(age,phone);  



 创建索引后，根据age, phone进行**升序（有升序降序之分）**排序

explain select id,age,phone from tb_user **order by age**  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691053836651-6e6d4589-d756-4405-ba75-1212355f3ac6.png)

   explain select id,age,phone from tb_user order by age , phone;  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691053916631-50772003-7c25-4cac-856e-34856f8b568b.png)

建立索引之后，再次进行排序查询，就由原来的Using filesort， 变为了 Using index，走了索引，性能就提升了

  

 根据age, phone进行**降序**排序  

 explain select id,age,phone from tb_user **order by age** **desc** **, phone** **desc** ;  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691053974695-98e8a552-486a-4f56-8a38-fd00c5d578e4.png)

也出现 Using index， **但是此时Extra中出现了 Backward index scan**，这个代表**反向扫描索引**，因为在MySQL中我们创建的索引，默认索引的叶子节点是从小到大排序的，而此时我们查询排序时，是从大到小，所以，在扫描时，就是反向扫描，就会出现 Backward index scan。 在 MySQL8版本中，支持降序索引，其实这种情况我感觉是没有性能差别的。但是如果我们有两个字段，第一个字段升序排列第二个字段降序排列，我们就需要创建降序索引了。  



 根据phone，age进行升序排序，phone在前，age在后  

 explain select id,age,phone from tb_user order by phone , age  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691055359265-a537942e-51ce-4a86-8f34-6ad48795ff57.png)

排序时,也需要满足最左前缀法则,否则也会出现 filesort。因为在创建索引的时候， age是第一个字段，phone是第二个字段，所以排序时，也就该按照这个顺序来，否则就会出现 Using filesort。  



 根据age, phone进行降序一个升序，一个降序  

 explain select id,age,phone from tb_user order by age asc , phone desc ;  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691055399885-64506049-3ff3-43a1-a805-e06626741e25.png)

因为创建索引时，如果未指定顺序，**默认都是按照升序排序**的，而查询时，一个升序，一个降序，此时就会出现Using filesort。  （其实这个可以这样想，如果得出了结果，但是这两个刚开始进行创建索引的时候都是升序，所以第一个走索引，第二个就得filesort了）

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691055514312-70c6026e-d542-4c48-90bc-a0868b4738d5.png)

 

为了解决上述的问题，我们可以创建一个索引，这个联合索引中 **age 升序排序**，**phone 倒序排序**。 

创建联合索引(age 升序排序，phone 倒序排序)  

 create index idx_user_age_phone_ad on tb_user(age asc ,phone desc);  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691055562913-2de65eec-dc7b-45c7-8946-0197874ec13e.png)

 然后再次执行如下SQL  

 explain select id,age,phone from tb_user order by age asc , phone desc ;  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691055599397-69414124-43c3-4ee6-a5b5-319d4564a778.png)

由上述的测试,我们得出order by优化原则:

A. 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则。

B. 尽量使用覆盖索引。

C. 多字段排序, 一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）。

D. 如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size(默认256k)。

##  group by优化  

 在没有任何索引（只有主键索引，没有下面的profession索引）的情况下，执行如下SQL，查询执行计划  ：

 explain select profession , count(*) from tb_user group by profession ;  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691055849762-55a49b81-e751-45b2-a81d-c5fd300a758b.png)

 然后，我们在针对于 profession ， age， status 创建一个联合索引。  

 create index idx_user_pro_age_sta on tb_user(profession , age , status);  

 紧接着，再执行前面相同的SQL查看执行计划。  

 explain select profession , count(*) from tb_user group by profession ;  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691055903005-cec38303-acd9-4125-b50b-f07b341fbbc1.png)

 再执行如下的分组查询SQL，查看执行计划：  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691055949905-93b4bde8-22f9-44d6-b41d-1d18ebbdea1c.png)

我们发现，如果仅仅根据age分组，就会出现 Using temporary(出现这个说明没走索引) ；而如果是根据profession,age两个字段同时分组，则不会出现 Using temporary。原因是因为：对于分组操作，也是需要符合最左前缀法则的。

所以，在分组操作中，我们需要通过以下两点进行优化，以提升性能：

1. 在分组操作时，可以通过索引来提高效率。
2. 分组操作时，索引的使用需要满足最左前缀法则。

##  limit优化  

 在数据量比较大时，如果进行limit分页查询，在查询时，越往后，分页查询效率越低  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691056120613-669d8b93-42c1-4a17-9e0c-577df39ad69a.png)

通过测试我们会看到，越往后，分页查询效率越低，这就是分页查询的问题所在。

因为，当在进行分页查询时，如果执行 limit 2000000,10 ，此时需要MySQL排序前2000010条记录，仅仅返回 2000000 - 2000010 的记录，其他记录则被丢弃，查询排序的代价非常大 。

优化思路: 一般分页查询时，通过创建覆盖索引 能够比较好地提高性能，可以通过覆盖索引加子查询形式进行优化。

 explain select * from tb_sku t , **(select id from tb_sku  limit 2000000,10) a** where t.id = a.id;  

上面这段sql代码的具体思想是：先根据主键索引查询出所有的**2000000-2000010的id**  不需要返回具体的数据，然后再根据这些id去拿取这十个id的数据，这种的好处就是减少了之前拿取**2000010条行的数据，**减少到了只拿取**10行**数据

##  count优化  

 在之前的测试中，我们发现，如果数据量很大，在执行count操作时，是非常耗时的。  

下面来看看不同的存储引擎怎么进行count操作的：

1. MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 count(*) 的时候会直接返回这个数（前提是没有where这种的过滤条件），效率很高； 但是如果是带条件的count，MyISAM也慢。 
2. InnoDB 引擎就麻烦了，它执行 count(*) 的时候，需要把数据一行一行地从引擎里面读出来，然后累积计数。  

 如果说要大幅度提升InnoDB表的count效率，主要的优化思路：**自己计数**



**首先来看下count的不同用法：**

count() 是一个聚合函数，对于返回的结果集，一行行地判断，如果 count 函数的参数不是 NULL，累计值就加 1，否则不加，最后返回累计值。 

用法：count（*）、count（主键）、count（字段）、count（数字）  

![img](../pic/SQL%E4%BC%98%E5%8C%96%E6%9C%80%E5%B9%B2%E8%B4%A7%E6%80%BB%E7%BB%93/1691056845059-941047c4-0d9b-41a7-a56f-44270eb84398.png)

 按照效率排序的话，count(字段) < count(主键 id) < count(1) ≈ count(*)，所以尽量使用 count(*)，效率还是比其他的高的，但是要是有具体业务需要统计非空的，就要具体分析了。  

##  update优化  

 update course set name = 'javaEE' where id = 1 ;  

 当我们在执行删除的SQL语句时，会锁定id为1这一行的数据，然后事务提交之后，行锁释放。  



 InnoDB的**行锁**是针对**索引**加的锁，并且该索引不能失效，否则会从行锁升级为表锁 。  

 但是当我们在执行如下SQL时。  

 update course set name = 'SpringBoot' where name = 'PHP' ;  



当我们开启多个事务，在执行上述的SQL时，我们发现行锁升级为了表锁（导致的后果是其他事务都执行不了sql，因为上面这个sql把整张表都锁住了）。 导致该update语句的性能大大降低。

为了提升性能，可以给name字段加上索引，这样的话在事务期间执行操作的话就是行锁了  
