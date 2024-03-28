# 基本示例

> 原文：[`docs.oracle.com/javase/tutorial/jaxb/intro/basic.html`](https://docs.oracle.com/javase/tutorial/jaxb/intro/basic.html)

本节描述了基本的 JAXB 示例（修改 Marshal、Unmarshal Validate），演示了如何：

+   将 XML 文档解组为 Java 内容树，并访问其中包含的数据。

+   修改 Java 内容树。

+   使用 `ObjectFactory` 类创建 Java 内容树，然后将其编组为 XML 数据。

+   在解组期间执行验证。

+   在运行时验证 Java 内容树。

## 修改 Marshal 示例

修改 Marshal 示例演示了如何修改 Java 内容树。

1.  *jaxb-ri-install*`/samples/modify-marshal/src/Main.java` 类声明导入了三个标准 Java 类，五个 JAXB 绑定框架类和 `primer.po` 包：

    ```java
    import java.io.FileInputStream;
    import java.io.IOException;
    import java.math.BigDecimal;

    import javax.xml.bind.JAXBContext;
    import javax.xml.bind.JAXBElement;
    import javax.xml.bind.JAXBException;
    import javax.xml.bind.Marshaller;
    import javax.xml.bind.Unmarshaller;

    import primer.po.*;

    ```

1.  创建一个用于处理在 `primer.po` 中生成的类的 `JAXBContext` 实例。

    ```java

    JAXBContext jc = JAXBContext.newInstance( "primer.po" );

    ```

1.  创建一个 `Unmarshaller` 实例，并解组 `po.xml` 文件。

    ```java
    Unmarshaller u = jc.createUnmarshaller();
    PurchaseOrder po = (PurchaseOrder)
        u.unmarshal(new FileInputStream("po.xml"));

    ```

1.  使用 `set` 方法修改内容树中 `address` 分支中的信息。

    ```java
    USAddress address = po.getBillTo();
    address.setName("John Bob");
    address.setStreet("242 Main Street");
    address.setCity("Beverly Hills");
    address.setState("CA");
    address.setZip(new BigDecimal
    address.setName("John Bob");
    address.setStreet("242 Main Street");
    address.setCity("Beverly Hills");
    address.setState("CA");
    address.setZip(new BigDecimal("90210"));

    ```

1.  创建一个 `Marshaller` 实例，并将更新后的 XML 内容编组到 `system.out`。使用 `setProperty` API 指定输出编码；在本例中为格式化（易读）的 XML。

    ```java
    Marshaller m = jc.createMarshaller();
    m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.TRUE);
    m.marshal(po, System.out);

    ```

### 使用 Ant 构建和运行修改 Marshal 示例

要使用 Ant 编译和运行修改 Marshal 示例，在终端窗口中，转到 *jaxb-ri-install*`/samples/modify-marshal/` 目录，并输入以下内容：

```java
ant

```

## Unmarshal Validate 示例

Unmarshal Validate 示例演示了如何在解组期间启用验证。请注意，JAXB 提供了在解组期间进行验证但不在编组期间进行验证的功能。有关验证的更多详细信息，请参阅更多关于验证。

1.  *jaxb-ri-install*`/samples/unmarshal-validate/src/Main.java` 类声明导入了一个标准 Java 类，十一个 JAXB 绑定框架类和 `primer.po` 包：

    ```java
    import java.io.File;

    import javax.xml.bind.JAXBContext;
    import javax.xml.bind.JAXBException;
    import javax.xml.bind.Marshaller;
    import javax.xml.bind.UnmarshalException;
    import javax.xml.bind.Unmarshaller;
    import javax.xml.bind.ValidationEvent;
    import javax.xml.bind.ValidationEventHandler;
    import javax.xml.bind.ValidationEventLocator;

    import static javax.xml.XMLConstants.W3C_XML_SCHEMA_NS_URI;
    import javax.xml.validation.SchemaFactory;
    import javax.xml.validation.Schema;

    import primer.po.*;

    ```

1.  创建一个用于处理在 `primer.po` 包中生成的类的 `JAXBContext` 实例。

    ```java
    JAXBContext jc = JAXBContext.newInstance("primer.po");

    ```

1.  创建一个 `Unmarshaller` 实例。

    ```java
    Unmarshaller u = jc.createUnmarshaller();

    ```

1.  默认的 JAXB `Unmarshaller` `ValidationEventHandler` 已启用，以将验证警告和错误发送到 `system.out`。默认配置导致在遇到第一个验证错误时解组操作失败。

    ```java
    u.setValidating( true );

    ```

1.  尝试将 `po.xml` 文件解组为 Java 内容树。在此示例中，`po.xml` 文件包含一个故意的错误。

    ```java
    PurchaseOrder po = (PurchaseOrder)u.unmarshal(
        new FileInputStream("po.xml"));

    ```

1.  默认验证事件处理程序处理验证错误，生成输出到 `system.out`，然后抛出异常。

    ```java
    } catch( UnmarshalException ue ) {
        System.out.println("Caught UnmarshalException");
    } catch( JAXBException je ) { 
        je.printStackTrace();
    } catch( IOException ioe ) {
        ioe.printStackTrace();
    }

    ```

### 使用 Ant 构建和运行 Unmarshal Validate 示例。

要使用 Ant 编译和运行 Unmarshal Validate 示例，在终端窗口中，转到 *jaxb-ri-install*`/samples/unmarshal-validate/` 目录，并输入以下内容：

```java
ant 

```
