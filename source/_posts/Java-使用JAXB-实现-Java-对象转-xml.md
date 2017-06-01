---
title: Java 使用JAXB 实现 Java 对象转 xml
date: 2017-02-17 09:51:37
tags: Java,xml,JAXB
---
** JDK中JAXB相关的重要Class和Interface：**

1. JAXBContext类，是应用的入口，用于管理XML/Java绑定信息。

2. `Marshaller`接口，将Java对象序列化为XML数据。

3. Unmarshaller接口，将XML数据反序列化为Java对象。

** JDK中JAXB相关的重要Annotation：**

1. `@XmlType`，将Java类或枚举类型映射到XML模式类型

<!-- more -->
2. @XmlAccessorType(XmlAccessType.FIELD) ，控制字段或属性的序列化。FIELD表示JAXB将自动绑定Java类中的每个非静态的（static）、非瞬态的（由@XmlTransient标注）字段到XML。其他值还有XmlAccessType.PROPERTY和XmlAccessType.NONE。

3. @XmlAccessorOrder，控制JAXB 绑定类中属性和字段的排序。

4. @XmlJavaTypeAdapter，使用定制的适配器（即扩展抽象类XmlAdapter并覆盖marshal()和unmarshal()方法），以序列化Java类为XML。

5. @XmlElementWrapper ，对于数组或集合（即包含多个元素的成员变量），生成一个包装该数组或集合的XML元素（称为包装器）。

6. `@XmlRootElement`，将Java类或枚举类型映射到XML元素。

7. `@XmlElement`，将Java类的一个属性映射到与属性同名的一个XML元素。

8. @XmlAttribute，将Java类的一个属性映射到与属性同名的一个XML属性。


Java Bean, 通过注解将Java类或枚举类型映射到XML模式类型
```java
import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;
import javax.xml.bind.annotation.XmlType;

@XmlRootElement(name = "envelope") //指定元素名称
@XmlType(propOrder = {"agentHead","agentBody"}) //指定生成元素顺序
public class Envelope {

    public AgentHead getAgentHead() {
        return agentHead;
    }

    @XmlElement(name = "head") //指定子节点元素名称
    public void setAgentHead(AgentHead agentHead) {
        this.agentHead = agentHead;
    }

    public AgentBody getAgentBody() {
        return agentBody;
    }

    @XmlElement(name = "body") //指定子节点元素名称
    public void setAgentBody(AgentBody agentBody) {
        this.agentBody = agentBody;
    }

    private AgentHead agentHead;
    private AgentBody agentBody;

}
```

Java 对象转 xml 方法,`Marshaller`实例可设置输出 xml 属性
```java
    /**
     * Java 对象转 xml
     * @param object
     * @return
     */
public static String object2Xml(Object object) {
    try {
        StringWriter writer = new StringWriter();
        JAXBContext context = JAXBContext.newInstance(object.getClass());
        Marshaller marshal = context.createMarshaller();

        marshal.setProperty(Marsha实例ller.JAXB_FORMATTED_OUTPUT, false); // 是否格式化输出(即按标签自动换行，false是一行的xml)
        marshal.setProperty(Marshaller.JAXB_ENCODING, "UTF-8");// 编码格式,默认为utf-8
        marshal.setProperty(Marshaller.JAXB_FRAGMENT, true);// 是否省略xml头信息
        //<?xmlversion="1.0"encoding="utf-8"standalone="yes"?>
        marshal.setProperty("jaxb.encoding", "utf-8"); //设置编码格式 utf-8
        marshal.marshal(object, writer);

        return new String(writer.getBuffer());
    } catch (Exception e) {
        e.printStackTrace();
        return null;
    }
}
```
按指定格式拼装并生成 xml
```java
      //envelope
       Envelope envelope = new Envelope();
       //head
       AgentHead agentHead = new AgentHead();
       //head添加子节点
       agentpayHead.setVersion("v1.0");
       agentpayHead.setCharset("UTF-8");
       //body
       AgentBody agentBody = new AgentBody();
       //envelope添加子节点
       envelope.setAgentHead(agentHead);
       envelope.setAgentBody(agentBody);

       //envelope 节点 转 xml
        String envelopeXML = object2Xml(envelope);
        System.out.println("-----> envelope报文： \n" + envelopeXML);
```

输出xml如下
```xml
<envelope>
    <head>
        <version>v1.0</version>
        <charset>UTF-8</charset>
    </head>
    </body>
</envelope>
```
