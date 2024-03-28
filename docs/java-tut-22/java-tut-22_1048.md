# 自定义生成的类和 Java 程序元素

> 原文：[`docs.oracle.com/javase/tutorial/jaxb/intro/customize.html`](https://docs.oracle.com/javase/tutorial/jaxb/intro/customize.html)

以下部分描述了如何自定义生成的 JAXB 类和 Java 程序元素。

## 模式到 Java

自定义 JAXB 绑定声明使您能够在 XML 模式中包含 Java 特定的细化，例如类和包名称映射，以定制生成的 JAXB 类。

JAXB 提供了两种自定义 XML 模式的方法：

+   作为源 XML 模式中的内联注释

+   作为传递给 JAXB 绑定编译器的外部绑定自定义文件中的声明

提供了显示如何自定义 JAXB 绑定的代码示例，稍后在本文档中提供。

## Java 到模式

在 `javax.xml.bind.annotation` 包中定义的 JAXB 注释可用于自定义 Java 程序元素到 XML 模式的映射。以下表总结了可以与 Java 包一起使用的 JAXB 注释。

表：与 Java 包相关的 JAXB 注释

| 注释 | 描述和默认设置 |
| --- | --- |

| `@XmlSchema` | 将包映射到 XML 目标命名空间。默认设置：

```java
@XmlSchema ( 
    xmlns = {}, 
    namespace = "", 
    elementFormDefault = XmlNsForm.UNSET, 
    attributeFormDefault = XmlNsForm.UNSET
)

```

|

| `@XmlAccessorType` | 控制字段和属性的默认序列化。默认设置：

```java
@XmlAccessorType (
    value = AccessType.PUBLIC_MEMBER 
)
```

|

| `@XmlAccessorOrder` | 控制映射到 XML 元素的属性和字段的默认排序。默认设置：

```java
@XmlAccessorOrder (
    value = AccessorOrder.UNDEFINED
)

```

|

| `@XmlSchemaType` | 允许自定义映射到 XML 模式内置类型。默认设置：

```java
@XmlSchemaType (
    namespace = 
    "http://www.w3.org/2001/XMLSchema", 
    type = DEFAULT.class
)

```

|

| `@XmlSchemaTypes` | 用于定义多个 `@XmlSchemaType` 注释的容器注释。默认设置：无 |
| --- | --- |

以下表总结了可以与 Java 类一起使用的 JAXB 注释。

表：与 Java 类相关的 JAXB 注释

| 注释 | 描述和默认设置 |
| --- | --- |

| `@XmlType` | 将 Java 类映射到模式类型。默认设置：

```java
@XmlType (
    name = "##default", 
    propOrder = {""}, 
    namespace = "##default", 
    factoryClass = DEFAULT.class, 
    factoryMethod = ""
)

```

|

| `@XmlRootElement` | 将全局元素与类映射到的模式类型关联。默认设置：

```java
@XmlRootElement (
    name = "##default", 
    namespace = "##default" 
)

```

|

以下表总结了可以与 Java `enum` 类型一起使用的 JAXB 注释。

表：与 Java `enum` 类型相关的 JAXB 注释

| 注释 | 描述和默认设置 |
| --- | --- |

| `@XmlEnum` | 将 Java 类型映射到 XML 简单类型。默认设置：

```java
@XmlEnum ( value = String.class )

```

|

| `@XmlEnumValue` | 将 Java 类型映射到 XML 简单类型。默认设置：无 |
| --- | --- |

| `@XmlType` | 将 Java 类映射到模式类型。默认设置：

```java

@XmlType (
    name = "##default", 
    propOrder = {""}, 
    namespace = "##default", 
    factoryClass = DEFAULT.class, 
    factoryMethod = ""
)

```

|

| `@XmlRootElement` | 将全局元素与类映射到的模式类型关联。默认设置：

```java
@XmlRootElement (
    name = "##default", 
    namespace = "##default" 
)

```

|

以下表总结了可以与 Java 属性和字段一起使用的 JAXB 注释。

表：与 Java 属性和字段相关的 JAXB 注释

| 注释 | 描述和默认设置 |
| --- | --- |

| `@XmlElement` | 将 JavaBeans 属性或字段映射到从属性或字段名称派生的 XML 元素。默认设置：

```java
@XmlElement (
    name = "##default", 
    nillable = false, 
    namespace = "##default", 
    type = DEFAULT.class, 
    defaultValue = "\u0000"
)

```

|

| `@XmlElements` | 用于定义多个 `@XmlElement` 注解的容器注解。默认设置：无 |
| --- | --- |

| `@XmlElementRef` | 将 JavaBeans 属性或字段映射到从属性或字段类型派生的 XML 元素。默认设置：

```java
@XmlElementRef (
    name = "##default", 
    namespace = "##default", 
    type = DEFAULT.class
)

```

|

| `@XmlElementRefs` | 用于定义多个 `@XmlElementRef` 注解的容器注解。默认设置：无 |
| --- | --- |

| `@XmlElementWrapper` | 在 XML 表示周围生成一个包装器元素。通常用作集合周围的包装 XML 元素。默认设置：

```java
@XmlElementWrapper (
    name = "##default", 
    namespace = "##default", 
    nillable = false
)

```

|

| `@XmlAnyElement` | 将 JavaBeans 属性映射到 XML 信息集表示或 JAXB 元素。默认设置：

```java
@XmlAnyElement (
    lax = false, 
    value = W3CDomHandler.class
)

```

|

| `@XmlAttribute` | 将一个 JavaBeans 属性映射到 XML 属性。默认设置：

```java
@XmlAttribute (
    name = ##default, 
    required = false, 
    namespace = "##default" 
)

```

|

| `@XmlAnyAttribute` | 将 JavaBeans 属性映射到通配符属性映射。默认设置：无 |
| --- | --- |
| `@XmlTransient` | 防止将 JavaBeans 属性映射到 XML 表示。默认设置：无 |
| `@XmlValue` | 定义类到 XML Schema 复杂类型或 XML Schema 简单类型的映射。默认设置：无 |
| `@XmlID` | 将 JavaBeans 属性映射到 XML ID。默认设置：无 |
| `@XmlIDREF` | 将 JavaBeans 属性映射到 XML IDREF。默认设置：无 |
| `@XmlList` | 将属性映射到简单类型列表。默认设置：无 |
| `@XmlMixed` | 标记 JavaBeans 多值属性以支持混合内容。默认设置：无 |
| `@XmlMimeType` | 关联控制属性的 XML 表示的 MIME 类型。默认设置：无 |
| `@XmlAttachmentRef` | 标记字段/属性，其 XML 表单是指向 mime 内容的 URI 引用。默认设置：无 |
| `@XmlInlineBinaryData` | 禁用对绑定到 XML 中 base64 编码二进制数据的数据类型考虑 XOP 编码。默认设置：无 |

以下表总结了可以与对象工厂一起使用的 JAXB 注解。

表格：与对象工厂相关的 JAXB 注解

| 注解 | 描述和默认设置 |
| --- | --- |

| `@XmlElementDecl` | 将工厂方法映射到 XML 元素。默认设置：

```java
@XmlElementDecl (
    scope = GLOBAL.class, 
    namespace = "##default", 
    substitutionHeadNamespace = "##default", 
    substitutionHeadName = ""
)

```

|

以下表总结了可以与适配器一起使用的 JAXB 注解。

表格：与适配器相关的 JAXB 注解

| 注解 | 描述和默认设置 |
| --- | --- |

| `@XmlJavaTypeAdapter` | 使用实现 `@XmlAdapter` 注解的适配器进行自定义编组。默认设置：

```java
@XmlJavaTypeAdapter ( type = DEFAULT.class )

```

|

| `@XmlJavaTypeAdapters` | 用于在包级别定义多个 `@XmlJavaTypeAdapter` 注解的容器注解。默认设置：无 |
| --- | --- |
