---
title: xorm
date: 2023-04-21 20:45:28
tags: Go框架
---

官方文档：[XORM - Simple and Powerful ORM for Go](https://xorm.io/zh/)

XORM 是一个简单而强大的 Go 语言 ORM 框架。

## 特性

- 支持 Struct 和数据库表之间的灵活映射，并支持自动同步
- 事务支持
- 同时支持原始SQL语句和ORM操作的混合执行
- 使用连写来简化调用
- 支持使用ID, In, Where, Limit, Join, Having, Table, SQL, Cols等函数和结构体等方式作为条件
- 支持级联加载Struct
- Schema支持（仅Postgres）
- 支持缓存
- 通过 [xorm.io/reverse](https://xorm.io/reverse) 支持根据数据库自动生成 xorm 结构体
- 支持记录版本（即乐观锁）
- 通过 [xorm.io/builder](https://xorm.io/builder) 内置 SQL Builder 支持
- 上下文缓存支持
- 支持日志上下文

## 安装

```
go get xorm.io/xorm
```
