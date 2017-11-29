---
title: HostKey值改变后导致SSH连接错误
date: 2017-11-29 17:41:44
tags:
---
**问题描述**
公司 Gitlab 地址变更后,更改 MacOS 下 hosts 的值,完成后 pull 项目时发现报错:
```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@       WARNING: POSSIBLE DNS SPOOFING DETECTED!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
The RSA host key for zhongmakj has changed,
and the key for the corresponding IP address 47.97.22.120
is unknown. This could either mean that
DNS SPOOFING is happening or the IP address for the host
and its host key have changed at the same time.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:B4rTpw5bAXW+0QAq0IhAGZ/aCQKarOM0p5wEn8kKQCA.
Please contact your system administrator.
Add correct host key in /Users/zealotpz/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/zealotpz/.ssh/known_hosts:29
RSA host key for zhongmakj has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

查阅后得知,ssh会把你每个你访问过计算机的公钥(public key)都记录在`~/.ssh/known_hosts`,当下次访问相同计算机时，OpenSSH会核对公钥。如果公钥不同，OpenSSH会发出警告， 避免你受到DNS Hijack之类的攻击。

**有以下两个解决方案：**
1. 手动删除修改`known_hsots`里面旧的HostKey值记录，下次重新保存新的就行。
2. 修改配置文件`~/.ssh/config`，加上这两行，重启服务器。
```bash
StrictHostKeyChecking no
UserKnownHostsFile /dev/null
```

**优缺点：**
1. 需要每次手动删除文件内容，一些自动化脚本的无法运行（在SSH登陆时失败），但是安全性高；
2. SSH登陆时会忽略`known_hsots`的访问，但是安全性低；
