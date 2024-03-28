# 新属性

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/properties/properties.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/properties.html)

JAXP 1.5 定义了三个新属性，用于调节 XML 处理器是否解析上述外部资源。这些属性是：

+   `javax.xml.XMLConstants.ACCESS_EXTERNAL_DTD`

+   `javax.xml.XMLConstants.ACCESS_EXTERNAL_SCHEMA`

+   `javax.xml.XMLConstants.ACCESS_EXTERNAL_STYLESHEET`

这些 API 属性具有相应的系统属性和 jaxp.properties。

## ACCESS_EXTERNAL_DTD

**名称**：`http://javax.xml.XMLConstants/property/accessExternalDTD`

**定义**：限制对外部 DTD、外部实体引用到指定协议的访问。

**值**：参见属性的值

**默认值**：`all`，允许连接到所有协议。

**系统属性**：`javax.xml.accessExternalDTD`

## ACCESS_EXTERNAL_SCHEMA

**名称**：`http://javax.xml.XMLConstants/property/accessExternalSchema`

**定义**：限制对由`schemaLocation`属性、Import 和 Include 元素设置的外部引用协议的访问。

**值**：参见属性的值

**默认值**：`all`，允许连接到所有协议。

**系统属性**：`javax.xml.accessExternalSchema`

## ACCESS_EXTERNAL_STYLESHEET

**名称**：`http://javax.xml.XMLConstants/property/accessExternalStylesheet`

**定义**：限制对由样式表处理指令、文档函数、Import 和 Include 元素设置的外部引用协议的访问。

**值**：参见属性的值

**默认值**：`all`，允许连接到所有协议。

**系统属性**：`javax.xml.accessExternalStylesheet`

## ${java.home}/lib/jaxp.properties

这些属性可以在`jaxp.properties`中指定，以定义所有使用 Java Runtime 的应用程序的行为。格式为`property-name=[value][,value]*`。例如：

```java
javax.xml.accessExternalDTD=file,http

```

属性名称与系统属性相同：`javax.xml.accessExternalDTD`、`javax.xml.accessExternalSchema`和`javax.xml.accessExternalStylesheet`。

## 属性的值

所有属性的值格式相同。

**值**：由逗号分隔的协议列表。协议是 URI 的 scheme 部分，或者在 JAR 协议的情况下，由冒号分隔的"jar"加上 scheme 部分。协议定义为：

`scheme = alpha *( alpha | digit | "+" | "-" | "." )`

其中 alpha = a-z 和 A-Z。

以及 JAR 协议：

`jar[:scheme]`

协议不区分大小写。值中由`Character.isSpaceChar`定义的任何空格将被忽略。协议的示例包括`file`、`http`、`jar:file`。

**默认值**：默认值是实现特定的。在 JAXP 1.5 RI、Java SE 7u40 和 Java SE 8 中，默认值为`all`，授予所有协议的权限。

**授予所有访问**：关键字`all`授予所有协议的权限。例如，在`jaxp.properties`中设置`javax.xml.accessExternalDTD=all`将允许系统像以前一样工作，无限制地访问外部 DTD 和实体引用。

**拒绝任何访问**：空字符串，即""，表示不授予任何协议权限。例如，在`jaxp.properties`中设置`javax.xml.accessExternalDTD=""`将指示 JAXP 处理器拒绝任何外部连接。
