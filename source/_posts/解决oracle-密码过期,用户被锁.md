---
title: 解决oracle 密码过期,用户被锁
date: 2015-08-05 16:13:18
tags: oracle
---
用PL/SQL Developer登录数据库时提示`密码过期`,原因是当时安装oracle时未设置密码过期时间,而oracle11g中默认在default概要文件中设置了`PASSWORD_LIFE_TIME=180天`所导致。
1. 查看用户的proifle是哪个，3个服务器均为默认的default：
```sql
SELECT username,PROFILE FROM dba_users;
```
2. 查看指定概要文件(如default)的密码有效期设置：
```sql
SELECT * FROM dba_profiles s WHERE s.profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME';
```
3. 将密码有效期由默认的180天修改成“无限制”：
```sql
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```
4. 如果弹出密码即将过期提示，则还需要再改一次密码(可为原密码):  
```bash
$sqlplus / as sysdba
sql> alter user SBM identified by SBM123;
```
<!-- more -->
#### 但当在云服务器上更改时发现无法用`sqlplus / as sysdba`登录，提示权限不够
1. 检查linux系统`oracle用户`的所属组的情况。
```bash
[oracle@xxx ~]$ id
uid=504(oracle) gid=504(oinstall) groups=504(oinstall),505(oracle)
```
2. 发现没有dba的用户组,root下执行
```bash
[root@xxx ~]$usermod -G oinstall,dba -g oinstall oracle
[root@xxx ~]$ id
uid=504(oracle) gid=504(oinstall) groups=504(oinstall),505(dba)
```
3. 加入`dba`组后可用`sqlplus / as sysdba`登录  

#### oracle用户被锁定后解锁办法,用户(card)
```bash
sqlplus sys/sys as sysdba
SQL> alter user card account unlock;
User altered.
SQL> commit;
Commit complete.
SQL> conn card
Enter password:
ERROR:
ORA-28001: the password has expired
Changing password for card
New password:
Retype new password:
Password changed
Connected.
```
