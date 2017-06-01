---
title: IDEA 远程调试(Tomcat 与 Springboot)
date: 2017-05-25 11:11:28
tags: IDEA,debug
---

#### 使用Tomcat项目
** 一、IDEA 配置 **
1. 添加远程 `Tomcat Server`,选择 `Remote`
![](http://oizl4tpho.bkt.clouddn.com//20170525140622_YPKDWi_tomcat1.jpeg)
<!--more-->
2. 这里的`Remote staging`选择的都是`same file system`，这就要求本地代码和远程Tomcat的代码要一致;
![](http://oizl4tpho.bkt.clouddn.com//20170525140635_fbLnY4_tomcat2.jpeg)

3. `9991` 这个端口为设置的远程Debug端口
![](http://oizl4tpho.bkt.clouddn.com//20170525141215_IkdUGZ_tomcat3.jpeg)

** 二、远程 Tomcat 配置 **
1. 在tomcat/bin下的`catalina.sh`上边添加下边的一段设置(即`IDEA 配置``图3中的配置)
```BASH
# -----------------------------------------------------------------------------

CATALINA_OPTS="-Xdebug -Xrunjdwp:transport=dt_socket,address=9991,suspend=n,server=y"
# OS specific support.  $var _must_ be set to either true or false.
```
2. 添加完成后重启 Tomcat 即可,使用 lsof 查看端口使用情况
```bash
lsof -i:9991
```
3. 启动成功后控制台输出如下:
![](http://oizl4tpho.bkt.clouddn.com//20170525142837_b4LISr_jar3.jpeg)

#### 使用 Spring Boot 项目
** 一、服务器端配置 **
1. jar文件启动,设置启动debug 端口为`9991`
```bash
nohup java -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=9991 -Dpay.notify.ip=123.11.11.11 -Deureka.hostname=123.22.22.22 -Deureka.instance.post=1111 -jar zealotpz-test-1.0.jar >> /dev/null &
```
** 二、IDEA 配置 **
1. 添加配置,选择 `Remote`
![](http://oizl4tpho.bkt.clouddn.com//20170525144203_CnTvgp_1111111111111.jpeg)

2. Remote 配置,填入远程 jar 启动的 ip 即端口即可,如图之前配置的9991
![](http://oizl4tpho.bkt.clouddn.com//20170525142353_2aWrAz_jar2.jpeg)

3. 启动成功后控制台输出如下:
![](http://oizl4tpho.bkt.clouddn.com//20170525142837_b4LISr_jar3.jpeg)
