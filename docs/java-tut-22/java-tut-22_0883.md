# 创建可扩展应用程序

> 原文：[`docs.oracle.com/javase/tutorial/ext/basics/spi.html`](https://docs.oracle.com/javase/tutorial/ext/basics/spi.html)

下面涵盖了以下主题：

+   介绍

+   字典服务示例

+   运行 DictionaryServiceDemo 示例

+   编译和运行 DictionaryServiceDemo 示例

+   理解 DictionaryServiceDemo 示例

    1.  定义服务提供者接口

    1.  定义检索服务提供者实现的服务

        +   单例设计模式

    1.  实现服务提供者

    1.  注册服务提供者

    1.  创建使用服务和服务提供者的客户端

    1.  将服务提供者、服务和服务客户端打包在 JAR 文件中

        +   将服务提供者打包在 JAR 文件中

        +   将字典 SPI 和字典服务打包在 JAR 文件中

        +   将客户端打包在 JAR 文件中

    1.  运行客户端

+   ServiceLoader 类

+   ServiceLoader API 的限制

+   摘要

## 介绍

可扩展的应用程序是一种可以在不修改其原始代码基础的情况下扩展的应用程序。您可以通过添加新的插件或模块来增强其功能。开发人员、软件供应商和客户可以通过将新的 Java 存档（JAR）文件添加到应用程序类路径或应用程序特定的扩展目录中来添加新功能或应用程序编程接口（API）。

本节描述了如何创建具有可扩展服务的应用程序，这使您或其他人可以提供不需要修改原始应用程序的服务实现。通过设计一个可扩展的应用程序，您提供了一种升级或增强产品特定部分而无需更改核心应用程序的方法。

可扩展应用程序的一个示例是允许最终用户添加新字典或拼写检查器的文字处理器。在这个示例中，文字处理器提供了一个字典或拼写功能，其他开发人员甚至客户可以通过提供自己的功能实现来扩展该功能。

以下是理解可扩展应用程序重要的术语和定义：

服务

一组编程接口和类，提供对某些特定应用功能或特性的访问。服务可以定义功能的接口和检索实现的方法。在文字处理器示例中，字典服务可以定义检索字典和单词定义的方法，但不实现底层功能集。相反，它依赖于*服务提供者*来实现该功能。

服务提供者接口（SPI）

服务定义的一组公共接口和抽象类。SPI 定义了应用程序可用的类和方法。

服务提供者

实现 SPI。具有可扩展服务的应用程序使您、供应商和客户能够添加服务提供者，而无需修改原始应用程序。

## 字典服务示例

考虑如何在文字处理器或编辑器中设计一个字典服务。一种方法是定义一个由类`DictionaryService`和服务提供者接口`Dictionary`表示的服务。`DictionaryService`提供一个单例`DictionaryService`对象。（有关更多信息，请参见单例设计模式部分。）此对象从`Dictionary`提供者那里检索单词的定义。字典服务客户端——您的应用代码——检索此服务的一个实例，服务将搜索、实例化和使用`Dictionary`服务提供者。

尽管文字处理器开发人员很可能会在原始产品中提供一个基本的通用字典，但客户可能需要一个包含法律或技术术语的专业字典。理想情况下，客户能够创建或购买新的字典并将其添加到现有应用程序中。

`DictionaryServiceDemo`示例向您展示如何实现`Dictionary`服务，创建添加额外字典的`Dictionary`服务提供者，并创建一个简单的`Dictionary`服务客户端来测试该服务。此示例打包在 zip 文件`DictionaryServiceDemo.zip`中，包括以下文件：

+   ``build.xml``

+   `DictionaryDemo`

    +   ``build.xml``

    +   `build`

    +   `dist`

        +   `DictionaryDemo.jar`

    +   `src`

        +   `dictionary`

            +   ``DictionaryDemo.java``

+   `DictionaryServiceProvider`

    +   ``build.xml``

    +   `build`

    +   `dist`

        +   `DictionaryServiceProvider.jar`

    +   `src`

        +   `dictionary`

            +   ``DictionaryService.java``

            +   `spi`

                +   ``Dictionary.java``

+   `ExtendedDictionary`

    +   ``build.xml``

    +   `build`

    +   `dist`

        +   `ExtendedDictionary.jar`

    +   `src`

        +   `dictionary`

            +   ``ExtendedDictionary.java``

        +   `META-INF`

            +   `services`

                +   ``dictionary.spi.Dictionary``

+   `GeneralDictionary`

    +   ``build.xml``

    +   `build`

    +   `dist`

        +   `GeneralDictionary.jar`

    +   `src`

        +   `dictionary`

            +   ``GeneralDictionary.java``

        +   `META-INF`

            +   `services`

                +   ``dictionary.spi.Dictionary``

**注意**：`build`目录包含与`src`目录中的 Java 源文件相同级别的编译后的类文件。

## 运行`DictionaryServiceDemo`示例

因为 zip 文件`DictionaryServiceDemo.zip`包含编译后的类文件，您可以将此文件解压缩到计算机上，并按照以下步骤运行示例而无需编译：

1.  下载并解压缩示例代码：将文件`DictionaryServiceDemo.zip`下载并解压缩到计算机上。这些步骤假定您已将此文件的内容解压缩到目录`C:\DictionaryServiceDemo`中。

1.  将当前目录更改为`C:\DictionaryServiceDemo\DictionaryDemo`，并按照步骤运行客户端进行操作。

## 编译和运行`DictionaryServiceDemo`示例

`DictionaryServiceDemo`示例包含 Apache Ant 构建文件，全部命名为`build.xml`。以下步骤展示了如何使用 Apache Ant 编译、构建和运行`DictionaryServiceDemo`示例：

1.  安装 Apache Ant：前往以下链接下载并安装 Apache Ant：

    `[`ant.apache.org/`](http://ant.apache.org/)`

    确保包含 Apache Ant 可执行文件的目录在您的`PATH`环境变量中，以便您可以从任何目录运行它。此外，请确保您的 JDK 的`bin`目录，其中包含`java`和`javac`可执行文件（对于 Microsoft Windows 为`java.exe`和`javac.exe`），在您的`PATH`环境变量中。有关设置`PATH`环境变量的信息，请参阅 PATH and CLASSPATH。

1.  下载并解压缩示例代码：将文件`DictionaryServiceDemo.zip`下载并解压缩到计算机上。这些步骤假定您已将此文件的内容解压缩到目录`C:\DictionaryServiceDemo`中。

1.  编译代码：将当前目录更改为`C:\DictionaryServiceDemo`，并运行以下命令：

    ```java
    ant compile-all
    ```

    此命令编译了`DictionaryDemo`、`DictionaryServiceProvider`、`ExtendedDictionary`和`GeneralDictionary`目录中包含的`src`目录中的源代码，并将生成的`class`文件放入相应的`build`目录中。

1.  将编译后的 Java 文件打包成 JAR 文件：确保当前目录为`C:\DictionaryServiceDemo`，然后运行以下命令：

    ```java
    ant jar
    ```

    此命令创建以下 JAR 文件：

    +   `DictionaryDemo/dist/DictionaryDemo.jar`

    +   `DictionaryServiceProvider/dist/DictionaryServiceProvider.jar`

    +   `GeneralDictionary/dist/GeneralDictionary.jar`

    +   `ExtendedDictionary/dist/ExtendedDictionary.jar`

1.  运行示例：确保包含`java`可执行文件的目录在您的`PATH`环境变量中。有关更多信息，请参阅 PATH 和 CLASSPATH。

    将当前目录更改为`C:\DictionaryServiceDemo\DictionaryDemo`，然后运行以下命令：

    ```java
    ant run
    ```

    该示例打印以下内容：

    `book: a set of written or printed pages, usually bound with a protective cover`

    编辑者：一个编辑文档的人

    xml：一种经常用于 Web 服务等的文档标准

    REST：一种用于创建、读取、更新和删除数据的架构风格，试图使用 HTTP 协议的常见词汇；表述性状态转移

## 理解 DictionaryServiceDemo 示例

以下步骤向您展示如何重新创建文件`DictionaryServiceDemo.zip`的内容。这些步骤向您展示示例的工作原理以及如何运行它。

### 1\. 定义服务提供者接口

`DictionaryServiceDemo`示例定义了一个 SPI，即``Dictionary.java``接口。它只包含一个方法：

```java

package dictionary.spi;

public interface Dictionary {
    public String getDefinition(String word);
}

```

该示例将编译后的类文件存储在`DictionaryServiceProvider/build`目录中。

### 2\. 定义检索服务提供者实现的服务

``DictionaryService.java``类加载并访问可用的`Dictionary`服务提供者，代表字典服务客户端：

```java

package dictionary;

import dictionary.spi.Dictionary;
import java.util.Iterator;
import java.util.ServiceConfigurationError;
import java.util.ServiceLoader;

public class DictionaryService {

    private static DictionaryService service;
    private ServiceLoader<Dictionary> loader;

    private DictionaryService() {
        loader = ServiceLoader.load(Dictionary.class);
    }

    public static synchronized DictionaryService getInstance() {
        if (service == null) {
            service = new DictionaryService();
        }
        return service;
    }

    public String getDefinition(String word) {
        String definition = null;

        try {
            Iterator<Dictionary> dictionaries = loader.iterator();
            while (definition == null && dictionaries.hasNext()) {
                Dictionary d = dictionaries.next();
                definition = d.getDefinition(word);
            }
        } catch (ServiceConfigurationError serviceError) {
            definition = null;
            serviceError.printStackTrace();

        }
        return definition;
    }
}

```

该示例将编译后的类文件存储在`DictionaryServiceProvider/build`目录中。

`DictionaryService`类实现了单例设计模式。这意味着`DictionaryService`类只会创建一个实例。有关更多信息，请参阅单例设计模式部分。

`DictionaryService`类是字典服务客户端使用任何已安装的`Dictionary`服务提供者的入口点。使用[`ServiceLoader.load`](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html#load-java.lang.Class-)方法来检索私有静态成员`DictionaryService.service`，单例服务入口点。然后应用程序可以调用`getDefinition`方法，该方法遍历可用的`Dictionary`提供者，直到找到目标词。如果没有`Dictionary`实例包含指定词的定义，则`getDefinition`方法返回 null。

字典服务使用`ServiceLoader.load`方法来查找目标类。SPI 由接口`dictionary.spi.Dictionary`定义，因此示例使用这个类作为 load 方法的参数。默认的 load 方法使用默认类加载器搜索应用程序类路径。

然而，这个方法的重载版本允许您指定自定义的类加载器。这使您能够进行更复杂的类搜索。一个特别热情的程序员可能会创建一个[`ClassLoader`](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassLoader.html)实例，可以在运行时添加包含提供者 JAR 的应用程序特定子目录中进行搜索。结果是一个应用程序不需要重新启动就可以访问新的提供者类。

当这个类的加载器存在后，您可以使用它的迭代器方法来访问和使用它找到的每个提供者。`getDefinition`方法使用`Dictionary`迭代器来遍历提供者，直到找到指定词的定义。迭代器方法缓存`Dictionary`实例，因此连续调用需要很少的额外处理时间。如果自上次调用以来已经投入使用新的提供者，则迭代器方法将它们添加到列表中。

``DictionaryDemo.java`` 类使用这个服务。要使用该服务，应用程序获取一个`DictionaryService`实例并调用`getDefinition`方法。如果有定义可用，应用程序将打印出来。如果没有定义可用，应用程序将打印一条消息，说明没有可用的字典包含这个词。

#### 单例设计模式

设计模式是软件设计中常见问题的一般解决方案。思路是将解决方案转化为代码，并且该代码可以应用在不同的情况下。单例模式描述了一种技术，确保只创建一个类的实例。本质上，该技术采取以下方法：不要让类外部的任何人创建对象的实例。

例如，``DictionaryService``类实现了单例模式如下：

+   将`DictionaryService`构造函数声明为`private`，这样除了`DictionaryService`之外的所有其他类都无法创建它的实例。

+   将`DictionaryService`成员变量`service`声明为`static`，这确保只存在一个`DictionaryService`实例。

+   定义了`getInstance`方法，使其他类可以受控地访问`DictionaryService`成员变量`service`。

### 3\. 实现服务提供程序

要提供此服务，您必须创建一个``Dictionary.java``的实现。为了保持简单，创建一个定义了几个词的通用词典。您可以使用数据库、一组属性文件或任何其他技术来实现词典。展示提供程序模式的最简单方法是在单个文件中包含所有单词和定义。

以下代码展示了`Dictionary` SPI 的一个实现，``GeneralDictionary.java``类。请注意，它提供了一个无参数构造函数，并实现了 SPI 定义的`getDefinition`方法。

```java

package dictionary;

import dictionary.spi.Dictionary;
import java.util.SortedMap;
import java.util.TreeMap;

public class GeneralDictionary implements Dictionary {

    private SortedMap<String, String> map;

    public GeneralDictionary() {
        map = new TreeMap<String, String>();
        map.put(
            "book",
            "a set of written or printed pages, usually bound with " +
                "a protective cover");
        map.put(
            "editor",
            "a person who edits");
    }

    @Override
    public String getDefinition(String word) {
        return map.get(word);
    }

}

```

示例将编译后的类文件存储在目录`GeneralDictionary/build`中。**注意**：在类`GeneralDictionary`之前，您必须编译类`dictionary.DictionaryService`和`dictionary.spi.Dictionary`。

此示例的`GeneralDictionary`提供程序仅定义了两个词：*book*和*editor*。显然，一个更可用的词典将提供一个更实质的通用词汇列表。

为了演示多个提供程序如何实现相同的 SPI，以下代码展示了另一个可能的提供程序。``ExtendedDictionary.java``服务提供程序是一个包含大多数软件开发人员熟悉的技术术语的扩展词典。

```java

package dictionary;

import dictionary.spi.Dictionary;
import java.util.SortedMap;
import java.util.TreeMap;

public class ExtendedDictionary implements Dictionary {

        private SortedMap<String, String> map;

    public ExtendedDictionary() {
        map = new TreeMap<String, String>();
        map.put(
            "xml",
            "a document standard often used in web services, among other " +
                "things");
        map.put(
            "REST",
            "an architecture style for creating, reading, updating, " +
                "and deleting data that attempts to use the common " +
                "vocabulary of the HTTP protocol; Representational State " +
                "Transfer");
    }

    @Override
    public String getDefinition(String word) {
        return map.get(word);
    }

}

```

示例将编译后的类文件存储在目录`ExtendedDictionary/build`中。**注意**：在类`ExtendedDictionary`之前，您必须编译类`dictionary.DictionaryService`和`dictionary.spi.Dictionary`。

很容易想象客户使用完整的`Dictionary`提供程序集来满足他们自己的特殊需求。服务加载器 API 使他们能够根据需要或偏好向其应用程序添加新的词典。由于底层的文字处理应用程序是可扩展的，因此客户无需编写额外的代码即可使用新的提供程序。

### 4\. 注册服务提供程序

要注册您的服务提供者，需要创建一个提供者配置文件，该文件存储在服务提供者的 JAR 文件的 `META-INF/services` 目录中。配置文件的名称是服务提供者的完全限定类名，其中名称的每个组件由句点（`.`）分隔，嵌套类由美元符号（`$`）分隔。

提供者配置文件包含您的服务提供者的完全限定类名，每个名称占一行。该文件必须使用 UTF-8 编码。此外，您可以通过在注释行前面加上井号（`#`）来在文件中包含注释。

例如，要注册服务提供者 `GeneralDictionary`，创建一个名为 ``dictionary.spi.Dictionary`` 的文本文件。该文件包含一行：

```java
dictionary.GeneralDictionary

```

类似地，要注册服务提供者 `ExtendedDictionary`，创建一个名为 ``dictionary.spi.Dictionary`` 的文本文件。该文件包含一行：

```java
dictionary.ExtendedDictionary

```

### 5\. 创建使用服务和服务提供者的客户端

因为开发完整的文字处理器应用程序是一项重大工作，本教程提供了一个更简单的应用程序，该应用程序使用 `DictionaryService` 和 `Dictionary` SPI。`DictionaryDemo` 示例从类路径上的任何 `Dictionary` 提供者中搜索单词 *book*、*editor*、*xml* 和 *REST*，并检索它们的定义。

以下是 ``DictionaryDemo`` 示例。它从 `DictionaryService` 实例请求目标单词的定义，然后将请求传递给已知的 `Dictionary` 提供者。

```java

package dictionary;

import dictionary.DictionaryService;

public class DictionaryDemo {

  public static void main(String[] args) {

    DictionaryService dictionary = DictionaryService.getInstance();
    System.out.println(DictionaryDemo.lookup(dictionary, "book"));
    System.out.println(DictionaryDemo.lookup(dictionary, "editor"));
    System.out.println(DictionaryDemo.lookup(dictionary, "xml"));
    System.out.println(DictionaryDemo.lookup(dictionary, "REST"));
  }

  public static String lookup(DictionaryService dictionary, String word) {
    String outputString = word + ": ";
    String definition = dictionary.getDefinition(word);
    if (definition == null) {
      return outputString + "Cannot find definition for this word.";
    } else {
      return outputString + definition;
    }
  }
}

```

该示例将编译后的类文件存储在目录 `DictionaryDemo/build` 中。**注意**：在运行 `DictionaryDemo` 类之前，必须先编译类 `dictionary.DictionaryService` 和 `dictionary.spi.Dictionary`。

### 6\. 将服务提供者、服务和服务客户端打包到 JAR 文件中

请参阅课程 在 JAR 文件中打包程序 了解如何创建 JAR 文件的信息。

#### 在 JAR 文件中打包服务提供者

要打包 `GeneralDictionary` 服务提供者，创建一个名为 `GeneralDictionary/dist/GeneralDictionary.jar` 的 JAR 文件，其中包含此服务提供者的编译类文件和以下目录结构中的配置文件：

+   `META-INF`

    +   `服务`

        +   `dictionary.spi.Dictionary`

+   `dictionary`

    +   `GeneralDictionary.class`

类似地，要打包 `ExtendedDictionary` 服务提供者，创建一个名为 `ExtendedDictionary/dist/ExtendedDictionary.jar` 的 JAR 文件，其中包含此服务提供者的编译类文件和以下目录结构中的配置文件：

+   `META-INF`

    +   `服务`

        +   `dictionary.spi.Dictionary`

+   `dictionary`

    +   `ExtendedDictionary.class`

注意，提供者配置文件必须位于 JAR 文件中的`META-INF/services`目录中。

#### 将 Dictionary SPI 和 Dictionary Service 打包成一个 JAR 文件

创建一个名为`DictionaryServiceProvider/dist/DictionaryServiceProvider.jar`的 JAR 文件，其中包含以下文件：

+   `dictionary`

    +   `DictionaryService.class`

    +   `spi`

        +   `Dictionary.class`

#### 将客户端打包成一个 JAR 文件

创建一个名为`DictionaryDemo/dist/DictionaryDemo.jar`的 JAR 文件，其中包含以下文件：

+   `dictionary`

    +   `DictionaryDemo.class`

### 7\. 运行客户端

以下命令运行带有`GeneralDictionary`服务提供者的`DictionaryDemo`示例：

**Linux 和 Solaris:**

`java -Djava.ext.dirs=../DictionaryServiceProvider/dist:../GeneralDictionary/dist -cp dist/DictionaryDemo.jar dictionary.DictionaryDemo`

**Windows:**

`java -Djava.ext.dirs=..\DictionaryServiceProvider\dist;..\GeneralDictionary\dist -cp dist\DictionaryDemo.jar dictionary.DictionaryDemo`

使用此命令时，假设以下情况：

+   当前目录是`DictionaryDemo`。

+   存在以下 JAR 文件：

    +   `DictionaryDemo/dist/DictionaryDemo.jar`: 包含`DictionaryDemo`类

    +   `DictionaryServiceProvider/dist/DictionaryServiceProvider.jar`: 包含`Dictionary` SPI 和`DictionaryService`类

    +   `GeneralDictionary/dist/GeneralDictionary.jar`: 包含`GeneralDictionary`服务提供者和配置文件

该命令打印以下内容：

```java
book: a set of written or printed pages, usually bound with a protective cover
editor: a person who edits
xml: Cannot find definition for this word.
REST: Cannot find definition for this word.

```

假设你运行以下命令并且`ExtendedDictionary/dist/ExtendedDictionary.jar`存在：

**Linux 和 Solaris:**

`java -Djava.ext.dirs=../DictionaryServiceProvider/dist:../ExtendedDictionary/dist -cp dist/DictionaryDemo.jar dictionary.DictionaryDemo`

**Windows:**

`java -Djava.ext.dirs=..\DictionaryServiceProvider\dist;..\ExtendedDictionary\dist -cp dist\DictionaryDemo.jar dictionary.DictionaryDemo`

该命令打印以下内容：

```java
book: Cannot find definition for this word.
editor: Cannot find definition for this word.
xml: a document standard often used in web services, among other things
REST: an architecture style for creating, reading, updating, and deleting data that attempts to use the common vocabulary of the HTTP protocol; Representational State Transfer

```

## ServiceLoader 类

`java.util.ServiceLoader`类帮助你查找、加载和使用服务提供者。它在应用程序的类路径或运行时环境的扩展目录中搜索服务提供者。它加载它们并使你的应用程序能够使用提供者的 API。如果你将新的提供者添加到类路径或运行时扩展目录中，`ServiceLoader`类会找到它们。如果你的应用程序知道提供者接口，它可以找到并使用该接口的不同实现。你可以使用接口的第一个可加载实例或遍历所有可用接口。

`ServiceLoader`类是 final 的，这意味着你不能将其作为子类或覆盖其加载算法。例如，你不能改变其算法以从不同位置搜索服务。

从`ServiceLoader`类的角度来看，所有服务都具有单一类型，通常是单一接口或抽象类。提供者本身包含一个或多个具体类，这些类扩展了服务类型，具有特定于其目的的实现。`ServiceLoader`类要求单个公开的提供者类型具有默认构造函数，不需要参数。这使得`ServiceLoader`类可以轻松实例化它找到的服务提供者。

提供者是按需定位和实例化的。服务加载器维护了已加载的提供者的缓存。加载器的`iterator`方法的每次调用都会返回一个迭代器，首先按实例化顺序产生缓存中的所有元素。然后，服务加载器会定位和实例化任何新的提供者，依次将每个提供者添加到缓存中。您可以使用`reload`方法清除提供者缓存。

要为特定类创建加载器，请将类本身提供给`load`或`loadInstalled`方法。您可以使用默认类加载器或提供自己的`ClassLoader`子类。

`loadInstalled`方法搜索已安装的运行时提供者的扩展目录。默认的扩展位置是您运行时环境的`jre/lib/ext`目录。您应该仅将扩展位置用于知名的、可信任的提供者，因为此位置将成为所有应用程序的类路径的一部分。在本文中，提供者不使用扩展目录，而是依赖于特定于应用程序的类路径。

## `ServiceLoader` API 的局限性

`ServiceLoader` API 很有用，但也有局限性。例如，不可能从`ServiceLoader`类派生类，因此无法修改其行为。您可以使用自定义的`ClassLoader`子类来更改类的查找方式，但`ServiceLoader`本身无法扩展。此外，当前的`ServiceLoader`类无法告诉您的应用程序运行时何时有新的提供者可用。此外，您无法向加载器添加更改侦听器，以查找新提供者是否放置在特定于应用程序的扩展目录中。

公共的`ServiceLoader` API 在 Java SE 6 中可用。虽然加载器服务早在 JDK 1.3 时就存在，但 API 是私有的，只对内部 Java 运行时代码可用。

## 摘要

可扩展的应用程序提供了可以由服务提供者扩展的服务点。创建可扩展应用程序的最简单方法是使用`ServiceLoader`，该工具适用于 Java SE 6 及更高版本。使用这个类，您可以将提供者实现添加到应用程序类路径中，以提供新功能。`ServiceLoader`类是 final 的，因此您无法修改其功能。
