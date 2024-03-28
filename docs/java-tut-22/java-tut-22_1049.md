# JAXB 示例

> 原文：[`docs.oracle.com/javase/tutorial/jaxb/intro/examples.html`](https://docs.oracle.com/javase/tutorial/jaxb/intro/examples.html)

以下部分描述如何使用包含在 JAXB RI 捆绑包中的示例应用程序。JAXB RI 捆绑包可从[`jaxb.java.net`](http://jaxb.java.net)获取。下载并安装 JAXB RI 捆绑包。示例位于*jaxb-ri-install*`/samples/`目录中。这些示例演示并构建了关键的 JAXB 功能和概念。请按照呈现的顺序执行这些步骤。

阅读完本节后，您应该对 JAXB 感到足够自信，可以：

+   从 XML 模式生成 JAXB Java 类

+   在 Java 应用程序中使用基于模式的 JAXB 类解组和组合 XML 内容

+   使用基于模式的 JAXB 类创建 Java 内容树

+   在解组期间和运行时验证 XML 内容

+   自定义 JAXB 模式到 Java 绑定

本文描述了三组示例：

+   基本示例（修改 Marshal、Unmarshal Validate）演示了基本的 JAXB 概念，如使用默认设置和绑定来解组、组合和验证 XML 内容。

+   自定义示例（Customize Inline、Datatype Converter、External Customize）演示了将 XML 模式默认绑定到 Java 对象的各种自定义方式。

+   java-to-schema 示例展示了如何使用注解将 Java 类映射到 XML 模式。

* * *

**注意：**基本和自定义示例基于采购订单场景。每个示例使用一个 XML 文档`po.xml`，根据一个 XML 模式`po.xsd`编写。这些文档源自由 David C. Fallside 编辑的[W3C XML Schema Part 0: Primer](http://www.w3.org/TR/xmlschema-0/)。

* * *

基本和自定义示例目录包含几个基本文件：

+   `po.xsd`是作为 JAXB 绑定编译器输入的 XML 模式，将从中生成基于模式的 JAXB Java 类。对于自定义内联和数据类型转换器示例，此文件包含内联绑定自定义。

+   `po.xml`是包含示例 XML 内容的采购订单 XML 文件，在每个示例中解组为 Java 内容树。这个文件在每个示例中几乎相同；为了突出不同的 JAXB 概念，存在轻微的内容差异。

+   `Main.java`是每个示例的主要 Java 类。

+   `build.xml`是一个 Ant 项目文件，为您提供方便。使用 Ant 工具自动生成、编译和运行基于模式的 JAXB 类。`build.xml`文件在示例之间有所不同。

+   `inline-customize`示例中的`MyDatatypeConverter.java`是一个 Java 类，用于提供自定义数据类型转换。

+   External Customize 示例中的`binding.xjb`是一个外部绑定声明文件，传递给 JAXB 绑定编译器以自定义默认的 JAXB 绑定。

以下表简要描述了基本、定制和 Java 到模式 JAXB 示例。

表：基本 JAXB 示例

| 示例名称 | 描述 |
| --- | --- |
| Modify Marshal | 演示如何修改 Java 内容树。 |
| Unmarshal Validate | 演示如何在解组期间启用验证。 |

表：定制 JAXB 示例

| 示例名称 | 描述 |
| --- | --- |
| Customize Inline | 演示如何通过在 XML 模式中使用内联注解来定制默认的 JAXB 绑定。 |
| Datatype Converter | 演示了将 XML `simpleType`定义绑定到 Java 数据类型的替代、更简洁的绑定方法，类似于内联定制示例。 |
| External Customize | 演示如何使用外部绑定声明文件来传递对 JAXB 绑定编译器的只读模式进行绑定自定义。 |

表：Java 到模式 JAXB 示例

| 示例名称 | 描述 |
| --- | --- |
| Create Marshal | 演示如何使用 ObjectFactory 类创建 Java 内容树并将其编组为 XML 数据。还演示了如何向 JAXB 列表属性添加内容。 |
| XmlAccessorOrder | 演示如何在 Java 类中使用`@XmlAccessorOrder`和`@XmlType.propOrder`映射注解来控制 Java 类型在编组或解组 XML 内容时的顺序。 |
| XmlAdapter | 演示如何使用接口`XmlAdapter`和注解`@XmlJavaTypeAdapter`为使用整数（`int`）作为键和字符串（`String`）作为值的`HashMap`（字段）提供自定义映射 XML 内容的方法。 |
| XmlAttribute | 演示如何使用注解`@XmlAttribute`来定义一个属性或字段作为 XML 属性处理。 |
| XmlRootElement | 演示如何使用注解`@XmlRootElement`为相应类的 XML 模式类型定义 XML 元素名称。 |
| XmlSchemaType Class | 演示如何使用注解`@XmlSchemaType`来自定义属性或字段与 XML 内置类型的映射。 |
| XmlType | 演示如何使用注解`@XmlType`将一个类或`enum`类型映射到 XML 模式类型。 |

## JAXB 编译器选项

JAXB XJC 模式绑定编译器将源 XML 模式转换或绑定到 Java 编程语言中的一组 JAXB 内容类。编译器类`xjc`提供为：在 Solaris/Linux 上为`xjc.sh`，在 Windows 上为`xjc.bat`在 JAXB RI 捆绑包中。`xjc`类包含在 JDK 类库中（在 tools.jar 中）。

`xjc.sh`和`xjc.bat`都接受相同的命令行选项。您可以通过在没有任何选项或使用`-help`开关的情况下调用脚本来显示快速使用说明。语法如下：

```java
xjc [-options ...] <schema file/URL/dir/jar>... [-b >bindinfo<] ...

```

如果指定了`dir`，则将编译目录中的所有模式文件。如果指定了`jar`，将编译/META-INF/sun-jaxb.episode 绑定文件。

`xjc`命令行选项如下：

`-nv`

不执行输入模式或模式的严格验证。默认情况下，`xjc` 在处理之前会对源模式进行严格验证。请注意，这并不意味着绑定编译器不会执行任何验证；它只会执行较少严格的验证。

`-extension`

默认情况下，XJC 绑定编译器严格执行 JAXB 规范的兼容性章节中概述的规则。在默认（严格）模式下，您也只能使用规范中定义的绑定自定义。通过使用`-extension`开关，您可以使用 JAXB 供应商扩展。

`-b` *file*

指定一个或多个要处理的外部绑定文件。（每个绑定文件必须有自己的`-b`开关。）外部绑定文件的语法是灵活的。您可以有一个包含多个模式的自定义的单个绑定文件，或者您可以将自定义拆分为多个绑定文件。此外，命令行中模式文件和绑定文件的顺序无关紧要。

`-d` *dir*

默认情况下，XJC 绑定编译器在当前目录中生成 Java 内容类。使用此选项指定替代输出目录。目录必须已经存在；XJC 绑定编译器不会为您创建一个。

`-p` *package*

指定替代输出目录。默认情况下，XJC 绑定编译器将在当前目录中生成 Java 内容类。输出目录必须已经存在；XJC 绑定编译器不会为您创建一个。

`-httpproxy` *proxy*

指定 HTTP/HTTPS 代理。格式为`[``*user*``[:``*password*``]@]``*proxyHost*``[:``*proxyPort*``]`。旧的`-host`和`-port`选项仍受参考实现支持以保持向后兼容性，但它们已被弃用。

`-httpproxyfile` *f*

类似于`-httpproxy`选项，但将参数放在文件中以保护密码。

`-classpath` *arg*

指定用于`<jxb:javaType>`和`<xjc:superClass>`自定义的客户端应用程序类文件的位置。

`-catalog` *file*

指定目录文件以解析外部实体引用。支持 TR9401、XCatalog 和 OASIS XML Catalog 格式。有关更多信息，请参阅 XML 实体和 URI 解析器文档或检查 catalog-resolver 示例应用程序。

`-readOnly`

强制 XJC 绑定编译器将生成的 Java 源文件标记为只读。默认情况下，XJC 绑定编译器不会写保护生成的 Java 源文件。

`-npa`

抑制将包级别注释生成到`**/package-info.java`中。使用此开关会导致生成的代码将这些注释内部化到其他生成的类中。

`-no-header`

抑制生成带有时间戳的文件头。

`-target (2.0|2.1)`

行为类似于 XJC 2.0 或 2.1，并生成不使用任何 XJC 2.2 功能的代码。

`-enableIntrospection`

启用正确生成布尔值 getter/setter 以启用 Bean Introspection api。

`-contentForWildcard`

为具有多个 xs:any 派生元素的类型生成内容属性。

`-xmlschema`

将输入模式视为 W3C XML 模式（默认）。 如果不指定此开关，则将处理输入模式为 W3C XML 模式。

`-verbose`

在编译器输出中更加详细。

`-quiet`

抑制编译器输出，如进度信息和警告。

`-help`

显示编译器开关的简要摘要。

`-version`

显示编译器版本信息。

`-fullversion`

显示编译器的完整版本信息。

`-Xinject-code`

将指定的 Java 代码片段注入生成的代码中。

`-Xlocator`

启用生成代码的源位置支持。

`-Xsync-methods`

生成带有`synchronized`关键字的访问器方法。

`-mark-generated`

使用`-@javax.annotation.`生成的注释标记生成的代码。

`-episode *FILE*`

为单独编译生成剧集文件。

## JAXB 模式生成器选项

JAXB 模式生成器`schemagen`为您的 Java 类中引用的每个命名空间创建一个模式文件。 模式生成器可以通过使用适用于您平台的`bin`目录中的适当`schemagen` shell 脚本启动。 模式生成器仅处理 Java 源文件。 如果您的 Java 源引用其他类，则这些源必须从系统的 CLASSPATH 环境变量中访问； 否则，在生成模式时将出现错误。 无法控制生成的模式文件的名称。

通过调用脚本而不使用任何选项或使用`-help`选项，您可以显示快速使用说明。 语法如下：

```java
schemagen [-d *path*] 
    [*java-source-files*]

```

`-d` *path* 选项指定了处理器生成和`javac`生成的类文件的位置。

## 关于模式到 Java 绑定

当您针对基本示例中使用的`po.xsd` XML 模式运行 JAXB 绑定编译器（解组读取，修改编组，解组验证）时，JAXB 绑定编译器将生成一个名为`primer.po`的 Java 包，其中包含下表中描述的类。

表：基本示例中的基于模式的 JAXB 类

| 类 | 描述 |
| --- | --- |
| `primer/po/Items.java` | 公共接口，绑定到名为`Items`的`complexType`模式。 |
| `primer/po/ObjectFactory.java` | 公共类，扩展`com.sun.xml.bind.DefaultJAXBContextImpl`； 用于创建指定接口的实例。 例如，`ObjectFactory`的`createComment()`方法实例化`Comment`对象。 |
| `primer/po/PurchaseOrderType.java` | 公共接口，绑定到名为`PurchaseOrderType`的`complexType`模式。 |
| `primer/po/USAddress.java` | 公共接口，绑定到名为`USAddress`的`complexType`模式。 |

这些类及其与基本示例源 XML 模式的特定绑定在以下表中描述。

表：基本示例的模式到 Java 绑定

| XML Schema | JAXB Binding |
| --- | --- |

|

```java
<xsd:schema xmlns:xsd=
 "http://www.w3.org/2001/XMLSchema">

```

|   |
| --- |

|

```java
<xsd:complexType 
  name="PurchaseOrderType">
  <xsd:sequence>
    <xsd:element 
      name="shipTo" 
      type="USAddress"/>
    <xsd:element 
      name="billTo" 
      type="USAddress"/>
    <xsd:element 
      ref="comment" 
      minOccurs="0"/>
    <xsd:element 
      name="items"
      type="Items"/>
  </xsd:sequence>
  <xsd:attribute 
    name="orderDate"
    type="xsd:date"/>
</xsd:complexType>

```

| `PurchaseOrderType.java` |
| --- |

|

```java
<xsd:complexType 
  name="USAddress">
  <xsd:sequence>
    <xsd:element 
      name="name" 
      type="xsd:string"/>
    <xsd:element 
      name="street" 
      type="xsd:string"/>
    <xsd:element 
      name="city" 
      type="xsd:string"/>
    <xsd:element 
      name="state" 
      type="xsd:string"/>
    <xsd:element 
      name="zip" 
      type="xsd:decimal"/>
  </xsd:sequence>
  <xsd:attribute 
    name="country" 
    type="xsd:NMTOKEN" 
    fixed="US"/>
</xsd:complexType>

```

| `USAddress.java` |
| --- |

|

```java
<xsd:complexType 
  name="Items">
  <xsd:sequence>
    <xsd:element 
      name="item" 
      minOccurs="1" 
      maxOccurs="unbounded">

```

| `Items.java` |
| --- |

|

```java
<xsd:complexType>
  <xsd:sequence>
    <xsd:element 
      name="productName" 
      type="xsd:string"/>
    <xsd:element 
      name="quantity">
      <xsd:simpleType>
        <xsd:restriction 
          base="xsd:positiveInteger">
          <xsd:maxExclusive 
            value="100"/>
        </xsd:restriction>
      </xsd:simpleType>
    </xsd:element>
    <xsd:element 
      name="USPrice" 
      type="xsd:decimal"/>
    <xsd:element 
      ref="comment" 
      minOccurs="0"/>
    <xsd:element 
      name="shipDate" 
      type="xsd:date" 
      minOccurs="0"/>
  </xsd:sequence>
  <xsd:attribute 
    name="partNum" 
    type="SKU" 
    use="required"/>
</xsd:complexType>

```

| `Items.ItemType` |
| --- |

|

```java
</xsd:element>
</xsd:sequence>
</xsd:complexType>

```

|   |
| --- |

|

```java
<!-- Stock Keeping Unit, a code for 
    identifying products -->

```

|   |
| --- |

|

```java
<xsd:simpleType 
  name="SKU">
  <xsd:restriction 
    base="xsd:string">
    <xsd:pattern 
      value="\d{3}-[A-Z]{2}"/>
  </xsd:restriction>
</xsd:simpleType>

```

|   |
| --- |

|

```java
</xsd:schema>

```

|   |
| --- |

## 基于模式的 JAXB 类

接下来的部分简要解释了 JAXB 绑定编译器为示例生成的以下各个单独类的功能：

+   `Items` 类

+   `ObjectFactory` 类

+   `PurchaseOrderType` 类

+   `USAddress` 类

### `Items` 类

在 `Items.java` 中：

+   `Items` 类是 `primer.po` 包的一部分。

+   该类为 `Items` 和 `ItemType` 提供了公共接口。

+   此类的实例化内容绑定到 XML ComplexTypes `Items` 及其子元素 `ItemType`。

+   `Item` 提供了 `getItem()` 方法。

+   `ItemType` 提供了以下方法：

    +   `getPartNum();`

    +   `setPartNum(String value);`

    +   `getComment();`

    +   `setComment(java.lang.String value);`

    +   `getUSPrice();`

    +   `setUSPrice(java.math.BigDecimal value);`

    +   `getProductName();`

    +   `setProductName(String value);`

    +   `getShipDate();`

    +   `setShipDate(java.util.Calendar value);`

    +   `getQuantity();`

    +   `setQuantity(java.math.BigInteger value);`

### `ObjectFactory` 类

在 `ObjectFactory.java` 中：

+   `ObjectFactory` 类是 `primer.po` 包的一部分。

+   `ObjectFactory` 提供了用于在 Java 内容树中实例化表示 XML 内容的 Java 接口的工厂方法。

+   方法名是通过连接生成的：

    +   字符串常量 `create`。

    +   所有外部 Java 类名称，如果 Java 内容接口嵌套在另一个接口中。

    +   Java 内容接口的名称。

例如，在这种情况下，对于 Java 接口 `primer.po.Items.ItemType`，`ObjectFactory` 创建了方法 `createItemsItemType()`。

### `PurchaseOrderType` 类

在 `PurchaseOrderType.java` 中：

+   `PurchaseOrderType` 类是 `primer.po` 包的一部分。

+   此类的实例化内容绑定到名为 `PurchaseOrderType` 的 XML 模式子元素。

+   `PurchaseOrderType` 是一个公共接口，提供以下方法：

    +   `getItems();`

    +   `setItems(primer.po.Items value);`

    +   `getOrderDate();`

    +   `setOrderDate(java.util.Calendar value);`

    +   `getComment();`

    +   `setComment(java.lang.String value);`

    +   `getBillTo();`

    +   `setBillTo(primer.po.USAddress value);`

    +   `getShipTo();`

    +   `setShipTo(primer.po.USAddress value);`

### `USAddress` 类

在 `USAddress.java` 中：

+   `USAddress` 类是 `primer.po` 包的一部分。

+   此类的实例化内容绑定到名为 `USAddress` 的 XML 模式元素。

+   `USAddress` 是一个公共接口，提供以下方法：

    +   `getState();`

    +   `setState(String value);`

    +   `getZip();`

    +   `setZip(java.math.BigDecimal value);`

    +   `getCountry();`

    +   `setCountry(String value);`

    +   `getCity();`

    +   `setCity(String value);`

    +   `getStreet();`

    +   `setStreet(String value);`

    +   `getName();`

    +   `setName(String value);`
