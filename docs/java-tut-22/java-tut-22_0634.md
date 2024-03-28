# 安装自定义资源包作为扩展

> 原文：[`docs.oracle.com/javase/tutorial/i18n/serviceproviders/resourcebundlecontrolprovider.html`](https://docs.oracle.com/javase/tutorial/i18n/serviceproviders/resourcebundlecontrolprovider.html)

自定义资源包加载部分向您展示如何更改资源包的加载方式。这涉及从类[`ResourceBundle.Control`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.Control.html)派生一个新类，然后通过调用以下方法检索资源包：

```java
ResourceBundle getBundle(
  String baseName,
  Locale targetLocale,
  ResourceBundle.Control control)

```

参数`control`是您对`ResourceBundle.Control`的实现。

[`java.util.spi.ResourceBundleControlProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/ResourceBundleControlProvider.html)接口使您能够更改以下方法加载资源包的方式：

```java
ResourceBundle getBundle(
  String baseName,
  Locale targetLocale)

```

请注意，此版本的[`ResourceBundle.getBundle`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-java.util.Locale-)方法不需要`ResourceBundle.Control`类的实例。`ResourceBundleControlProvider`是一个服务提供者接口（SPI）。SPI 使您能够创建可扩展的应用程序，即可以在不修改其原始代码库的情况下轻松扩展的应用程序。有关更多信息，请参阅创建可扩展应用程序。

要使用 SPI，首先通过实现类似`ResourceBundleControlProvider`的 SPI 来创建一个服务提供者。当您实现一个 SPI 时，您指定它将如何提供服务。`ResourceBundleControlProvider` SPI 提供的服务是在应用程序调用方法`ResourceBundle.getBundle(String baseName, Locale targetLocale)`时获取适当的`ResourceBundle.Control`实例。您将服务提供者与 Java 扩展机制一起打包为已安装的扩展。运行应用程序时，不需要在类路径中命名您的扩展；运行时环境会找到并加载这些扩展。

已安装的`ResourceBundleControlProvider` SPI 实现将替换默认的`ResourceBundle.Control`类（定义了默认的包加载过程）。因此，`ResourceBundleControlProvider`接口使您可以使用任何自定义的`ResourceBundle.Control`类，而无需对应用程序的源代码进行任何其他更改。此外，该接口使您能够编写应用程序，而无需引用任何自定义的`ResourceBundle.Control`类。

``RBCPTest.java``示例展示了如何实现`ResourceBundleControlProvider`接口以及如何将其打包为已安装的扩展。这个示例被打包在 zip 文件`RBCPTest.zip`中，包含以下文件：

+   `src`

    +   ``java.util.spi.ResourceBundleControlProvider``

    +   ``RBCPTest.java``

    +   `rbcp`

        +   ``PropertiesResourceBundleControl.java``

        +   ``PropertiesResourceBundleControlProvider.java``

        +   ``XMLResourceBundleControl.java``

        +   ``XMLResourceBundleControlProvider.java``

    +   `resources`

        +   ``RBControl.properties``

        +   ``RBControl_zh.properties``

        +   ``RBControl_zh_CN.properties``

        +   ``RBControl_zh_HK.properties``

        +   ``RBControl_zh_TW.properties``

        +   ``XmlRB.xml``

        +   ``XmlRB_ja.xml``

+   `lib`

    +   `rbcontrolprovider.jar`

+   `build`：包含所有打包在`rbcontrolprovider.jar`中的文件以及类文件`RBCPTest.class`

+   ``build.xml``

以下步骤展示了如何重新创建文件`RBCPTest.zip`的内容，`RBCPTest`示例的工作原理以及如何运行它：

1.  创建 ResourceBundle.Control 类的实现。

1.  实现 ResourceBundleControlProvider 接口。

1.  在应用程序中调用 ResourceBundle.getBundle 方法。

1.  通过创建配置文件注册服务提供者。

1.  将提供者、其所需的类以及配置文件打包到一个 JAR 文件中。

1.  运行 RBCPTest 程序。

## 1\. 创建 ResourceBundle.Control 类的实现。

``RBCPTest.java`` 示例使用了两种`ResourseBundle.Control`的实现：

+   ``PropertiesResourceBundleControlProvider.java``：这是在自定义资源包加载中定义的相同`ResourceBundle.Control`实现。

+   ``XMLResourceBundleControl.java``：这个`ResourceBundle.Control`实现使用[`Properties.loadFromXML`](https://docs.oracle.com/javase/8/docs/api/java/util/Properties.html#loadFromXML-java.io.InputStream-)方法加载基于 XML 的包。

### XML 属性文件

如在 使用属性文件支持 ResourceBundle 部分所述，属性文件是简单的文本文件。它们每行包含一个键值对。XML 属性文件与属性文件类似：它们包含键值对，只是具有 XML 结构。以下是 XML 属性文件 `XmlRB.xml`：

```java
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE properties [
<!ELEMENT properties ( comment?, entry* ) >
<!ATTLIST properties version CDATA #FIXED "1.0">
<!ELEMENT comment (#PCDATA) >
<!ELEMENT entry (#PCDATA) >
<!ATTLIST entry key CDATA #REQUIRED>
]>

<properties>
    <comment>Test data for RBCPTest.java</comment>
    <entry key="type">XML</entry>
</properties>

```

以下是等效的属性文本文件：

```java
# Test data for RBCPTest.java
type = XML

```

所有 XML 属性文本文件具有相同的结构：

+   指定文档类型定义（DTD）的 DOCTYPE 声明：DTD 定义了 XML 文件的结构。**注意**：在 XML 属性文件中，你可以使用以下 DOCTYPE 声明：

    ```java
    <!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">  

    ```

    系统 URI (`http://java.sun.com/dtd/properties.dtd`) 在导出或导入属性时不会被访问；它是一个唯一标识 XML 属性文件的 DTD 的字符串。

+   根元素 `<properties>`：此元素包含所有其他元素。

+   任意数量的 `<comment>` 元素：用于注释。

+   任意数量的 `<entry>` 元素：使用属性 `key` 指定键；在 `<entry>` 标签之间指定键的值。

查看 [`Properties`](https://docs.oracle.com/javase/8/docs/api/java/util/Properties.html) 类以获取有关 XML 属性文件的更多信息。

## 2\. 实现 ResourceBundleControlProvider 接口。

这个接口包含一个方法，[`ResourceBundle.Control getControl(String baseName)`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/ResourceBundleControlProvider.html#getControl-java.lang.String-) 方法。参数 `baseName` 是资源包的名称。在 `getBundle` 方法的定义中，指定应返回的 `ResourceBundle.Control` 实例，给定资源包的名称。

`RBCPTest` 示例包含 `ResourceBundleControlProvider` 接口的两个实现，``PropertiesResourceBundleControlProvider.java`` 和 ``XMLResourceBundleControlProvider.java``。如果资源包的基本名称以 `resources.RBControl` 开头，则方法 `PropertiesResourceBundleControlProvider.getBundle` 返回 `PropertiesResourceBundleControl` 的实例（在此示例中，所有资源文件都包含在 `resources` 包中）：

```java

package rbcp;

import java.util.ResourceBundle;
import java.util.spi.ResourceBundleControlProvider;

public class PropertiesResourceBundleControlProvider
    implements ResourceBundleControlProvider {
    static final ResourceBundle.Control PROPERTIESCONTROL =
        new PropertiesResourceBundleControl();

    public ResourceBundle.Control getControl(String baseName) {
        System.out.println("Class: " + getClass().getName() + ".getControl");
        System.out.println("    called for " + baseName);

        // Throws a NPE if baseName is null.
        if (baseName.startsWith("resources.RBControl")) {
            System.out.println("    returns " + PROPERTIESCONTROL);
            return PROPERTIESCONTROL;
        }
        System.out.println("    returns null");
        System.out.println();
        return null;
    }
}

```

类似地，如果资源包的基本名称以 `resources.Xml` 开头，则方法 `XMLResourceBundleControlProvider.getControl` 返回 `XMLResourceBundleControl` 的实例。

**注意**：你可以创建一个实现 `ResourceBundleControlProvider` 接口的实现，根据基本名称返回 `PropertiesResourceBundleControl` 或 `XMLResourceBundleControl` 的实例。

## 3\. 在你的应用程序中，调用方法 ResourceBundle.getBundle。

类`RBCPTest`使用方法[`ResourceBundle.getBundle`](https://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-java.util.Locale-)检索资源包：

```java

import java.io.*;
import java.net.*;
import java.util.*;

public class RBCPTest {
    public static void main(String[] args) {
        ResourceBundle rb = ResourceBundle.getBundle(
            "resources.XmlRB", Locale.ROOT);
        String type = rb.getString("type");
        System.out.println("Root locale. Key, type: " + type);
        System.out.println();

        rb = ResourceBundle.getBundle("resources.XmlRB", Locale.JAPAN);
        type = rb.getString("type");
        System.out.println("Japan locale. Key, type: " + type);
        System.out.println();

        test(Locale.CHINA);
        test(new Locale("zh", "HK"));
        test(Locale.TAIWAN);
        test(Locale.CANADA);
    }

    private static void test(Locale locale) {
        ResourceBundle rb = ResourceBundle.getBundle(
            "resources.RBControl", locale);
        System.out.println("locale: " + locale);
        System.out.println("    region: " + rb.getString("region"));
        System.out.println("    language: " + rb.getString("language"));
        System.out.println();
    }
}

```

请注意，此类中没有`ResourceBundle.Control`或`ResourceBundleControlProvider`的实现。因为`ResourceBundleControlProvider`接口使用 Java 扩展机制，运行时环境会找到并加载这些实现。但是，使用 Java 扩展机制安装的`ResourceBundleControlProvider`实现和其他服务提供程序是使用[`ServiceLoader`](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html)类加载的。使用此类意味着您必须使用配置文件注册服务提供程序，下一步将对此进行描述。

## 4\. 通过创建配置文件注册服务提供程序。

配置文件的名称是提供程序实现的接口或类的完全限定名称。配置文件包含您的提供程序的完全限定类名。文件``java.util.spi.ResourceBundleControlProvider``包含`PropertiesResourceBundleControlProvider`和`XMLResourceBundleControlProvider`的完全限定名称，每行一个名称：

```java
rbcp.XMLResourceBundleControlProvider
rbcp.PropertiesResourceBundleControlProvider

```

## 5\. 将提供程序、其所需类和配置文件打包到 JAR 文件中。

编译源文件。从包含`build.xml`文件的目录中，运行以下命令：

```java
javac -d build src/java.* src/rbcp/*.java
```

此命令将编译`src`目录中包含的源文件，并将类文件放在`build`目录中。在 Windows 上，请确保使用反斜杠（`\`）来分隔目录和文件名。

创建一个 JAR 文件，其中包含编译的类文件、资源文件和配置文件，目录结构如下：

+   `META-INF`

    +   `services`

        +   `java.util.spi.ResourceBundleControlProvider`

+   `rbcp`

    +   `PropertiesResourceBundleControl.class`

    +   `PropertiesResourceBundleControlProvider.class`

    +   `XMLResourceBundleControl.class`

    +   `XMLResourceBundleControlProvider.class`

+   `资源`

    +   `RBControl.properties`

    +   `RBControl_zh.properties`

    +   `RBControl_zh_CN.properties`

    +   `RBControl_zh_HK.properties`

    +   `RBControl_zh_TW.properties`

    +   `XmlRB.xml`

    +   `XmlRB_ja.xml`

注意，配置文件`java.util.spi.ResourceBundleControlProvider`必须打包在目录`/META-INF/services`中。此示例将这些文件打包在`lib`目录中的 JAR 文件`rbcontrolprovider.jar`中。

查看在 JAR 文件中打包程序以获取有关创建 JAR 文件的更多信息。

或者，下载并安装[Apache Ant](http://ant.apache.org/)，这是一个可以帮助您自动化构建过程的工具，例如编译 Java 文件和创建 JAR 文件。确保 Apache Ant 可执行文件在您的`PATH`环境变量中，以便您可以从任何目录运行它。安装 Apache Ant 后，请按照以下步骤操作：

1.  编辑文件``build.xml``，将`${JAVAC}`更改为您的 Java 编译器`javac`的完整路径名，并将`${JAVA}`更改为您的 Java 运行时可执行文件`java`的完整路径名。

1.  从包含文件`build.xml`的相同目录运行以下命令：

    ```java
    ant jar
    ```

    这个命令编译 Java 源文件并将它们与所需的资源和配置文件打包到`lib`目录中的 JAR 文件`rbcontrolprovider.jar`中。

## 6\. 运行 RBCPTest 程序。

在命令提示符下，从包含`build.xml`文件的目录运行以下命令：

```java
java -Djava.ext.dirs=lib -cp build RBCPTest
```

此命令假定以下情况：

+   包含 RBCPTest 示例编译代码的 JAR 文件位于`lib`目录中。

+   编译后的类`RBCPTest.class`位于`build`目录中。

或者，使用 Apache Ant，并从包含`build.xml`文件的目录运行以下命令：

```java
ant run
```

当您安装 Java 扩展时，通常将扩展的 JAR 文件放在 JRE 的`lib/ext`目录中。但是，此命令使用系统属性`java.ext.dirs`指定包含 Java 扩展的目录。

`RBCPTest`程序首先尝试使用基本名称为`resources.XmlRB`和区域设置为`Locale.ROOT`和`Local.JAPAN`的资源包。程序检索这些资源包的输出类似于以下内容：

```java
Class: rbcp.XMLResourceBundleControlProvider.getControl
    called for resources.XmlRB
    returns rbcp.XMLResourceBundleControl@16c1857
Root locale. Key, type: XML

Class: rbcp.XMLResourceBundleControlProvider.getControl
    called for resources.XmlRB
    returns rbcp.XMLResourceBundleControl@16c1857
Japan locale. Key, type: Value from Japan locale

```

该程序成功获取`XMLResourceBundleControl`的实例并访问属性文件`XmlRB.xml`和`XmlRB_ja.xml`。

当`RBCPTest`程序尝试检索资源包时，它调用配置文件`java.util.spi.ResourceBundleControlProvider`中定义的所有类。例如，当程序检索基本名称为`resources.RBControl`和区域设置为`Locale.CHINA`的资源包时，它打印以下输出：

```java
Class: rbcp.XMLResourceBundleControlProvider.getControl
    called for resources.RBControl
    returns null

Class: rbcp.PropertiesResourceBundleControlProvider.getControl
    called for resources.RBControl
    returns rbcp.PropertiesResourceBundleControl@1ad2911
locale: zh_CN
    region: China
    language: Simplified Chinese

```
