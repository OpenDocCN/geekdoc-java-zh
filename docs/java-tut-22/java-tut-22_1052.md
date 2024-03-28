# Java-to-Schema 示例

> 原文：[`docs.oracle.com/javase/tutorial/jaxb/intro/j2schema.html`](https://docs.oracle.com/javase/tutorial/jaxb/intro/j2schema.html)

Java-to-Schema 示例展示了如何使用注解将 Java 类映射到 XML 模式。

## j2s-create-marshal 示例

j2s-create-marshal 示例演示了 Java-to-schema 数据绑定。它演示了对带有 JAXB 注解的类进行编组和解组，并展示了如何在解组时使用从 JAXB 映射类生成的模式文件启用 JAXP 1.3 验证。

schema 文件`bc.xsd`是通过以下命令生成的：

```java
schemagen src/cardfile/*.java
cp schema1.xsd bc.xsd

```

请注意，`schema1.xsd`被复制到`bc.xsd`；`schemagen`不允许您指定自己选择的模式名称。

### 使用 Ant 构建和运行 j2s-create-marshal 示例

要使用 Ant 编译和运行 j2s-create-marshal 示例，在终端窗口中，转到*jaxb-ri-install*/samples/j2s-create-marshal/目录并输入以下内容：

```java
ant 

```

## j2s-xmlAccessorOrder 示例

j2s-xmlAccessorOrder 示例展示了如何使用`@XmlAccessorOrder`和`@XmlType.propOrder`注解来指定 Java 类型在编组和解组时的 XML 内容顺序。

使用 Java-to-schema 映射，JavaBean 的属性和字段被映射到 XML 模式类型。类元素被映射为 XML 模式复杂类型或 XML 模式简单类型。生成的模式类型的默认元素顺序目前未指定，因为 Java 反射不强制返回顺序。可靠的元素排序的缺失对应用程序的可移植性产生负面影响。您可以使用两个注解`@XmlAccessorOrder`和`@XmlType.propOrder`，为必须跨 JAXB 提供程序可移植的应用程序定义模式元素排序。

### 使用`@XmlAccessorOrder`注解定义模式元素排序

`@XmlAccessorOrder`注解强制执行两种元素排序算法，`AccessorOrder.UNDEFINED`或`AccessorOrder.ALPHABETICAL`。`AccessorOrder.UNDEFINED`是默认设置。顺序取决于系统的反射实现。`AccessorOrder.ALPHABETICAL`算法按`java.lang.String.CompareTo(String anotherString)`确定的字典顺序对元素进行排序。

您可以为注解类型`ElementType.PACKAGE`的类对象定义`@XmlAccessorOrder`注解。当`@XmlAccessorOrder`注解定义在包上时，格式规则的范围对包中的每个类都有效。当定义在类上时，规则对该类的内容有效。

一个包中可以有多个 `@XmlAccessorOrder` 注解。最内层（类）注解优先于外部注解。例如，如果在一个包中定义了 `@XmlAccessorOrder(AccessorOrder.ALPHABETICAL)`，并且在该包中的一个类上定义了 `@XmlAccessorOrder(AccessorOrder.UNDEFINED)`，则该类的生成的模式类型的内容将以未指定的顺序排列，而该包中的其他每个类的生成的模式类型的内容将按字母顺序排列。

### 使用 `@XmlType` 注解定义模式元素顺序

`@XmlType` 注解可以定义在一个类上。`@XmlType` 注解中的 `propOrder()` 元素使您能够指定生成的模式类型中的内容顺序。当您在一个类上使用 `@XmlType.propOrder` 注解来指定内容顺序时，类中的所有公共属性和公共字段必须在参数列表中指定。您希望保留在参数列表之外的任何公共属性或字段必须用 `@XmlAttribute` 或 `@XmlTransient` 注解进行标注。

`@XmlType.propOrder` 的默认内容顺序为 `{}` 或 `{""}`，不活动。在这种情况下，活动的 `@XmlAccessorOrder` 注解优先。当类内容顺序由 `@XmlType.propOrder` 注解指定时，它优先于类或包上的任何活动的 `@XmlAccessorOrder` 注解。如果在一个类上指定了 `@XmlAccessorOrder` 和 `@XmlType.propOrder(A, B, ...)` 注解，那么 `propOrder` 总是优先，不管注解语句的顺序如何。例如，在以下代码段中，`@XmlAccessorOrder` 注解在 `@XmlType.propOrder` 注解之前。

```java
@XmlAccessorOrder(AccessorOrder.ALPHABETICAL)
@XmlType(propOrder={"name", "city"})

public class USAddress {
    // ...
    public String getCity() {return city;}
    public void setCity(String city) {this.city = city;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    // ...
}

```

在以下代码段中，`@XmlType.propOrder` 注解在 `@XmlAccessorOrder` 注解之前。

```java
@XmlType(propOrder={"name", "city"})
@XmlAccessorOrder(AccessorOrder.ALPHABETICAL)
public class USAddress {
    // ...
    public String getCity() {return city;}
    public void setCity(String city) {this.city = city;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    // ...
}

```

在两种情况下，`propOrder` 优先，并生成以下相同的模式内容：

```java
<xs:complexType name="usAddress">
    <xs:sequence>
        <xs:element 
            name="name" 
            type="xs:string" 
            minOccurs="0"/>
        <xs:element 
            name="city" 
            type="xs:string" 
            minOccurs="0"/>
    </xs:sequence>
</xs:complexType>

```

### 示例中的模式内容排序

采购订单代码示例演示了使用包和类级别的 `@XmlAccessorOrder` 注解以及在类上使用 `@XmlType.propOrder` 注解来定义模式内容顺序的效果。

类 `package-info.java` 定义了包中 `@XmlAccessorOrder` 为 `ALPHABETICAL`。类 `PurchaseOrderType` 中的公共字段 `shipTo` 和 `billTo` 受此规则影响，生成的模式内容顺序由此规则确定。类 `USAddress` 在类上定义了 `@XmlType.propOrder` 注解，演示了用户定义的属性顺序优先于生成的模式中的 `ALPHABETICAL` 顺序。

生成的模式文件可以在 *jaxb-ri-install*`/samples/j2s-xmlAccessorOrder/build/schemas/` 目录中找到。

### 使用 Ant 构建和运行 j2s-xmlAccessorOrder 示例

要使用 Ant 编译和运行 j2s-xmlAccessorOrder 示例，在终端窗口中，转到 *jaxb-ri-install*`/samples/j2s-xmlAccessorOrder/` 目录，并输入以下内容：

```java
ant 

```

## j2s-xmlAdapter 示例

j2s-xmlAdapter 示例演示了如何使用`XmlAdapter`接口和`@XmlJavaTypeAdapter`注解为使用`int`作为键和`String`作为值的`HashMap`（字段）提供自定义映射的 XML 内容的组合和解组。

接口`XmlAdapter`和注解`@XmlJavaTypeAdapter`用于在解组和组合期间对数据类型进行特殊处理。有各种 XML 数据类型，其表示方式不容易映射到 Java（例如，`xs:DateTime`和`xs:Duration`），以及 Java 类型不正确地映射到 XML 表示。例如，`java.util.Collection`（如`List`）和`java.util.Map`（如`HashMap`）的实现，或非 JavaBean 类。

为这类情况提供了`XmlAdapter`接口和`@XmlJavaTypeAdapter`注解。这种组合提供了一个可移植的机制，用于将 XML 内容读取和写入 Java 应用程序。

`XmlAdapter`接口定义了数据读取和写入的方法。

```java
/*
 *  ValueType - Java class that provides an 
 *  XML representation of the data. 
 *  It is the object that is used for marshalling and 
 *  unmarshalling.
 *
 *  BoundType - Java class that is used to 
 *  process XML content.
 */

public abstract class XmlAdapter<ValueType,BoundType> {

    // Do-nothing constructor for the derived classes.
    protected XmlAdapter() {}

    // Convert a value type to a bound type.
    public abstract BoundType unmarshal(ValueType v);

    // Convert a bound type to a value type.
    public abstract ValueType marshal(BoundType v);
}

```

您可以使用`@XmlJavaTypeAdapter`注解将特定的`XmlAdapter`实现与`Target`类型`PACKAGE`、`FIELD`、`METHOD`、`TYPE`或`PARAMETER`关联。

j2s-xmlAdapter 示例展示了如何使用`XmlAdapter`将 XML 内容映射到（自定义）`HashMap`中并从中组合出来。类`KitchenWorldBasket`中的`HashMap`对象`basket`使用`int`类型的键和`String`类型的值。这些数据类型应该反映在读取和写入的 XML 内容中，因此 XML 内容应如下示例所示：

```java
<basket>
    <entry key="9027">glasstop stove in black</entry>
    <entry key="288">wooden spoon</entry>
</basket>

```

为 Java 类型`HashMap`生成的默认模式不反映所需的格式。

```java
<xs:element name="basket">
    <xs:complexType>
        <xs:sequence>
            <xs:element 
                name="entry" 
                minOccurs="0" 
                maxOccurs="unbounded">
                <xs:complexType>
                    <xs:sequence>
                        <xs:element 
                            name="key" 
                            minOccurs="0"
                            type="xs:anyType"/>
                        <xs:element 
                            name="value" 
                            minOccurs="0" 
                            type="xs:anyType"/>
                    </xs:sequence>
                </xs:complexType>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
</xs:element>

```

在默认的`HashMap`模式中，键和值都是元素，并且是`anyType`数据类型。XML 内容如下所示：

```java
<basket>
    <entry>
        <key>9027</key>
        <value>glasstop stove in black</value>
    </entry>
    <entry>
        <key>288</key>
        <value>wooden spoon</value>
    </entry>
</basket>

```

要解决这个问题，示例使用了两个 Java 类，`PurchaseList`和`PartEntry`，它们反映了用于解组和组合内容的所需模式格式。为这些类生成的 XML 模式如下：

```java
<xs:complexType name="PurchaseListType">
    <xs:sequence>
        <xs:element 
            name="entry" 
            type="partEntry"
            nillable="true" 
            maxOccurs="unbounded"
            minOccurs="0"/>
    </xs:sequence>
</xs:complexType>

<xs:complexType name="partEntry">
    <xs:simpleContent>
        <xs:extension base="xs:string">
            <xs:attribute
                name="key" 
                type="xs:int"
                use="required"/>
        </xs:extension>
    </xs:simpleContent>
</xs:complexType>

```

类`AdapterPurchaseListToHashMap`实现了`XmlAdapter`接口。在类`KitchenWorldBasket`中，使用`@XmlJavaTypeAdapter`注解将`AdapterPurchaseListToHashMap`与字段`HashMap` `basket`配对。这种配对导致在`KitchenWorldBasket`上进行任何对应的组合或解组操作时，将调用`AdapterPurchaseListToHashMap`的组合或解组方法。

### 使用 Ant 构建和运行 j2s-xmlAdapter 示例

要使用 Ant 编译和运行 j2s-xmlAdapter 示例，在终端窗口中，转到*jaxb-ri-install*`/samples/j2s-xmlAdapter/`目录，然后输入以下内容：

```java
ant

```

## j2s-xmlAttribute 示例

j2s-xmlAttribute 示例展示了如何使用`@XmlAttribute`注解来定义一个属性或字段被视为 XML 属性。

`@XmlAttribute`注解将字段或 JavaBean 属性映射到 XML 属性。强加以下规则：

+   静态最终字段映射到 XML 固定属性。

+   当字段或属性是集合类型时，集合类型的项目必须映射到模式简单类型。

+   当字段或属性不是集合类型时，类型必须映射到模式简单类型。

遵循 JavaBean 编程范式时，属性由字段名上的`get`和`set`前缀定义。

```java
int zip;
public int getZip(){return zip;}
public void setZip(int z){zip=z;}

```

在 bean 类中，您可以选择在三个组件之一上设置`@XmlAttribute`注解：字段、setter 方法或 getter 方法。如果在字段上设置`@XmlAttribute`注解，则必须重命名 setter 方法，否则将在编译时出现命名冲突。如果在其中一个方法上设置`@XmlAttribute`注解，则必须在 setter 或 getter 方法上设置，但不能同时设置在两者上。

XmlAttribute 示例展示了如何在静态最终字段上使用`@XmlAttribute`注解，在字段而不是相应的 bean 方法上使用，在 bean 属性（方法）上使用，以及在不是集合类型的字段上使用。在类`USAddress`中，字段 country 和 zip 被标记为属性。`setZip`方法被禁用以避免编译错误。属性 state 在 setter 方法上被标记为属性。您也可以使用 getter 方法。在类`PurchaseOrderType`中，字段`cCardVendor`是非集合类型。它符合简单类型的要求；它是一个`enum`类型。

### 使用 Ant 构建和运行 j2s-xmlAttribute 示例

要使用 Ant 编译和运行 j2s-xmlAttribute 示例，在终端窗口中，转到*jaxb-ri-install*`/samples/j2s-xmlAttribute/`目录并键入以下内容：

```java
ant

```

## j2s-xmlRootElement 示例

j2s-xmlRootElement 示例演示了如何使用`@XmlRootElement`注解为相应类的 XML 模式类型定义 XML 元素名称。

`@XmlRootElement`注解将类或`enum`类型映射到 XML 元素。每个用于解组和组装的顶级 Java 类型都需要至少一个元素定义。如果没有元素定义，XML 内容处理就没有起始位置。

`@XmlRootElement`注解使用类名作为默认元素名。您可以通过使用注解属性`name`来更改默认名称。如果这样做，指定的名称将用作元素名和类型名。元素和类型名称不同是常见的模式实践。您可以使用`@XmlType`注解来设置元素类型名称。

`@XmlRootElement`注解的命名空间属性用于为元素定义命名空间。

### 使用 Ant 构建和运行 j2s-xmlRootElement 示例

要使用 Ant 编译和运行 j2s-xmlRootElement 示例，在终端窗口中，转到*jaxb-ri-install*`/samples/j2s-xmlRootElement/`目录并输入以下内容：

```java
ant

```

## j2s-xmlSchemaType 示例

j2s-xmlSchemaType 示例演示了如何使用注解`@XmlSchemaType`自定义将属性或字段映射到 XML 内置类型的映射。

`@XmlSchemaType`注解可用于将 Java 类型映射到 XML 内置类型之一。此注解在将 Java 类型映射到九种日期/时间原始数据类型之一时最有用。

当在包级别定义`@XmlSchemaType`注解时，标识需要同时包含 XML 内置类型名称和相应的 Java 类型类。在字段或属性上的`@XmlSchemaType`定义优先于包定义。

XmlSchemaType 类示例展示了如何在包级别、字段和属性上使用`@XmlSchemaType`注解。文件`TrackingOrder`有两个字段，`orderDate`和`deliveryDate`，它们被定义为`XMLGregorianCalendar`类型。生成的模式将定义这些元素为 XML 内置类型`gMonthDay`。此关系在文件`package-info.java`中在包中定义。文件`TrackingOrder`中的`shipDate`字段也被定义为`XMLGregorianCalendar`类型，但`@XmlSchemaType`注解语句覆盖了包定义，并指定该字段为`date`类型。属性方法`getTrackingDuration`定义模式元素被定义为原始类型`duration`而不是 Java 类型`String`。

### 使用 Ant 构建和运行 j2s-xmlSchemaType 示例

要使用 Ant 编译和运行 j2s-xmlSchemaType 示例，在终端窗口中，转到*jaxb-ri-install*`/samples/j2s-xmlSchemaType/`目录并输入以下内容：

```java
ant 

```

## j2s-xmlType 示例

j2s-xmlType 示例演示了如何使用`@XmlType`注解。`@XmlType`注解将一个类或一个`enum`类型映射到一个 XML Schema 类型。

一个类必须具有公共零参数构造函数或静态零参数工厂方法，以便通过此注解进行映射。在解组期间，其中一个方法用于创建类的实例。工厂方法可以位于工厂类中或现有类中。

有一个关于解组使用哪种方法的优先顺序：

+   如果注解中标识了工厂类，则该类中还必须标识相应的工厂方法，并且该方法将被使用。

+   如果注解中标识了工厂方法但未标识工厂类，则工厂方法必须位于当前类中。即使存在公共零参数构造方法，也将使用工厂方法。

+   如果注解中未标识工厂方法，则类必须包含一个公共零参数构造方法。

在这个例子中，一个工厂类为几个类提供了零参数的工厂方法。类`OrderContext`上的`@XmlType`注解引用了工厂类。解组器在这个类中使用了标识的工厂方法。

```java
public class OrderFormsFactory {

    public OrderContext newOrderInstance() {
        return new OrderContext()
    }

    public PurchaseOrderType {
        newPurchaseOrderType() {
            return new newPurchaseOrderType();
        }
    }

    @XmlType(name="oContext",
        factoryClass="OrderFormsFactory",
        factoryMethod="newOrderInstance")
    public class OrderContext {
        public OrderContext() {
            // ...
        }
    }
}

```

在这个例子中，一个工厂方法在一个类中被定义，该类还包含一个标准的类构造。因为`factoryMethod`的值被定义了，而没有定义`factoryClass`，所以在解组时会使用工厂方法`newOrderInstance`。

```java
@XmlType(name="oContext", 
    factoryMethod="newOrderInstance")
public class OrderContext {

    public OrderContext() {
        // ...
    }

    public OrderContext newOrderInstance() {
        return new OrderContext();
    }
}

```

### 使用 Ant 构建和运行 j2s-xmlType 示例

要使用 Ant 编译和运行 j2s-xmlType 示例，在终端窗口中，进入*jaxb-ri-install*/samples/j2s-xmlType/目录，并输入以下内容：

```java
ant

```
