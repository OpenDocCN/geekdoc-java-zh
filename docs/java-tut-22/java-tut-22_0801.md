# 使用限制

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/limits/using.html`](https://docs.oracle.com/javase/tutorial/jaxp/limits/using.html)

## 环境评估

评估包括在系统级别考虑应用程序可用的内存量，是否接受和处理来自不受信任来源的 XML、XSD 或 XSL 源，以及在应用程序级别考虑是否使用某些构造（如 DTD）。

## 内存设置和限制

XML 处理可能非常消耗内存。允许消耗的内存量取决于特定环境中应用程序的要求。必须防止处理格式不正确的 XML 数据消耗过多内存。

默认限制通常设置为允许大多数应用程序的合法 XML 输入，并允许小型硬件系统（如 PC）的内存使用。建议将限制设置为可能的最小值，以便在消耗大量内存之前捕获任何格式不正确的输入。

这些限制是相关的，但并非完全冗余。您应为所有限制设置适当的值：通常限制应设置为比默认值小得多的值。

例如，可以设置`ENTITY_EXPANSION_LIMIT`和`GENERAL_ENTITY_SIZE_LIMIT`来防止过多的实体引用。但是当扩展和实体大小的确切组合未知时，`TOTAL_ENTITY_SIZE_LIMIT`可以作为整体控制。同样，虽然`TOTAL_ENTITY_SIZE_LIMIT`控制替换文本的总大小，但如果文本是一个非常大的 XML 块，`ENTITY_REPLACEMENT_LIMIT`会限制文本中可以出现的节点总数，并防止系统过载。

## 通过使用`getEntityCountInfo`属性估计限制

为帮助您分析应设置的限制值，提供了一个名为`http://www.oracle.com/xml/jaxp/properties/getEntityCountInfo`的特殊属性。以下代码片段显示了使用该属性的示例：

```java
parser.setProperty("http://www.oracle.com/xml/jaxp/properties/getEntityCountInfo", "yes");

```

查看示例以获取有关下载示例代码的更多信息。

当程序在 W3C MathML 3.0 中运行时，将打印出以下表格：

| 属性 | 限制 | 总大小 | 大小 | 实体名称 |
| --- | --- | --- | --- | --- |
| `ENTITY_EXPANSION_LIMIT` | 64000 | 1417 | 0 | `null` |
| `MAX_OCCUR_NODE_LIMIT` | 5000 | 0 | 0 | `null` |
| `ELEMENT_ATTRIBUTE_LIMIT` | 10000 | 0 | 0 | `null` |
| `TOTAL_ENTITY_SIZE_LIMIT` | 50000000 | 55425 | 0 | `null` |
| `GENERAL_ENTITY_SIZE_LIMIT` | 0 | 0 | 0 | `null` |
| `PARAMETER_ENTITY_SIZE_LIMIT` | 1000000 | 0 | 7303 | %MultiScriptExpression |
| `MAX_ELEMENT_DEPTH_LIMIT` | 0 | 2 | 0 | `null` |
| `MAX_NAME_LIMIT` | 1000 | 13 | 13 | `null` |
| `ENTITY_REPLACEMENT_LIMIT` | 3000000 | 0 | 0 | `null` |

在此示例中，实体引用的总数，或实体扩展，为 1417；默认限制为 64000。所有实体的总大小为 55425；默认限制为 50000000。在解析所有引用后，最大的参数实体是 `%MultiScriptExpression`，长度为 7303；默认限制为 1000000。

如果这是应用程序预计要处理的最大文件，请建议将限制设置为较小的数字。例如，`ENTITY_EXPANSION_LIMIT` 设置为 2000，`TOTAL_ENTITY_SIZE_LIMIT` 设置为 100000，`PARAMETER_ENTITY_SIZE_LIMIT` 设置为 10000。

## 设置限制

限制可以像其他 JAXP 属性一样设置。它们可以通过工厂方法或解析器设置：

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

以下示例显示了如何使用 `DocumentBuilderFactory` 设置限制：

```java
dbf.setAttribute(JDK_ENTITY_EXPANSION_LIMIT, "2000");
dbf.setAttribute(TOTAL_ENTITY_SIZE_LIMIT, "100000");
dbf.setAttribute(PARAMETER_ENTITY_SIZE_LIMIT, "10000"); 
dbf.setAttribute(JDK_MAX_ELEMENT_DEPTH, "100"); 

```

## 使用系统属性

如果更改代码不可行，系统属性可能很有用。

要为整个 JDK 或 JRE 的调用设置限制，请在命令行上设置系统属性。要仅为应用程序的一部分设置限制，可以在该部分之前设置系统属性，并在之后清除。以下代码显示了如何使用系统属性：

```java
public static final String SP_GENERAL_ENTITY_SIZE_LIMIT = "jdk.xml.maxGeneralEntitySizeLimit";

//set limits using system property
System.setProperty(SP_GENERAL_ENTITY_SIZE_LIMIT, "2000");

//this setting will affect all processing after it's set
...

//after it is done, clear the property
System.clearProperty(SP_GENERAL_ENTITY_SIZE_LIMIT);

```

请注意，属性的值应为整数。如果输入的值不包含可解析的整数，将抛出 `NumberFormatException`；请参阅方法 [`parseInt(String)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#parseInt-java.lang.String-)。

查看 示例 以获取有关下载示例代码的更多信息。

## 使用 `jaxp.properties` 文件

`jaxp.properties` 文件是一个配置文件。通常位于 `${*java.home*}/lib/jaxp.properties`，其中 `*java.home*` 是 JRE 安装目录，例如，*[安装目录路径]*/jdk8/jre。

可通过向 `jaxp.properties` 文件添加以下行来设置限制：

```java
jdk.xml.maxGeneralEntitySizeLimit=2000

```

请注意，属性名称与系统属性相同，并具有前缀 `jdk.xml`。属性的值应为整数。如果输入的值不包含可解析的整数，将抛出 `NumberFormatException`；请参阅方法 [`parseInt(String)`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#parseInt-java.lang.String-)。

当在文件中设置属性时，所有 JDK 和 JRE 的调用都将遵守限制。
