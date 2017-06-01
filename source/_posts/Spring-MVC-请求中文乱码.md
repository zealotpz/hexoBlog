---
title: Spring MVC 请求中文乱码
date: 2016-11-30 11:38:25
tags: Spring MVC
---
最近在用 Spring MVC编写 restful 接口的时候发现,如果直接通过浏览器直接访问,后台不能正确的接受中文 url 参数,整理了如下几种方法:

#### 1.Tomcat 可以直接配置 `URIEconding="UTF-8"`
在`/conf/server.xml`中增加编码
```xml
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8"/>
```
#### 2.对参数进行转码
```java
new String("中文".getBytes("ISO-8859-1"), "UTF-8");
```
<!--more-->
```java
public static String encodeStr(String param) {
    try {
        return new String(param.getBytes("ISO-8859-1"), "UTF-8");
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
        return null;
    }
}
```

#### 3.将中文使用`URLEncoder`编码
如：[baidu.com/s?wd=%e7%a5%9e%e7%a7%98%e6%b5%b7%e5%9f%9f](http://baidu.com/s?wd=%e7%a5%9e%e7%a7%98%e6%b5%b7%e5%9f%9f)
