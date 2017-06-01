---
title: Mysql查询并删除重复数据(多字段)
date: 2017-01-24 10:05:46
tags: mysql
---
一个表内两个字段应该加上unique约束,但由于表设计不周,未加唯一约束导致表中出现大量重复数据

#### 查询重复记录
根据`userName, userAge, sex`查询user表中 的重复记录,即姓名,年龄,性别均相同的用户数据
```sql
select userName, userAge, sex from user group by userName, userAge, sex having count(*) > 1
```
<!-- more -->

#### 删除重复记录
根据3个字段删除表中重复数据(`删除前请备份!`)
```sql
DELETE FROM user
WHERE (userName, userAge, sex) IN
      (SELECT userName, userAge, sex FROM
         (SELECT userName, userAge, sex FROM user
          GROUP BY userName, userAge, sex HAVING count(*) > 1) s1)
      AND id NOT IN
          (SELECT id FROM (SELECT id FROM user
                 GROUP BY userName, userAge, sex HAVING count(*) > 1) s2);
```
