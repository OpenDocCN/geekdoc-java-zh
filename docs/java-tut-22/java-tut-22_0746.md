# 标准 MBeans

> 原文：[`docs.oracle.com/javase/tutorial/jmx/mbeans/standard.html`](https://docs.oracle.com/javase/tutorial/jmx/mbeans/standard.html)

本节介绍了一个简单的标准 MBean 示例。

通过编写一个名为 `SomethingMBean` 的 Java 接口和一个实现该接口的名为 `Something` 的 Java 类来定义标准 MBean。接口中的每个方法默认定义一个操作。属性和操作是遵循特定设计模式的方法。标准 MBean 由一个 MBean 接口和一个类组成。MBean 接口列出了所有公开属性和操作的方法。类实现此接口并提供受监视资源的功能。

以下部分将介绍一个标准 MBean 和一个简单的支持 JMX 技术的代理（JMX 代理）管理该 MBean。

## MBean 接口

一个基本 MBean 接口的示例，`HelloMBean`，如下所示：

```java

package com.example; 

public interface HelloMBean { 

    public void sayHello(); 
    public int add(int x, int y); 

    public String getName(); 

    public int getCacheSize(); 
    public void setCacheSize(int size); 
} 

```

按照惯例，一个 MBean 接口采用实现它的 Java 类的名称，后缀 *`MBean`* 添加在后面。在这种情况下，接口被称为 `HelloMBean`。实现此接口的 `Hello` 类将在下一节中描述。

根据 JMX 规范，一个 MBean 接口由具有名称和类型的属性组成，这些属性可读取并可能可写，另外还包括由 MBean 管理的应用程序可以调用的具有名称和类型的操作。`HelloMBean` 接口声明了两个操作：Java 方法 `add()` 和 `sayHello()`。

`HelloMBean` 声明了两个属性：`Name` 是一个只读字符串，`CacheSize` 是一个既可读又可写的整数。声明了 getter 和 setter 方法，允许受管应用程序访问并可能更改属性值。根据 JMX 规范的定义，*getter* 是任何不返回 void 并且名称以 `get` 开头的公共方法。getter 使管理者能够读取属性的值，其类型与返回对象的类型相同。*setter* 是任何接受单个参数并且名称以 `set` 开头的公共方法。setter 使管理者能够在属性中写入新值，其类型与参数的类型相同。

这些操作和属性的实现在下一节中展示。

## MBean 实现

下面的 `Hello` Java 类实现了 `HelloMBean` MBean 接口：

```java
package com.example; 

public class Hello ... 
    implements HelloMBean { 
    public void sayHello() { 
        System.out.println("hello, world"); 
    } 

    public int add(int x, int y) { 
        return x + y; 
    } 

    public String getName() { 
        return this.name; 
    }  

    public int getCacheSize() { 
        return this.cacheSize; 
    } 

    public synchronized void setCacheSize(int size) {
        ...

        this.cacheSize = size; 
        System.out.println("Cache size now " + this.cacheSize); 
    } 
    ...

    private final String name = "Reginald"; 
    private int cacheSize = DEFAULT_CACHE_SIZE; 
    private static final int 
        DEFAULT_CACHE_SIZE = 200; 
}

```

直接的 `Hello` 类提供了由 `HelloMBean` 声明的操作和属性的定义。`sayHello()` 和 `add()` 操作非常简单，但实际操作可以根据需要简单或复杂。

还定义了获取`Name`属性和获取/设置`CacheSize`属性的方法。在此示例中，`Name`属性值永远不会改变。但是，在实际情况下，此属性可能随着受管资源的运行而发生变化。例如，该属性可能代表诸如正常运行时间或内存使用情况之类的统计信息。在这里，该属性仅仅是名称`Reginald`。

调用`setCacheSize`方法可以修改`CacheSize`属性，将其从声明的默认值 200 改变。在实际情况下，更改`CacheSize`属性可能需要执行其他操作，例如丢弃条目或分配新条目。此示例仅仅打印一条消息以确认缓存大小已更改。但是，可以定义更复杂的操作，而不是简单调用`println()`。

有了`Hello` MBean 及其接口的定义，它们现在可以用来管理它们所代表的资源，如下一节所示。

## 创建一个用于管理资源的 JMX 代理

一旦资源被 MBeans 进行了仪器化，该资源的管理就由 JMX 代理执行。

JMX 代理的核心组件是 MBean 服务器。MBean 服务器是一个托管对象服务器，其中注册了 MBeans。JMX 代理还包括一组服务来管理 MBeans。有关 MBean 服务器实现的详细信息，请参阅[`MBeanServer`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanServer.html)接口的 API 文档。

接下来的`Main`类代表了一个基本的 JMX 代理：

```java
package com.example; 

import java.lang.management.*; 
import javax.management.*; 

public class Main { 

    public static void main(String[] args) 
        throws Exception { 

        MBeanServer mbs = ManagementFactory.getPlatformMBeanServer(); 
        ObjectName name = new ObjectName("com.example:type=Hello"); 
        Hello mbean = new Hello(); 
        mbs.registerMBean(mbean, name); 

        ...

        System.out.println("Waiting forever..."); 
        Thread.sleep(Long.MAX_VALUE); 
    } 
} 

```

JMX 代理`Main`首先通过调用[`java.lang.management.ManagementFactory`](https://docs.oracle.com/javase/8/docs/api/java/lang/management/ManagementFactory.html)类的`getPlatformMBeanServer()`方法来获取由平台创建和初始化的 MBean 服务器。如果平台尚未创建 MBean 服务器，则`getPlatformMBeanServer()`会通过调用 JMX 方法[`MBeanServerFactory.createMBeanServer()`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanServerFactory.html#createMBeanServer--)自动创建 MBean 服务器。`Main`获取的`MBeanServer`实例命名为`mbs`。

接下来，`Main`为将要创建的 MBean 实例定义了一个对象名称。每个 JMX MBean 都必须有一个对象名称。对象名称是 JMX 类[`ObjectName`](https://docs.oracle.com/javase/8/docs/api/javax/management/ObjectName.html)的一个实例，并且必须符合 JMX 规范定义的语法。换句话说，对象名称必须包含一个域和一组键-属性。在`Main`定义的对象名称中，域是`com.example`（示例 MBean 所在的包）。此外，键-属性声明此对象是`Hello`类型。

创建了一个名为`mbean`的`Hello`对象实例。然后，通过将对象和对象名称传递给 JMX 方法[`MBeanServer.registerMBean()`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanServer.html#registerMBean-java.lang.Object-javax.management.ObjectName-)，将名为`mbean`的`Hello`对象注册为 MBean 在 MBean 服务器`mbs`中。

在 MBean 服务器中注册了`Hello` MBean 后，`Main`只是等待对`Hello`执行管理操作。在这个示例中，这些管理操作包括调用`sayHello()`和`add()`，以及获取和设置属性值。

## 运行标准 MBean 示例

在检查了示例类之后，现在可以运行示例了。在这个示例中，使用 JConsole 与 MBean 交互。

要运行示例，请按照以下步骤操作：

1.  将 JMX API 示例类的包`jmx_examples.zip`保存到你的工作目录`work_dir`。

1.  使用以下命令在终端窗口中解压示例类的包。

    ```java
    unzip jmx_examples.zip

    ```

1.  在`work_dir`目录中编译示例 Java 类。

    ```java
    javac com/example/*.java

    ```

1.  如果你正在运行 Java 开发工具包（JDK）版本 6，请使用以下命令启动`Main`应用程序。

    ```java
    java com.example.Main

    ```

    如果你使用的 JDK 版本低于 6，你需要使用以下选项启动`Main`应用程序，以便监控和管理应用程序。

    ```java
    java -Dcom.sun.management.jmxremote example.Main

    ```

    显示`Main`正在等待某些事件发生的确认。

1.  在同一台机器的不同终端窗口中启动 JConsole。

    ```java
    jconsole

    ```

    显示新连接对话框，列出了可以连接的正在运行的 JMX 代理的列表。

1.  在新连接对话框中，从列表中选择`com.example.Main`，然后点击连接。

    显示您平台当前活动的摘要。

1.  点击 MBeans 选项卡。

    此面板显示当前在 MBean 服务器中注册的所有 MBean。

1.  在左侧框架中，展开 MBean 树中的`com.example`节点。

    你会看到由`Main`创建和注册的示例 MBean `Hello`。如果你点击`Hello`，你会在 MBean 树中看到其关联的属性和操作节点。

1.  展开 MBean 树中`Hello` MBean 的属性节点。

    显示由`Hello`类定义的 MBean 属性。

1.  将`CacheSize`属性的值更改为 150。

    在你启动`Main`的终端窗口中，会生成对属性更改的确认。

1.  展开 MBean 树中`Hello` MBean 的操作节点。

    `Hello` MBean 声明的两个操作，`sayHello()`和`add()`，是可见的。

1.  通过点击`sayHello`按钮调用`sayHello()`操作。

    一个 JConsole 对话框通知您方法已成功调用。消息“hello, world”会在运行`Main`的终端窗口中生成。

1.  为`add()`操作提供两个整数相加，然后点击`add`按钮。

    答案显示在一个 JConsole 对话框中。

1.  要关闭 JConsole，请选择 Connection -> Exit。
