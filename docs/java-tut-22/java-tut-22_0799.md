# 处理限制定义

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/limits/limits.html`](https://docs.oracle.com/javase/tutorial/jaxp/limits/limits.html)

以下列表描述了 JDK 支持的 JAXP XML 处理限制。这些限制可以通过工厂 API、系统属性和`jaxp.properties`文件指定。

## entityExpansionLimit

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/entityExpansionLimit` |
| 定义 | 限制实体扩展的数量。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 64000 |
| 系统属性 | `jdk.xml.entityExpansionLimit` |
| 自从 | 7u45, 8 |

## elementAttributeLimit

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/elementAttributeLimit` |
| 定义 | 限制元素可以拥有的属性数量。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 10000 |
| 系统属性 | `jdk.xml.elementAttributeLimit` |
| 自从 | 7u45, 8 |

## maxOccurLimit

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/maxOccurLimit` |
| 定义 | 限制在构建包含值不是"unbounded"的`maxOccurs`属性的 W3C XML Schema 的语法时可以创建的内容模型节点的数量。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 5000 |
| 系统属性 | `jdk.xml.maxOccurLimit` |
| 自从 | 7u45, 8 |

## totalEntitySizeLimit

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/totalEntitySizeLimit` |
| 定义 | 限制包含通用实体和参数实体的所有实体的总大小。大小是所有实体的聚合计算。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 5x10⁷ |
| 系统属性 | `jdk.xml.totalEntitySizeLimit` |
| 自从 | 7u45, 8 |

## maxGeneralEntitySizeLimit

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/maxGeneralEntitySizeLimit` |
| 定义 | 限制任何通用实体的最大大小。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 0 |
| 系统属性 | `jdk.xml.maxGeneralEntitySizeLimit` |
| 自从 | 7u45, 8 |

## maxParameterEntitySizeLimit

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/maxParameterEntitySizeLimit` |
| 定义 | 限制任何参数实体的最大大小，包括嵌套多个参数实体的结果。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 1000000 |
| 系统属性 | `jdk.xml.maxParameterEntitySizeLimit` |
| 自 JDK 7u45, 8 起 |

## entityReplacementLimit

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/entityReplacementLimit` |
| 定义 | 限制所有实体引用中节点的总数。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 3000000 |
| 系统属性 | `jdk.xml.entityReplacementLimit` |
| 自 JDK 7u111, 8u101 起 |

## maxElementDepth

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/maxElementDepth` |
| 定义 | 限制最大元素深度。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 0 |
| 系统属性 | `jdk.xml.maxElementDepth` |
| 自 JDK 7u65, 8u11 起 |

## maxXMLNameLimit

| 属性 | 描述 |
| --- | --- |
| 名称 | `http://www.oracle.com/xml/jaxp/properties/maxXMLNameLimit` |
| 定义 | 限制 XML 名称的最大大小，包括元素名称、属性名称和命名空间前缀和 URI。 |
| 值 | 一个正整数。小于或等于 0 的值表示没有限制。如果值不是整数，则会抛出`NumericFormatException`异常。 |
| 默认值 | 1000 |
| 系统属性 | `jdk.xml.maxXMLNameLimit` |
| 自 JDK 7u91, 8u65 起 |

## 旧版系统属性

这些属性自 JDK 5.0 和 6 起被引入，并继续为向后兼容性而受支持。

| 系统属性 | 自 JDK 5.0 和 6 起 | 新系统属性 |
| --- | --- | --- |
| entityExpansionLimit | 1.5 | jdk.xml.entityExpansionLimit |
| elementAttributeLimit | 1.5 | jdk.xml.elementAttributeLimit |
| maxOccurLimit | 1.6 | jdk.xml.maxOccur |

## {java.home}/lib/jaxp.properties

可以在`jaxp.properties`文件中指定系统属性，以定义 JDK 或 JRE 的所有调用的行为。格式为`system-property-name=value`。例如：

```java
jdk.xml.maxGeneralEntitySizeLimit=1024

```
