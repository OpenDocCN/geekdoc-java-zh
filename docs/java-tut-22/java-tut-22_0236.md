# 系统属性

> 原文：[`docs.oracle.com/javase/tutorial/essential/environment/sysprop.html`](https://docs.oracle.com/javase/tutorial/essential/environment/sysprop.html)

在 Properties 中，我们研究了应用程序如何使用 `Properties` 对象来维护其配置。Java 平台本身使用 `Properties` 对象来维护自己的配置。`System` 类维护一个描述当前工作环境配置的 `Properties` 对象。系统属性包括当前用户、当前 Java 运行时版本以及用于分隔文件路径名组件的字符的信息。

以下表格描述了一些最重要的系统属性。

| 键 | 含义 |
| --- | --- |
| `"file.separator"` | 文件路径中分隔组件的字符。在 UNIX 上是 "`/`"，在 Windows 上是 "`\`"。 |
| `"java.class.path"` | 用于查找包含类文件的目录和 JAR 存档的路径。类路径的元素由 `path.separator` 属性中指定的特定于平台的字符分隔。 |
| `"java.home"` | Java Runtime Environment (JRE) 的安装目录 |
| `"java.vendor"` | JRE 供应商名称 |
| `"java.vendor.url"` | JRE 供应商 URL |
| `"java.version"` | JRE 版本号 |
| `"line.separator"` | 操作系统用于在文本文件中分隔行的序列 |
| `"os.arch"` | 操作系统架构 |
| `"os.name"` | 操作系统名称 |
| `"os.version"` | 操作系统版本 |
| `"path.separator"` | `java.class.path` 中使用的路径分隔符字符 |
| `"user.dir"` | 用户工作目录 |
| `"user.home"` | 用户主目录 |
| `"user.name"` | 用户账户名 |

* * *

**安全注意事项：** 访问系统属性可能受到 安全管理器 的限制。这在小程序中最常见，小程序被阻止读取某些系统属性，并且无法写入*任何*系统属性。有关在小程序中访问系统属性的更多信息，请参阅 System Properties 中的 Java 富互联网应用程序进阶 课程。

* * *

## 读取系统属性

`System` 类有两个用于读取系统属性的方法：`getProperty` 和 `getProperties`。

`System` 类有两个不同版本的 `getProperty`。两者都检索参数列表中命名的属性的值。其中较简单的 `getProperty` 方法接受一个参数，即属性键。例如，要获取 `path.separator` 的值，请使用以下语句：

```java
System.getProperty("path.separator");

```

`getProperty` 方法返回一个包含属性值的字符串。如果属性不存在，此版本的 `getProperty` 返回 null。

`getProperty` 的另一个版本需要两个 `String` 参数：第一个参数是要查找的键，第二个参数是在找不到键或键没有值时要返回的默认值。例如，下面的 `getProperty` 调用查找名为 `subliminal.message` 的 `System` 属性。这不是一个有效的系统属性，所以该方法返回提供的第二个参数作为默认值：“`购买 StayPuft 棉花糖！`”

```java
System.getProperty("subliminal.message", "Buy StayPuft Marshmallows!");

```

`System` 类提供的最后一个方法用于访问属性值的是 `getProperties` 方法，它返回一个 [`Properties`](https://docs.oracle.com/javase/8/docs/api/java/util/Properties.html) 对象。该对象包含完整的系统属性定义集。

## 写入系统属性

要修改现有的系统属性集，请使用 `System.setProperties`。此方法接受一个已初始化以包含要设置的属性的 `Properties` 对象。此方法用新的由 `Properties` 对象表示的属性集替换整个系统属性集。

* * *

**警告：** 改变系统属性可能是潜在危险的，应谨慎操作。许多系统属性在启动后不会重新读取，并且仅用于信息目的。更改某些属性可能会产生意想不到的副作用。

* * *

下一个示例，`PropertiesTest`，创建一个 `Properties` 对象，并从 `myProperties.txt` 初始化它。

```java
subliminal.message=Buy StayPuft Marshmallows!

```

`PropertiesTest` 然后使用 `System.setProperties` 将新的 `Properties` 对象安装为当前系统属性集。

```java

import java.io.FileInputStream;
import java.util.Properties;

public class PropertiesTest {
    public static void main(String[] args)
        throws Exception {

        // set up new properties object
        // from file "myProperties.txt"
        FileInputStream propFile =
            new FileInputStream( "myProperties.txt");
        Properties p =
            new Properties(System.getProperties());
        p.load(propFile);

        // set the system properties
        System.setProperties(p);
        // display new properties
        System.getProperties().list(System.out);
    }
}

```

注意 `PropertiesTest` 如何创建 `Properties` 对象 `p`，并将其用作 `setProperties` 的参数：

```java
Properties p = new Properties(System.getProperties());

```

此语句使用当前系统属性集初始化新的属性对象 `p`，在这个小应用程序的情况下，这是运行时系统初始化的属性集。然后应用程序从文件 `myProperties.txt` 中加载额外的属性到 `p` 中，并将系统属性设置为 `p`。这将导致将 `myProperties.txt` 中列出的属性添加到运行时系统在启动时创建的属性集中。请注意，应用程序可以创建没有任何默认 `Properties` 对象的 `p`，如下所示：

```java
Properties p = new Properties();

```

还要注意系统属性的值是可以被覆盖的！例如，如果 `myProperties.txt` 包含以下行，则 `java.vendor` 系统属性将被覆盖：

```java
java.vendor=Acme Software Company

```

一般来说，要小心不要覆盖系统属性。

`setProperties` 方法更改当前运行应用程序的系统属性集。这些更改不是持久的。也就是说，在应用程序内更改系统属性不会影响此应用程序或任何其他应用程序的将来调用 Java 解释器。运行时系统每次启动时都会重新初始化系统属性。如果要使系统属性的更改持久化，则应用程序必须在退出之前将值写入某个文件，并在启动时重新读取。
