---
title: 一文教会你什么是MySQL的索引提示
date: 2023-08-02 14:16:51
tags: MySQL
---

# 一文教会你什么是MySQL的索引提示

MySql支持索引提示（INDEX HINT）显式地告诉优化器使用那个索引。

也就是比如profession字段有一个联合索引和一个单列索引，你可以选择这条sql是走联合索引还是单列索引



举例来描述：

 A. 执行SQL : explain select * from tb_user where profession = '软件工程';  

![img](../pic/%E4%B8%80%E6%96%87%E6%95%99%E4%BC%9A%E4%BD%A0%E4%BB%80%E4%B9%88%E6%98%AFMySQL%E7%9A%84%E7%B4%A2%E5%BC%95%E6%8F%90%E7%A4%BA/1690955112030-740d5a7c-06ec-4b3b-8475-58545de5cee5.png)

 查询走了联合索引。  

 B. 执行SQL，创建profession的单列索引

 C. 创建单列索引后，再次执行A中的SQL语句，查看执行计划，看看到底走哪个索引。  ![img](../pic/%E4%B8%80%E6%96%87%E6%95%99%E4%BC%9A%E4%BD%A0%E4%BB%80%E4%B9%88%E6%98%AFMySQL%E7%9A%84%E7%B4%A2%E5%BC%95%E6%8F%90%E7%A4%BA/1690955175236-c2666a71-120b-4bb8-a34e-d81336d60814.png)

 测试结果，我们可以看到，possible_keys中 idx_user_pro_age_sta,idx_user_pro 这两个 索引都可能用到，最终MySQL选择了idx_user_pro_age_sta索引。这是MySQL自动选择的结果。  



但是我们就是想让他走给他建的单列索引，那怎么办呢，那就用到了索引/SQL提示

 SQL提示，是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。  

1.  use index ： 建议MySQL使用哪一个索引完成此次查询（仅仅是建议，mysql内部还会再次进行评估）。  `explain select * from tb_user **use** index(idx_user_pro) where profession = '软件工程';  `
2.  ignore index ： 忽略指定的索引  ` explain select * from tb_user **ignore** index(idx_user_pro) where profession = '软件工程';  `
3.  force index ： 强制使用索引。  ` explain select * from tb_user **force** index(idx_user_pro) where profession = '软件工程';  `

