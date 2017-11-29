---
title: CentOS 7 修改时区
date: 2017-07-19 14:51:00
tags: CentOS, 时区
---

#### 基本概念

** GMT、UTC、CST、DST 时间 **

(1) UTC
整个地球分为二十四时区，每个时区都有自己的本地时间。在国际无线电通信场合，为了统一起见，使用一个统一的时间，称为通用协调时(UTC, Universal Time Coordinated)。

(2) GMT
格林威治标准时间 (Greenwich Mean Time)指位于英国伦敦郊区的皇家格林尼治天文台的标准时间，因为本初子午线被定义在通过那里的经线。(UTC与GMT时间基本相同，本文中不做区分)

(3) CST
中国标准时间 (China Standard Time)
1
GMT + 8 = UTC + 8 = CST

(4) DST
夏令时(Daylight Saving Time) 指在夏天太阳升起的比较早时，将时钟拨快一小时，以提早日光的使用。（中国不使用）

1.2 硬件时钟和系统时钟

(1) 硬件时钟
RTC(Real-Time Clock)或CMOS时钟，一般在主板上靠电池供电，服务器断电后也会继续运行。仅保存日期时间数值，无法保存时区和夏令时设置。

(2) 系统时钟
一般在服务器启动时复制RTC时间，之后独立运行，保存了时间、时区和夏令时设置。

在 CentOS 7 中, 引入了一个叫 timedatectl 的设置设置程序.

```bash
dev@localhost ~/ » timedatectl
      Local time: Wed 2017-07-19 14:55:27 CST
  Universal time: Wed 2017-07-19 06:55:27 UTC
        RTC time: Wed 2017-07-19 14:55:28
       Time zone: Asia/Shanghai (CST, +0800)
     NTP enabled: no
NTP synchronized: no
 RTC in local TZ: yes
      DST active: n/a
```

```bash
timedatectl list-timezones # 列出所有时区
timedatectl set-local-rtc 1 # 将硬件时钟调整为与本地时钟一致, 0 为设置为 UTC 时间
timedatectl set-timezone Asia/Shanghai # 设置系统时区为上海
```

其实不考虑各个发行版的差异化, 从更底层出发的话, 修改时间时区比想象中要简单:
```bash
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
