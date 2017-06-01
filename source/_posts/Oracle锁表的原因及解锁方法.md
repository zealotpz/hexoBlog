---
title: Oracle锁表的原因及解锁方法
date: 2015-12-28 14:31:33
tags: Oracle
---
** 事情经过 **
整个自行车租赁系统突然挂掉，ssh 连上服务器后甚至 su 无法切换用户，登录SQL*Plus后修改用户最大进程数后可以登录，重启自行车系统后程序仍不能正常运行;
后发现为同事在 PL/SQL Developer 上编辑表数据(核心业务表)后未提交，系统运行一段时间和数据库就将表锁住。

** 锁表分析 **
使用PL/SQL Developer 编辑表数据或者是编写 update 语句时,执行(excute)这些 SQL语句后,未进行`提交`(`commit`)操作(绿色箭头按钮),从而导致的锁表.
`提交`或者`回滚事务`后系统正常

<!--more-->
** 解锁方法：**
1. 查看锁表进程:
```sql
select * from v$session t1, v$locked_object t2 where t1.sid = t2.SESSION_ID;
```
2. 将锁住的进程杀掉:
```sql
alter system kill session SID,serial#;
```
