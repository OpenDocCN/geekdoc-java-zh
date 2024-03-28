# 使用属性

> 译文：[`docs.oracle.com/javase/tutorial/jaxp/properties/usingProps.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/usingProps.html)

本节重点介绍了 JAXP 1.5 中引入的新属性。

## 何时使用属性

只有处理不受信任的 XML 内容的应用程序才需要限制获取外部资源。不处理不受信任内容的内部系统和应用程序不需要关注新的限制或进行任何更改。自 7u40 和 JDK8 默认没有对此类限制的要求，应用程序在升级到 7u40 和 JDK8 时不会出现行为变化。

对于处理不受信任的 XML 输入、Schema 或样式表的应用程序，如果已经存在安全措施，比如启用 Java 安全管理器仅授予受信任的外部连接，或者使用解析器解析实体，则不需要 JAXP 1.5 中添加的新功能。

然而，JAXP 1.5 确实为没有安全管理器运行的系统和应用程序提供了直接的保护。对于这类应用程序，可以考虑使用下面详细描述的新功能来进行限制。

## 通过 API 设置属性

当改变代码可行时，通过 JAXP 工厂或解析器设置新属性是启用限制的最佳方式。属性可以通过以下接口设置：

```java
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
dbf.setAttribute(name, value);

SAXParserFactory spf = SAXParserFactory.newInstance();
SAXParser parser = spf.newSAXParser();
parser.setProperty(name, value);

XMLInputFactory xif = XMLInputFactory.newInstance();
xif.setProperty(name, value);

SchemaFactory schemaFactory = SchemaFactory.newInstance(schemaLanguage);
schemaFactory.setProperty(name, value);

TransformerFactory factory = TransformerFactory.newInstance();
factory.setAttribute(name, value);

```

以下是一个将 DOM 解析器限制为仅本地连接的外部 DTD 的示例：

```java
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
try {
    dbf.setAttribute({{XMLConstants.ACCESS_EXTERNAL_DTD}}, "file, jar:file");
} catch (IllegalArgumentException e) {
    //jaxp 1.5 feature not supported
}

```

当代码更改可行，并且对于新开发，建议设置新属性如上所示。通过这种方式设置属性，应用程序可以确保无论部署到较旧还是较新版本的 JDK，或者通过系统属性或`jaxp.properties`设置属性，都能保持所需的行为。

## 使用系统属性

如果改变代码不可行，系统属性可能会有用。

如果希望为整个 JDK/JRE 调用设置限制，可以在命令行上设置系统属性；如果仅需要部分应用程序，可以在该部分之前设置系统属性，然后在之后清除。例如，以下代码展示了如何使用系统属性：

```java
//allow resolution of external schemas

System.setProperty("javax.xml.accessExternalSchema", "file, http");

//this setting will affect all processing after it's set
some processing here

//after it's done, clear the property
System.clearProperty("javax.xml.accessExternalSchema");

```

## 使用 jaxp.properties

`jaxp.properties` 是一个普通的配置文件。它位于`${java.home}/lib/jaxp.properties`，其中 `java.home` 是 JRE 安装目录，例如，`[安装目录路径]/jdk7/jre`。

可通过将以下行添加到 jaxp.properties 文件来设置外部访问限制：

```java
javax.xml.accessExternalStylesheet=file, http

```

设置此项后，所有 JDK/JRE 的调用将遵守加载外部样式表的限制。

对于不希望允许 XML 处理器进行任何外部连接的系统，此功能可能很有用，此时，所有三个属性可以设置为，例如，仅文件。
