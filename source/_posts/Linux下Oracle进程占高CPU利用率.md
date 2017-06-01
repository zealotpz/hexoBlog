---
title: Linux下Oracle进程占高CPU利用率
date: 2015-04-02 15:14:33
tags: oracle
---
最近公司服务器Oracle数据库经常会遇到CPU利用率很高的情况，而内存和I/O都不高,top命令查看如下:
![top](http://images.cnitblog.com/blog2015/688833/201504/021510194205644.png)

先查看数据库的警告日志ALERT文件，并没有发现有什么错误存在，日志显示数据库运行正常，排除数据库本身存在问题。
然后查看这些占用CPU资源很高的Oracle进程究竟是在做什么操作，使用如下SQL语句：

```sql
select sql_text,spid,v$session.program,process  from v$sqlarea,v$session,v$process
where v$sqlarea.address=v$session.sql_address
and v$sqlarea.hash_value=v$session.sql_hash_value
and v$session.paddr=v$process.addr
and v$process.spid in (PID);
```
<!--more-->
用top命令中占用CPU很高的进程的PID替换脚本中的PID,得到相应的Oracle进程所执行的SQL语句，发现占用CPU资源很高的进程都是执行同一个SQL语句：
```sql
--PID 31173
 INSERT INTO T_SBM_MT_TRANS_DAY_DTL (ID_, DATA_TYPE_, STA_COUNT_, GEN_DATE_, CREATE_ID_, CREATE_DATE_)    SELECT 021411280811,AREA_NAME_, COUNT(AREA_NAME_),TO_DATE('0214-11-28 13:40:00', 'YYYY-MM-DD HH24:MI:SS'),'0', SYSDATE    FROM (SELECT DECODE(C.AREA_NAME_, NULL, '未知区域', AREA_NAME_) AREA_NAME_              FROM T_SBM_BIKE_HIRE A              LEFT JOIN T_SBM_STATION B ON A.STATION_ID_ = B.ID_              LEFT JOIN T_SBM_AREA C ON B.AREA_ID_ = C.ID_               WHERE HIRE_DATE_ > TO_DATE('0214-11-28 13:30:00', 'YYYY-MM-DD HH24:MI:SS') AND               HIRE_DATE_ < TO_DATE('0214-11-28 13:40:00', 'YYYY-MM-DD HH24:MI:SS')) A               GROUP BY AREA_NAME_  HAVING COUNT(AREA_NAME_) > 0      
 --PID 11341
INSERT INTO T_SBM_MT_TRANS_DAY_DTL (ID_, DATA_TYPE_, STA_COUNT_, GEN_DATE_, CREATE_ID_, CREATE_DATE_)    SELECT 021501181421,AREA_NAME_, COUNT(AREA_NAME_),TO_DATE('0215-01-18 23:50:00', 'YYYY-MM-DD HH24:MI:SS'),'0', SYSDATE    FROM (SELECT DECODE(C.AREA_NAME_, NULL, '未知区域', AREA_NAME_) AREA_NAME_              FROM T_SBM_BIKE_HIRE A              LEFT JOIN T_SBM_STATION B ON A.STATION_ID_ = B.ID_              LEFT JOIN T_SBM_AREA C ON B.AREA_ID_ = C.ID_               WHERE HIRE_DATE_ > TO_DATE('0215-01-18 23:40:00', 'YYYY-MM-DD HH24:MI:SS') AND               HIRE_DATE_ < TO_DATE('0215-01-18 23:50:00', 'YYYY-MM-DD HH24:MI:SS')) A               GROUP BY AREA_NAME_  HAVING COUNT(AREA_NAME_) > 0   
--PID 11342
INSERT INTO T_SBM_MT_TRANS_DAY_DTL (ID_, DATA_TYPE_, STA_COUNT_, GEN_DATE_, CREATE_ID_, CREATE_DATE_)    SELECT 021502090331,AREA_NAME_, COUNT(AREA_NAME_),TO_DATE('0215-02-09 05:40:00', 'YYYY-MM-DD HH24:MI:SS'),'0', SYSDATE    FROM (SELECT DECODE(C.AREA_NAME_, NULL, '未知区域', AREA_NAME_) AREA_NAME_              FROM T_SBM_BIKE_HIRE A              LEFT JOIN T_SBM_STATION B ON A.STATION_ID_ = B.ID_              LEFT JOIN T_SBM_AREA C ON B.AREA_ID_ = C.ID_               WHERE HIRE_DATE_ > TO_DATE('0215-02-09 05:30:00', 'YYYY-MM-DD HH24:MI:SS') AND               HIRE_DATE_ < TO_DATE('0215-02-09 05:40:00', 'YYYY-MM-DD HH24:MI:SS')) A               GROUP BY AREA_NAME_  HAVING COUNT(AREA_NAME_) > 0
```
上述 SQl 中时间明显不合法(`0215年`)：
```sql
TO_DATE('0215-02-09 05:30:00', 'YYYY-MM-DD HH24:MI:SS')
```
从而导致后台系统的执行该 SQL语句时时间错误，从0215年统计到2015年的数据
