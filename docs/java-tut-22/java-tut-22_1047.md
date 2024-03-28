# 绑定 XML 模式

> 原文：[`docs.oracle.com/javase/tutorial/jaxb/intro/bind.html`](https://docs.oracle.com/javase/tutorial/jaxb/intro/bind.html)

本节描述了 JAXB 使用的默认 XML 到 Java 绑定。所有这些绑定都可以通过使用自定义绑定声明全局或逐案例地进行覆盖。有关默认 JAXB 绑定的完整信息，请参阅[JAXB 规范](http://jaxb.java.net)。

## 简单类型定义

使用简单类型定义的模式组件通常绑定到 Java 属性。由于有不同类型的模式组件，以下 Java 属性属性（对于模式组件通用）包括：

+   基本类型

+   集合类型，如果有的话

+   谓词

其余的 Java 属性属性在使用`simple`类型定义的模式组件中指定。

## 默认数据类型绑定

以下部分解释了默认模式到 Java、`JAXBElement`和 Java 到模式数据类型绑定。

### 模式到 Java 的映射

Java 语言提供了比 XML 模式更丰富的数据类型。以下表格提供了 JAXB 中 XML 数据类型到 Java 数据类型的映射。

表：XML 模式内置数据类型的 JAXB 映射

| XML 模式类型 | Java 数据类型 |
| --- | --- |
| `xsd:string` | `java.lang.String` |
| `xsd:integer` | `java.math.BigInteger` |
| `xsd:int` | `int` |
| `xsd.long` | `long` |
| `xsd:short` | `short` |
| `xsd:decimal` | `java.math.BigDecimal` |
| `xsd:float` | `float` |
| `xsd:double` | `double` |
| `xsd:boolean` | `boolean` |
| `xsd:byte` | `byte` |
| `xsd:QName` | `javax.xml.namespace.QName` |
| `xsd:dateTime` | `javax.xml.datatype.XMLGregorianCalendar` |
| `xsd:base64Binary` | `byte[]` |
| `xsd:hexBinary` | `byte[]` |
| `xsd:unsignedInt` | `long` |
| `xsd:unsignedShort` | `int` |
| `xsd:unsignedByte` | `short` |
| `xsd:time` | `javax.xml.datatype.XMLGregorianCalendar` |
| `xsd:date` | `javax.xml.datatype.XMLGregorianCalendar` |
| `xsd:g` | `javax.xml.datatype.XMLGregorianCalendar` |
| `xsd:anySimpleType` | `java.lang.Object` |
| `xsd:anySimpleType` | `java.lang.String` |
| `xsd:duration` | `javax.xml.datatype.Duration` |
| `xsd:NOTATION` | `javax.xml.namespace.QName` |

### `JAXBElement` 对象

当无法通过 XML 内容的派生 Java 表示来推断 XML 元素信息时，会提供一个`JAXBElement`对象。该对象具有获取和设置对象名称和对象值的方法。

### Java 到模式的映射

以下表格显示了 Java 类到 XML 数据类型的默认映射。

表：XML 数据类型到 Java 类的 JAXB 映射

| Java 类 | XML 数据类型 |
| --- | --- |
| `java.lang.String` | `xs:string` |
| `java.math.BigInteger` | `xs:integer` |
| `java.math.BigDecimal` | `xs:decimal` |
| `java.util.Calendar` | `xs:dateTime` |
| `java.util.Date` | `xs:dateTime` |
| `javax.xml.namespace.QName` | `xs:QName` |
| `java.net.URI` | `xs:string` |
| `javax.xml.datatype.XMLGregorianCalendar` | `xs:anySimpleType` |
| `javax.xml.datatype.Duration` | `xs:duration` |
| `java.lang.Object` | `xs:anyType` |
| `java.awt.Image` | `xs:base64Binary` |
| `javax.activation.DataHandler` | `xs:base64Binary` |
| `javax.xml.transform.Source` | `xs:base64Binary` |
| `java.util.UUID` | `xs:string` |
