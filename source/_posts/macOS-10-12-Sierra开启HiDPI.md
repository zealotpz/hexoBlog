---
title: macOS 10.12 Sierra开启HiDPI
date: 2016-12-23 21:42:10
tags: macOS 10.12,Sierra,HiDPI,SwitchResX
---
MacBook Pro 升级macOS 10.12 Sierra系统后外接2K 显示器无法开启HiDPI,
之前在Mac OS X 10.11 El Capitan下的几个方法均失效:
* 使用`SwitchResX`
按照 知乎: https://www.zhihu.com/question/35300978/answer/68752378 回答操作, 关闭SIP后选择
1920*1080分辨率即可
* 使用`RDM`
按照 Github: https://github.com/syscl/Enable-HiDPI-OSX 的流程,执行`enable-HiDPI.sh`后用 RDM选择1920*1080分辨率即可
<!-- more -->
后根据 https://www.zhihu.com/question/35300978/answer/126332986 成功开启

** 流程如下: **
1.如已安装,卸载SwitchResX,并在`系统偏好设置`内移除设置面板
2.重启电脑,开机按`CMD+R`进入恢复模式,进入终端后输入`csrutil disable`关闭SIP
3.官网 http://www.madrau.com/ 下载 最新版`SwitchResX4.zip`,选择`为电脑上所有用户安装!`
4.新建一个管理员角色,如 test
5.注销当前用户,用 test 用户登录,用10.11 El Capitan下的方法新建`3840*2160`分辨率即可,后续操作相同
6.删除 test 用户,开启 SIP

*** `注意事项`: ***
- 请尽量使用 `DP线`.
- 如SwitchResX 中的`Custom Resolutions`界面`+`号为灰色,请检查`SIP`是否成功关闭.
- `Scaled resolution` 中设置的分辨率应为 HiPDI 的`2`倍,如1920x1080(HiDPI)设置为3840x2160
- 操作正常,但重启后仍显示`not installed`,请检查SwitchResX是否付费.
