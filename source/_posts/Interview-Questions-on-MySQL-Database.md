---
title: Interview Questions on MySQL Database
date: 2024-08-04 22:12:36
cover_image: /image/cover006.png
cover_image_alt:
tags:
    - Interview
    - Database
    - MySQL
categories:
    - Code
---
# MySQL的事务(Transaction)
- 事务是数据库并发控制的基本单位
- 事务可以看作是一系列SQL语句的集合
- 事务必须全部执行成功，或者全部执行失败

## 事务的四个基本特性ACID 
- 原子性（Atomicity）：一个事务中所有的操作全部完成或失败
- 一致性（Consistency）：事务开始和结束之后的数据完整性没有被破坏
- 隔离性（Isolation）：允许多个事务同时对数据库进行修改和读写
- 持久性（Durability）：事务结束之后，修改是永久的不会丢失

## 事务并发控制

- 如果不进行并发控制，可能会产生的现象
    - 幻读（Phantom Read）：一个事务第二次查出现了第一次没有的结果
    - 非重复读（Nonrepeatable Read）：一个事务重复读两次得到不同的结果
    - 脏读（Dirty Read）：一个输入读取到另一个事务没有提交的修改
    - 修改丢失（Lost Update）：并发写入造成其中一些修改丢失
### 事务的隔离级别
1. 读未提交（Read Uncommitted）：别的事务可以读取到未提交的改变
2. 读已提交（Read Committed）：只能读取已提交的数据
3. 可重复读（Repeatable Read）：同一个事务先后查询结果一样
4. 串行化（Serializable）：事务完全串行化执行，隔离级别最高，执行效率最低

### 高并发场景下的插入重复
- 使用数据库唯一索引
- 使用队列异步写入
- 使用redis等实现分布式锁

### 乐观锁，悲观锁

- 悲观锁是先获取所再进行操作。一锁二查三更新（select for update）
- 乐观锁先修改，更新的时候发现数据变了就回滚（check and set）一般通过版本号或者时间戳来实现
- 根据响应速度，冲突频率，重试代价来判断使用哪一种锁

# MySQL常用的字段、含义和区别

# 常用数据库引擎的区别