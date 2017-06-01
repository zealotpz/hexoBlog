---
title: Java double相乘后结果偏差
date: 2017-03-21 14:01:45
tags:
---

```java
public static void main(String[] args) {
    double amt = 4869.61;
    double amt2 = amt * 100;

    System.out.println(amt2);
}
```

结果不是预期的`486961`,而是`486960.99999999994`
<!-- more -->

** 解决方案一 : **
** ~~此方法有误 !!!~~ **
~~100 用 `*10*10` 替换,10000 用 `100*100`替换,~~
如
```java
double amt2 = amt * 100;
```
替换成
```java
double amt3 = amt * 10 * 10;
```

** 解决方案二 : **
```java
public static void main(String[] args) {
    double d =4 869.61;
    BigDecimal a1 = new BigDecimal(Double.toString(d));
    BigDecimal b1 = new BigDecimal(Double.toString(100));  
    BigDecimal result = a1.multiply(b1);// 相乘结果
    System.out.println(result);
    BigDecimal one = new BigDecimal("1");
    double a = result.divide(one,2,BigDecimal.ROUND_HALF_UP).doubleValue();//保留1位数
    System.out.println(a);
}
```
** 解决方案三 : **
```java
double amt3 = (float)amt * 100;
```

 ** double 转 String **
```java
DecimalFormat decimalFormat = new DecimalFormat("###################.###########");
decimalFormat.format(amt2);

System.out.println("------->amt*10*10:" + amt1); //------->amt*10*10:486961.0
System.out.println("------->amt*100:" + amt2); //------->amt*100:486960.99999999994
System.out.println("------->float:" + amt3); //float:486961.0
System.out.println("------->String:" + decimalFormat.format(amt1)); //486961
```
