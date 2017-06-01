---
title: CentOS下shadowsocks一键安装脚本
date: 2017-01-05 10:24:30
tags: shadowsocks,ss,centOS
---

** 本脚本适用环境：**
系统支持：CentOS 32 或 64 位
内存要求：≥128M
日期：`2016.11.05`

** 关于本脚本：**
一键安装 `libev` 版的 shadowsocks 最新版本。该版本的特点是内存占用小（600k左右），低 CPU 消耗，甚至可以安装在基于 OpenWRT 的路由器上。
友情提示：如果你有问题，请先参考这篇 [Shadowsocks Troubleshootin](https://teddysun.com/399.html)后再问。

<!-- more -->
** 默认配置：**
服务器端口：自己设定（如不设定，默认为 `8989`）
客户端端口：`1080`
密码：自己设定（如不设定，默认为`teddysun.com`）

** 客户端下载: **
[Windowns -> shadowsocks-windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
[macOS -> ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG/releases/)

推荐使用[ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG)作为客户端,官方版本的Shadowsocks客户端的`GWF list`基本属于无人维护状态

** 使用方法：**
使用`root`用户登录，运行以下命令：
```bash
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh
chmod +x shadowsocks-libev.sh
./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log
```

如成功安装完成后，脚本提示如下：
```bash
Congratulations, Shadowsocks-libev install completed!
Your Server IP:your_server_ip
Your Server Port:your_server_port
Your Password:your_password
Your Local IP:127.0.0.1
Your Local Port:1080
Your Encryption Method:aes-256-cfb

Welcome to visit:https://teddysun.com/357.html
Enjoy it!
```
** 卸载方法：**
使用`root`用户登录，运行以下命令：
```bash
./shadowsocks-libev.sh uninstall
```

安装完成后即已后台启动 `Shadowsocks-libev` ，运行：
```bash
/etc/init.d/shadowsocks status
```
可以查看进程是否启动。
本脚本安装完成后，会将 Shadowsocks-libev 加入`开机自启动`

** 配置文件路径：** `/etc/shadowsocks.json`
```json
{
    "server":"0.0.0.0",
    "server_port":8989,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"yourpassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

** 使用命令 **
`启动`：/etc/init.d/shadowsocks start
`停止`：/etc/init.d/shadowsocks stop
`重启`：/etc/init.d/shadowsocks restart
`查看状态`：/etc/init.d/shadowsocks status



转自：https://teddysun.com/342.html
