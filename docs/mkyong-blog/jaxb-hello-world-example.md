# JAXB hello world 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/jaxb-hello-world-example/>

![java xml logo](img/42ef581321a9eae641edde67581e0960.png)

[Jakarta XML 绑定](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Jakarta_XML_Binding)(JAXB；以前的 Java Architecture for XML Binding)是一个 XML 绑定框架，用于在 Java 对象和 XML 之间进行转换。

1.  XML 编组–将 Java 对象转换成 XML。
2.  XML 解组——将 XML 转换回 Java 对象。

**注**
本文将重点介绍 [Java 11](http://web.archive.org/web/20220626054146/https://mkyong.com/java/what-is-new-in-java-11/) ，JAXB 3 和 [EclipseLink MOXy JAXB RI](http://web.archive.org/web/20220626054146/https://wiki.eclipse.org/EclipseLink/Examples/MOXy) 。

目录

*   [1。Java 6、7、8、Java 9、10、11 以及更高版本上的 JAXB](#jaxb-on-java-6-7-8-java-9-10-11-and-beyond)
*   [2。Java 11 和 JAXB RI 依赖关系](#java-11-and-jaxb-ri-dependencies)
    *   [2.1 EclipseLink MOXy](#eclipselink-moxy)
    *   [2.1 Jakarta XML 绑定](#jakarta-xml-binding)
*   [3。JAXB hello world 示例](#jaxb-hello-world-example)
    *   [3.1 JAXB XML 编组——将 Java 对象转换成 XML](#jaxb-xml-marshalling-convert-java-objects-to-xml)
    *   [3.2 JAXB XML 解组——将 XML 转换成 Java 对象](#jaxb-xml-unmarshalling-convert-xml-to-java-objects)
*   [4。什么是@XmlAccessorType(XmlAccessType。场)](#what-is-xmlaccessortypexmlaccesstypefield)
*   [5。javax.xml.*和 jakarta.xml.*](#difference-between-javaxxml-and-jakartaxml) 的区别
    *   【JAXB 版本 2 的 5.1 Java EE，javax . XML . *
    *   [5.2 Jakarta EE，jakarta.xml.*适用于 JAXB 版本 3](#jakarta-ee-jakartaxml-for-jaxb-version-3)
*   [6。JAXB 示例、列表、时间和 CDATA 适配器](#jaxb-examples-list-adaptor-for-time-and-cdata)
    *   [6.1 JAXB 域类](#jaxb-domain-class)
    *   [6.2 JAXB 适配器](#jaxb-adapter)
    *   [6.3 JAXB 和 CDATA](#jaxb-and-cdata)
    *   [6.4 运行 JAXB](#run-jaxb)
*   [7。JAXB 提示](#jaxb-tips)
    *   [7.1 XML 漂亮打印](#xml-pretty-print)
    *   [7.2 更改 XML 编码](#change-the-xml-encoding)
    *   [7.3 删除 XML 声明](#remove-the-xml-declaration)
    *   [7.4 改变字段的顺序](#change-the-order-of-the-fields)
    *   [7.5 隐藏指定的映射字段](#hide-a-specified-field-for-mapping)
    *   [7.6 没有找到 JAXB-API 的实现](#implementation-of-jaxb-api-has-not-been-found)
*   [8。下载源代码](#download-source-code)
*   [9。参考文献](#references)

## 1。Java 6、7、8、Java 9、10、11 以及更高版本上的 JAXB

下面是 Jakarta XML 绑定的简史(JAXB 以前的 XML 绑定 Java 架构)。

1.  随着基于 XML 的 web 服务的兴起，JAXB 成为了 Java 6 的一部分。
2.  基于 JSON 的 web 服务或 REST web 结构的兴起，使得开发者从 XML 迁移到 JSON 和 REST。
3.  JAXB 仍然是 Java 7 和 Java 8 的一部分。
4.  Java 9 弃用了 Java EE 模块，包括 JAXB `javax.xml.*`，并将其标记为[弃用以移除](http://web.archive.org/web/20220626054146/https://openjdk.java.net/jeps/277)，这意味着 JAXB 仍然是 Java 9 的一部分，未来的 Java 版本将移除它们。
5.  JAXB `javax.xml.*`仍然被弃用，并且是 Java 10 的一部分。
6.  JAXB 仍然是 Java 9 和 Java 10 的一部分，但是被禁用或者不包含在默认的模块路径中；但是，我们仍然可以通过`--add-modules`显式启用它(不推荐)。推荐的解决方案是添加一个单独的 JAXB API 和 JAXB 实现。
7.  微服务的兴起，开发者想要一个小巧轻便的 Java 运行时。
8.  Java 11 完全移除了 JAXB `javax.xml.*`。现在，我们需要添加一个单独的 JAXB API 和 JAXB 实现来使用 JAXB 特性。
9.  Oracle 向 Eclipse Foundation 提交了 Java EE，Java EE 更名为 Jakarta EE 是因为商标“Java”。JAXB `javax.xml.*`从 3.0 版本开始也重新打包到了`jakarta.xml.*`。

**延伸阅读**

*   [Java 11–JEP 320:移除 Java EE 和 CORBA 模块](http://web.archive.org/web/20220626054146/https://openjdk.java.net/jeps/320)
*   [维基百科–Jakarta XML 绑定](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Jakarta_XML_Binding)
*   [Wikipedia – Jakarta EE](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Jakarta_EE)

## 2。Java 11 和 JAXB RI 依赖关系

JAXB 是一个规范 [JSR-222](http://web.archive.org/web/20220626054146/https://jcp.org/en/jsr/detail?id=222) ，下面是两个常见的 JAXB 实现:

*   [EclipseLink MOXy](http://web.archive.org/web/20220626054146/https://wiki.eclipse.org/EclipseLink/Examples/MOXy)
*   [Jakarta XML 绑定](http://web.archive.org/web/20220626054146/https://eclipse-ee4j.github.io/jaxb-ri/)

### 2.1 EclipseLink MOXy

下面是 Maven EclipseLink MOXy 依赖。

pom.xml

```java
 <!-- JAXB API only -->
  <dependency>
      <groupId>jakarta.xml.bind</groupId>
      <artifactId>jakarta.xml.bind-api</artifactId>
      <version>3.0.0</version>
  </dependency>

  <!-- JAXB RI, EclipseLink MOXy -->
  <dependency>
      <groupId>org.eclipse.persistence</groupId>
      <artifactId>org.eclipse.persistence.moxy</artifactId>
      <version>3.0.0</version>
  </dependency> 
```

### 2.1 Jakarta XML 绑定

下面是 Maven [Jakarta XML 绑定](http://web.archive.org/web/20220626054146/https://eclipse-ee4j.github.io/jaxb-ri/)依赖。

pom.xml

```java
 <!-- JAXB API only -->
  <dependency>
      <groupId>jakarta.xml.bind</groupId>
      <artifactId>jakarta.xml.bind-api</artifactId>
      <version>3.0.0</version>
  </dependency>

  <!-- JAXB RI, Jakarta XML Binding -->
  <dependency>
      <groupId>com.sun.xml.bind</groupId>
      <artifactId>jaxb-impl</artifactId>
      <version>3.0.0</version>
      <scope>runtime</scope>
  </dependency> 
```

如果我们仍然喜欢旧的 JAXB 包`javax.xml.*`，那么坚持使用 JAXB 版本 2.x。

pom.xml

```java
 <dependency>
      <groupId>jakarta.xml.bind</groupId>
      <artifactId>jakarta.xml.bind-api</artifactId>
      <version>2.3.3</version>
  </dependency>

  <dependency>
      <groupId>com.sun.xml.bind</groupId>
      <artifactId>jaxb-ri</artifactId>
      <version>2.3.3</version>
  </dependency> 
```

**注**

*   在 Java 6、7 和 8 中，JAXB 是 JDK 的一部分。
*   在 Java 9、10、11 和更高版本中，我们需要添加单独的 JAXB API 和 JAXB RI 或实现库来使用 JAXB 特性。

## 3。JAXB hello world 示例

带有 JAXB 注释的类。

Fruit.java

```java
 package com.mkyong.xml.jaxb.model;

import jakarta.xml.bind.annotation.XmlAccessType;
import jakarta.xml.bind.annotation.XmlAccessorType;
import jakarta.xml.bind.annotation.XmlAttribute;
import jakarta.xml.bind.annotation.XmlElement;
import jakarta.xml.bind.annotation.XmlRootElement;

@XmlRootElement
// order of the fields in XML
// @XmlType(propOrder = {"price", "name"})
@XmlAccessorType(XmlAccessType.FIELD)
public class Fruit {

    @XmlAttribute
    int id;

    @XmlElement(name = "n")
    String name;

    String price;

    // getter, setter and toString...
} 
```

### 3.1 JAXB XML 编组——将 Java 对象转换成 XML

下面的 XML 编组 JAXB 示例将 Java 对象转换成 XML。

JaxbExampleFruit1.java

```java
 package com.mkyong.xml.jaxb;

import com.mkyong.xml.jaxb.model.Fruit;
import jakarta.xml.bind.JAXBContext;
import jakarta.xml.bind.JAXBException;
import jakarta.xml.bind.Marshaller;

import java.io.File;

public class JaxbExampleFruit1 {

    public static void main(String[] args) {

        JAXBContext jaxbContext = null;
        try {

            // Normal JAXB RI
            //jaxbContext = JAXBContext.newInstance(Fruit.class);

            // EclipseLink MOXy needs jaxb.properties at the same package with Fruit.class
            // Alternative, I prefer define this via eclipse JAXBContextFactory manually.
            jaxbContext = org.eclipse.persistence.jaxb.JAXBContextFactory
                    .createContext(new Class[]{Fruit.class}, null);

            Marshaller jaxbMarshaller = jaxbContext.createMarshaller();

            // output pretty printed
            jaxbMarshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);

            Fruit o = new Fruit();
            o.setId(1);
            o.setName("Banana");
            o.setPrice("9.99");

            // output to a xml file
            jaxbMarshaller.marshal(o, new File("C:\\test\\fruit.xml"));

            // output to console
            // jaxbMarshaller.marshal(o, System.out);

        } catch (JAXBException e) {
            e.printStackTrace();
        }

    }

} 
```

输出

C:\\test\\fruit.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<fruit id="1">
   <n>Banana</n>
   <price>9.99</price>
</fruit> 
```

### 3.2 JAXB XML 解组——将 XML 转换成 Java 对象

下面是 XML 解组的 JAXB 示例，将 XML 转换回 Java 对象。

JaxbExampleFruit2.java

```java
 package com.mkyong.xml.jaxb;

import com.mkyong.xml.jaxb.model.Fruit;
import jakarta.xml.bind.JAXBContext;
import jakarta.xml.bind.JAXBException;
import jakarta.xml.bind.Unmarshaller;

import java.io.File;

public class JaxbExampleFruit2 {

  public static void main(String[] args) {

      JAXBContext jaxbContext = null;
      try {

          // Normal JAXB RI
          //jaxbContext = JAXBContext.newInstance(Fruit.class);

          // EclipseLink MOXy needs jaxb.properties at the same package with Fruit.class
          // Alternative, I prefer define this via eclipse JAXBContextFactory manually.
          jaxbContext = org.eclipse.persistence.jaxb.JAXBContextFactory
                  .createContext(new Class[]{Fruit.class}, null);

          File file = new File("C:\\test\\fruit.xml");

          Unmarshaller jaxbUnmarshaller = jaxbContext.createUnmarshaller();

          Fruit o = (Fruit) jaxbUnmarshaller.unmarshal(file);

          System.out.println(o);

      } catch (JAXBException e) {
          e.printStackTrace();
      }

  }

} 
```

输出

Terminal

```java
 Fruit{id=1, name='Banana', price='9.99'} 
```

## 4。什么是@XmlAccessorType(XmlAccessType。场)

默认情况下，JAXB 实现将 getter/setter 对、公共字段和 JAXB 注释的非公共字段用于 XML 转换。

4.1 再次检查`Fruit`类，如果我们注释掉`@XmlAccessorType(XmlAccessType.FIELD)`，并重新运行上面的 JAXB 程序。

Fruit.java

```java
 @XmlRootElement
//@XmlAccessorType(XmlAccessType.FIELD)
public class Fruit {

    @XmlAttribute
    int id;

    @XmlElement(name = "n")
    String name;

    String price;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPrice() {
        return price;
    }

    public void setPrice(String price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Fruit{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", price='" + price + '\'' +
                '}';
    } 
```

4.2 我们将得到下面的`Class has two properties of the same name`错误信息。

Terminal

```java
 Class has two properties of the same name "id"
	this problem is related to the following location:
		at public int com.mkyong.xml.jaxb.model.Fruit.getId()
		at com.mkyong.xml.jaxb.model.Fruit
	this problem is related to the following location:
		at int com.mkyong.xml.jaxb.model.Fruit.id
		at com.mkyong.xml.jaxb.model.Fruit

Class has two properties of the same name "name"
	this problem is related to the following location:
		at public java.lang.String com.mkyong.xml.jaxb.model.Fruit.getName()
		at com.mkyong.xml.jaxb.model.Fruit
	this problem is related to the following location:
		at java.lang.String com.mkyong.xml.jaxb.model.Fruit.name
		at com.mkyong.xml.jaxb.model.Fruit 
```

JAXB 将把`Fruit.getId()`和`Fruit.id`(因为我们用`@XmlAttribute`或`@XmlElement`标注)视为同一个属性；为了解决这个问题，我们在类上添加了一个`@XmlAccessorType(XmlAccessType.FIELD)`，告诉 JAXB 只将`Fruit.id`作为属性。

**注**
eclipse link MOXy JAXB 不存在上述问题。

## 5。javax.xml.*和 jakarta.xml.*
的区别

Eclipse foundation 将 Java EE `javax.xml.*`更名为 Jakarta EE `jakarta.xml.*`。

下面是版本 2 和 3 中的一些 JAXB APIs。

```java
 // Jakarta EE
// @Since 3.0.0, rebrand to jakarta.xml
import jakarta.xml.bind.JAXBContext;
import jakarta.xml.bind.JAXBException;
import jakarta.xml.bind.Marshaller;

// Java EE
// old APIs JAXB version 2.*
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBException;
import javax.xml.bind.Marshaller; 
```

### 【JAXB 版本 2 的 5.1 Java EE，javax . XML . *

在 JAXB 版本 2 中，API 使用旧的 Java EE 包`javax.xml.*`。

pom.xml

```java
 <dependency>
      <groupId>javax.xml.bind</groupId>
      <artifactId>jaxb-api</artifactId>
      <version>2.3.1</version>
  </dependency>

  <!-- JAXB RI -->
  <dependency>
      <groupId>org.eclipse.persistence</groupId>
      <artifactId>org.eclipse.persistence.moxy</artifactId>
      <version>2.7.8</version>
  </dependency> 
```

### 5.2 Jakarta EE，jakarta.xml.*适用于 JAXB 版本 3

在 JAXB 版本 3.x 和更高版本中，API 被重新打包到 Jakarta EE 包`jakarta.xml.*`。

pom.xml

```java
 <!-- JAXB API -->
  <dependency>
      <groupId>jakarta.xml.bind</groupId>
      <artifactId>jakarta.xml.bind-api</artifactId>
      <version>3.0.0</version>
  </dependency>

  <!-- JAXB RI -->
  <dependency>
      <groupId>org.eclipse.persistence</groupId>
      <artifactId>org.eclipse.persistence.moxy</artifactId>
      <version>3.0.0</version>
  </dependency> 
```

## 6。JAXB 示例、列表、时间和 CDATA 适配器

下面的 JAXB 例子涉及以下内容:

1.  一个`Company.class`包含一个`Staff.class`列表，带有用于 XML 转换的 JAXB 注释。
2.  使用`@XmlJavaTypeAdapter`转换 Java 8 `ZonedDateTime`。
3.  对于特殊字符的 XML CDATA，`@XmlCDATA`只在 EclipseLink MOXy JAXB RI 中可用。

### 6.1 JAXB 域类

两个带有 JAXB 注释的域类。

Staff.java

```java
 package com.mkyong.xml.jaxb.model;

import com.mkyong.xml.jaxb.adaptor.TimeZoneAdaptor;

// @Since 3.0
import jakarta.xml.bind.annotation.XmlAccessType;
import jakarta.xml.bind.annotation.XmlAccessorType;
import jakarta.xml.bind.annotation.XmlAttribute;
import jakarta.xml.bind.annotation.XmlRootElement;
import jakarta.xml.bind.annotation.adapters.XmlJavaTypeAdapter;
import org.eclipse.persistence.oxm.annotations.XmlCDATA;

// Java 8?
//import com.sun.xml.internal.txw2.annotation.XmlCDATA;
// jaxb 2
//import javax.xml.bind.annotation.*;

import java.time.ZonedDateTime;

@XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
public class Staff {

  @XmlAttribute
  int id;

  String name;

  String Salary;

  @XmlCDATA
  String bio;

  @XmlJavaTypeAdapter(TimeZoneAdaptor.class)
  ZonedDateTime joinDate;

  //getters, setters
} 
```

Company.java

```java
 package com.mkyong.xml.jaxb.model;

//import javax.xml.bind.annotation.*;

// @Since 3.0.0
import jakarta.xml.bind.annotation.*;

import java.util.List;

@XmlRootElement
@XmlType(propOrder = {"name", "list"})
@XmlAccessorType(XmlAccessType.FIELD)
public class Company {

  @XmlElement(name = "staff")
  List<Staff> list;

  String name;

  // getters, and setters
} 
```

### 6.2 JAXB 适配器

我们可以使用`@XmlJavaTypeAdapter`将`ZonedDateTime`(或其他类型)与`String`相互转换。

TimeZoneAdaptor.java

```java
 package com.mkyong.xml.jaxb.adaptor;

// @Since 3.0.0
import jakarta.xml.bind.annotation.adapters.XmlAdapter;
//import javax.xml.bind.annotation.adapters.XmlAdapter;

import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;

public class TimeZoneAdaptor extends XmlAdapter<String, ZonedDateTime> {

    DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ISO_OFFSET_DATE_TIME;

    @Override
    public ZonedDateTime unmarshal(String v) throws Exception {
        ZonedDateTime parse = ZonedDateTime.parse(v, dateTimeFormatter);
        return parse;
    }

    @Override
    public String marshal(ZonedDateTime v) throws Exception {
        return dateTimeFormatter.format(v);
    }
} 
```

### 6.3 JAXB 和 CDATA

对于 XML 文档中的一些特殊字符，比如`<`和`&`，我们需要 [CDATA](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/CDATA) 。

我们选择 EclipseLink MOXy JAXB RI 是因为有一个内置的`@XmlCDATA`自动用`CDATA`将文本换行。

Staff.java

```java
 import org.eclipse.persistence.oxm.annotations.XmlCDATA;

//...

@XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
public class Staff {

  //...
  @XmlCDATA
  String bio; 
```

### 6.4 运行 JAXB

下面的 JAXB 示例尝试将对象列表转换为 XML 文档。

JaxbExample.java

```java
 package com.mkyong.xml.jaxb;

import com.mkyong.xml.jaxb.model.Company;
import com.mkyong.xml.jaxb.model.Staff;

// @Since 3.0.0, rebrand to jakarta.xml
import jakarta.xml.bind.JAXBContext;
import jakarta.xml.bind.JAXBException;
import jakarta.xml.bind.Marshaller;

// old APIs 2.3.*,
//import javax.xml.bind.JAXBContext;
//import javax.xml.bind.JAXBException;
//import javax.xml.bind.Marshaller;

import java.io.File;
import java.time.ZonedDateTime;
import java.util.Arrays;

public class JaxbExample {

    public static void main(String[] args) {

        JAXBContext jaxbContext = null;
        try {

            //jaxbContext = JAXBContext.newInstance(Company.class);

            // EclipseLink MOXy needs jaxb.properties at the same package with Company.class or Staff.class
            // Alternative, I prefer define this via eclipse JAXBContextFactory manually.
            jaxbContext = org.eclipse.persistence.jaxb.JAXBContextFactory
                    .createContext(new Class[] {Company.class}, null);

            Marshaller jaxbMarshaller = jaxbContext.createMarshaller();

            // output pretty printed
            jaxbMarshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);

            //jaxbMarshaller.marshal(createCompanyObject(), new File("C:\\test\\company.xml"));

            jaxbMarshaller.marshal(createCompanyObject(), System.out);

            // XML Unmarshalling
            /*File file = new File("C:\\test\\company.xml");
            Unmarshaller jaxbUnmarshaller = jaxbContext.createUnmarshaller();
            Company o = (Company) jaxbUnmarshaller.unmarshal(file);
            System.out.println(o);*/

        } catch (JAXBException e) {
            e.printStackTrace();
        }

    }

    private static Company createCompanyObject() {

        Company comp = new Company();
        comp.setName("ABCDEFG Enterprise");

        Staff o1 = new Staff();
        o1.setId(1);
        o1.setName("mkyong");
        o1.setSalary("8000 & Bonus");
        o1.setBio("<h1>support</h1>");
        o1.setJoinDate(ZonedDateTime.now().minusMonths(12));

        Staff o2 = new Staff();
        o2.setId(2);
        o2.setName("yflow");
        o2.setSalary("9000");
        o2.setBio("<h1>developer & database</h1>");
        o2.setJoinDate(ZonedDateTime.now().minusMonths(6));

        comp.setList(Arrays.asList(o1, o2));

        return comp;
    }
} 
```

输出

Terminal

```java
 <?xml version="1.0" encoding="UTF-8"?>
<company>
 <name>ABCDEFG Enterprise</name>
 <staff id="1">
    <name>mkyong</name>
    <Salary>8000 &amp; Bonus</Salary>
    <bio><![CDATA[<h1>support</h1>]]></bio>
    <joinDate>2020-04-21T12:19:28.5450719+08:00</joinDate>
 </staff>
 <staff id="2">
    <name>yflow</name>
    <Salary>9000</Salary>
    <bio><![CDATA[<h1>developer & database</h1>]]></bio>
    <joinDate>2020-10-21T12:19:28.5450719+08:00</joinDate>
 </staff>
</company> 
```

## 7 .。JAXB 提示

一些常见的 JAXB 问题和技巧。

### 7.1 XML 漂亮打印

默认情况下，JAXB 以紧凑模式输出 XML。

```java
 <?xml version="1.0" encoding="UTF-8"?><fruit id="1">
  <n>Banana</n><price>9.99</price></fruit> 
```

为了让 JAXB 以漂亮的打印或格式化模式输出 XML，我们可以将属性`Marshaller.JAXB_FORMATTED_OUTPUT`设置为`true`。

```java
 // default false
  jaxbMarshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true); 
```

输出

```java
 <?xml version="1.0" encoding="UTF-8"?>
<fruit id="1">
   <n>Banana</n>
   <price>9.99</price>
</fruit> 
```

### 7.2 更改 XML 编码

JAXB 默认 XML 编码为`encoding="UTF8"`，我们可以通过属性`Marshaller.JAXB_ENCODING`配置指定的 XML 编码。

```java
 // change XML encoding
  jaxbMarshaller.setProperty(Marshaller.JAXB_ENCODING, "ISO-8859-1"); 
```

输出

```java
 <?xml version="1.0" encoding="ISO-8859-1"?> 
```

### 7.3 删除 XML 声明

将属性`Marshaller.JAXB_FRAGMENT`设置为`true`，它将删除开始的 XML 声明。

```java
 // default false
  // remove <?xml version="1.0" encoding="UTF-8"?>
  jaxbMarshaller.setProperty(Marshaller.JAXB_FRAGMENT, true); 
```

输出

```java
 <fruit id="1">
 <n>Banana</n>
 <price>9.99</price>
</fruit> 
```

### 7.4 改变字段的顺序

我们可以使用`@XmlType propOrder`来改变写入 XML 文档的字段的顺序。

再次复习`Fruit`课。

Fruit.java

```java
 @XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
public class Fruit {

    @XmlAttribute
    int id;

    @XmlElement(name = "n")
    String name;

    String price;

    //...
} 
```

输出

```java
 <?xml version="1.0" encoding="UTF-8"?>
<fruit id="1">
   <n>Banana</n>
   <price>9.99</price>
</fruit> 
```

我们可以使用`@XmlType propOrder`来控制字段的顺序；例如，下面的例子想要首先显示`price`。

Fruit.java

```java
 package com.mkyong.xml.jaxb.model;

import jakarta.xml.bind.annotation.XmlAccessType;
import jakarta.xml.bind.annotation.XmlAccessorType;
import jakarta.xml.bind.annotation.XmlAttribute;
import jakarta.xml.bind.annotation.XmlElement;
import jakarta.xml.bind.annotation.XmlRootElement;
import jakarta.xml.bind.annotation.XmlType;

@XmlRootElement
@XmlType(propOrder = {"price", "name"})
@XmlAccessorType(XmlAccessType.FIELD)
public class Fruit {

  @XmlAttribute
  int id;

  @XmlElement(name = "n")
  String name;

  String price;

  //...
} 
```

输出

```java
 <?xml version="1.0" encoding="UTF-8"?>
<fruit id="1">
   <price>9.99</price>
   <n>Banana</n>
</fruit> 
```

### 7.5 隐藏指定的映射字段

我们可以使用`@XmlTransient`来隐藏或阻止特定字段转换成 XML。

Fruit.java

```java
 import jakarta.xml.bind.annotation.XmlTransient;

//...

@XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
public class Fruit {

    @XmlAttribute
    int id;

    @XmlElement(name = "n")
    String name;

    // Prevents the mapping
    @XmlTransient
    String price; 
```

输出

```java
 <?xml version="1.0" encoding="UTF-8"?>
<fruit id="1">
  <n>Banana</n>
</fruit> 
```

### 7.6 没有找到 JAXB-API 的实现

我们需要提供一个 JAXB RI 或实现，参考这篇[文章](http://web.archive.org/web/20220626054146/https://mkyong.com/java/jaxbexception-implementation-of-jaxb-api-has-not-been-found-on-module-path-or-classpath/)。

相关错误

*   [Java . lang . noclassdeffounderror:javax/XML/bind/JAXB exception](http://web.archive.org/web/20220626054146/https://mkyong.com/java/java-lang-noclassdeffounderror-javax-xml-bind-jaxbexception/)
*   [Java . lang . classnotfoundexception:com . sun . XML . bind . v2 . context factory](http://web.archive.org/web/20220626054146/https://mkyong.com/java/java-lang-classnotfoundexception-com-sun-xml-bind-v2-contextfactory/)

## 8。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054146/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/JAXB/

## 9。参考文献

*   [JAXB 规格 JSR-222](http://web.archive.org/web/20220626054146/https://jcp.org/en/jsr/detail?id=222)
*   [Oracle–JAXB 简介](http://web.archive.org/web/20220626054146/https://docs.oracle.com/javase/tutorial/jaxb/intro/index.html)
*   [Wikipedia – Jakarta EE](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Jakarta_EE)
*   [维基百科–Jakarta XML 绑定](http://web.archive.org/web/20220626054146/https://en.wikipedia.org/wiki/Jakarta_XML_Binding)
*   [JAXB 用户指南](http://web.archive.org/web/20220626054146/https://javaee.github.io/jaxb-v2/doc/user-guide/ch03.html)
*   [EclipseLink MOXy](http://web.archive.org/web/20220626054146/https://wiki.eclipse.org/EclipseLink/Examples/MOXy)
*   [用于 XML 绑定的 Java 架构(JAXB)](http://web.archive.org/web/20220626054146/https://javaee.github.io/jaxb-v2/)
*   [Jakarta XML 绑定](http://web.archive.org/web/20220626054146/https://eclipse-ee4j.github.io/jaxb-ri/)
*   [Java 11–JEP 320:移除 Java EE 和 CORBA 模块](http://web.archive.org/web/20220626054146/https://openjdk.java.net/jeps/320)
*   [Java 8 – ZonedDateTime examples](http://web.archive.org/web/20220626054146/https://mkyong.com/java8/java-8-zoneddatetime-examples/)
*   [Java 8–如何将字符串转换为本地日期](http://web.archive.org/web/20220626054146/https://mkyong.com/java8/java-8-how-to-convert-string-to-localdate/)
*   [JAXB 异常:没有找到 JAXB-API 的实现](http://web.archive.org/web/20220626054146/https://mkyong.com/java/jaxbexception-implementation-of-jaxb-api-has-not-been-found-on-module-path-or-classpath/)
*   [stack overflow——如何使用 JAXB 生成 CDATA 块？](http://web.archive.org/web/20220626054146/https://stackoverflow.com/questions/3136375/how-to-generate-cdata-block-using-jaxb)
*   [Java 9、10、11 及更高版本上的 JAXB](http://web.archive.org/web/20220626054146/https://jesperdj.com/2018/09/30/jaxb-on-java-9-10-11-and-beyond/)

<input type="hidden" id="mkyong-current-postId" value="9909">