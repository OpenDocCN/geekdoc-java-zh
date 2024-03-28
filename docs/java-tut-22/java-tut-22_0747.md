# MXBeans

> 原文：[`docs.oracle.com/javase/tutorial/jmx/mbeans/mxbeans.html`](https://docs.oracle.com/javase/tutorial/jmx/mbeans/mxbeans.html)

本节介绍了一种特殊类型的 MBean，称为 *MXBeans*。

*MXBean* 是一种只引用预定义数据类型的 MBean 类型。通过这种方式，您可以确保您的 MBean 可以被任何客户端使用，包括远程客户端，而无需客户端访问表示您的 MBeans 类型的特定模型类。MXBeans 提供了一种方便的方式将相关值捆绑在一起，而无需客户端特别配置以处理这些捆绑。

与标准 MBeans 一样，MXBean 是通过编写一个名为 `SomethingMXBean` 的 Java 接口和实现该接口的 Java 类来定义的。但是，与标准 MBeans 不同，MXBeans 不要求 Java 类的名称为 `Something`。接口中的每个方法都定义了 MXBean 中的属性或操作。注解 `@MXBean` 也可以用于注解 Java 接口，而不需要接口的名称后跟 MXBean 后缀。

MXBeans 存在于 Java 2 Platform, Standard Edition (J2SE) 5.0 软件中，位于 [`java.lang.management`](https://docs.oracle.com/javase/8/docs/api/java/lang/management/package-summary.html) 包中。然而，用户现在可以定义自己的 MXBeans，除了在 `java.lang.management` 中定义的标准集之外。

MXBeans 的主要思想是，例如在 MXBean 接口中引用的 [`java.lang.management.MemoryUsage`](https://docs.oracle.com/javase/8/docs/api/java/lang/management/MemoryUsage.html) 这样的类型，在本例中是 [`java.lang.management.MemoryMXBean`](https://docs.oracle.com/javase/8/docs/api/java/lang/management/MemoryMXBean.html)，被映射到一组标准类型，即所谓的 *Open Types*，这些类型在 [`javax.management.openmbean`](https://docs.oracle.com/javase/8/docs/api/javax/management/openmbean/package-summary.html) 包中定义。确切的映射规则出现在 MXBean 规范中。然而，一般原则是简单类型如 int 或 String 保持不变，而复杂类型如 `MemoryUsage` 被映射为标准类型 [`CompositeDataSupport`](https://docs.oracle.com/javase/8/docs/api/javax/management/openmbean/CompositeDataSupport.html)。

MXBean 示例包括以下文件，这些文件位于 `jmx_examples.zip` 中：

+   `QueueSamplerMXBean` 接口

+   实现 MXBean 接口的 `QueueSampler` 类

+   `QueueSample` 是由 MXBean 接口中的 `getQueueSample()` 方法返回的 Java 类型

+   `Main`，设置并运行示例的程序

MXBean 示例使用这些类执行以下操作：

+   定义了一个管理 `Queue<String>` 类型资源的简单 MXBean

+   在 MXBean 中声明一个 getter，`getQueueSample`，当调用时获取队列的快照并返回一个捆绑以下值的 Java 类 `QueueSample`：

    +   获取快照的时间

    +   队列大小

    +   给定时间的队列头

+   在 MBean 服务器中注册 MXBean

## MXBean 接口

以下代码显示了示例 `QueueSamplerMXBean` MXBean 接口：

```java
package com.example; 

public interface QueueSamplerMXBean { 
    public QueueSample getQueueSample(); 
    public void clearQueue(); 
} 

```

请注意，声明 MXBean 接口的方式与声明标准 MBean 接口的方式完全相同。`QueueSamplerMXBean` 接口声明了一个 getter，`getQueueSample` 和一个操作，`clearQueue`。

## 定义 MXBean 操作

MXBean 操作在 `QueueSampler` 示例类中声明如下：

```java
package com.example; 

import java.util.Date; 
import java.util.Queue; 

public class QueueSampler 
                implements QueueSamplerMXBean { 

    private Queue<String> queue; 

    public QueueSampler (Queue<String> queue) { 
        this.queue = queue; 
    } 

    public QueueSample getQueueSample() { 
        synchronized (queue) { 
            return new QueueSample(new Date(), 
                           queue.size(), queue.peek()); 
        } 
    } 

    public void clearQueue() { 
        synchronized (queue) { 
            queue.clear(); 
        } 
    } 
} 

```

`QueueSampler` 定义了由 MXBean 接口声明的 `getQueueSample()` getter 和 `clearQueue()` 操作。`getQueueSample()` 操作返回一个 `QueueSample` Java 类型的实例，该实例是使用 [`java.util.Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html) 方法 `peek()` 和 `size()` 返回的值以及 [`java.util.Date`](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html) 的实例创建的。

## 定义 MXBean 接口返回的 Java 类型

`QueueSampler` 返回的 `QueueSample` 实例在 `QueueSample` 类中定义如下：

```java
package com.example; 

import java.beans.ConstructorProperties; 
import java.util.Date; 

public class QueueSample { 

    private final Date date; 
    private final int size; 
    private final String head; 

    @ConstructorProperties({"date", "size", "head"}) 
    public QueueSample(Date date, int size, 
                        String head) { 
        this.date = date; 
        this.size = size; 
        this.head = head; 
    } 

    public Date getDate() { 
        return date; 
    } 

    public int getSize() { 
        return size; 
    } 

    public String getHead() { 
        return head; 
    } 
}   

```

在 `QueueSample` 类中，MXBean 框架调用 `QueueSample` 中的所有 getter 将给定实例转换为一个 [`CompositeData`](https://docs.oracle.com/javase/8/docs/api/javax/management/openmbean/CompositeData.html) 实例，并使用 `@ConstructorProperties` 注解从 `CompositeData` 实例重建一个 `QueueSample` 实例。

## 创建并在 MBean 服务器中注册 MXBean

到目前为止，已经定义了以下内容：一个 MXBean 接口和实现它的类，以及返回的 Java 类型。接下来，MXBean 必须在 MBean 服务器中创建并注册。这些操作由相同的 `Main` 示例 JMX 代理执行，该代理在标准 MBean 示例中使用，但相关代码未在 标准 MBean 课程中显示。

```java

package com.example; 

import java.lang.management.ManagementFactory; 
import java.util.Queue; 
import java.util.concurrent.ArrayBlockingQueue; 
import javax.management.MBeanServer; 
import javax.management.ObjectName; 

public class Main { 

    public static void main(String[] args) throws Exception { 
        MBeanServer mbs = 
            ManagementFactory.getPlatformMBeanServer(); 

        ...  
        ObjectName mxbeanName = new ObjectName("com.example:type=QueueSampler");

        Queue<String> queue = new ArrayBlockingQueue<String>(10);
        queue.add("Request-1");
        queue.add("Request-2");
        queue.add("Request-3");
        QueueSampler mxbean = new QueueSampler(queue);

        mbs.registerMBean(mxbean, mxbeanName);

        System.out.println("Waiting..."); 
        Thread.sleep(Long.MAX_VALUE); 
    } 
} 

```

`Main` 类执行以下操作：

+   获取平台 MBean 服务器。

+   为 MXBean `QueueSampler` 创建对象名称。

+   为 `QueueSampler` MXBean 创建一个 `Queue` 实例以进行处理。

+   将 `Queue` 实例提供给新创建的 `QueueSampler` MXBean。

+   以与标准 MBean 完全相同的方式在 MBean 服务器中注册 MXBean。

## 运行 MXBean 示例

MXBean 示例使用了您在 标准 MBeans 部分中使用的 `jmx_examples.zip` 包中的类。此示例需要 Java SE 平台的第 6 版本。要运行 MXBeans 示例，请按照以下步骤操作：

1.  如果尚未这样做，请将`jmx_examples.zip`保存到`work_dir`目录中。

1.  在终端窗口中使用以下命令解压示例类的捆绑包。

    ```java
    unzip jmx_examples.zip

    ```

1.  从`work_dir`目录中编译示例 Java 类。

    ```java
    javac com/example/*.java

    ```

1.  启动`Main`应用程序。生成一个确认信息，表示`Main`正在等待某些事件发生。

    ```java
    java com.example.Main

    ```

1.  在同一台机器的不同终端窗口中启动 JConsole。显示新连接对话框，展示可以连接的运行中的 JMX 代理列表。

    ```java
    jconsole

    ```

1.  在新连接对话框中，从列表中选择`com.example.Main`并点击连接。

    显示平台当前活动的摘要。

1.  点击 MBeans 选项卡。

    此面板显示当前在 MBean 服务器中注册的所有 MBeans。

1.  在左侧框架中，展开 MBean 树中的`com.example`节点。

    你会看到示例 MBean `QueueSampler`是由`Main`创建和注册的。如果你点击`QueueSampler`，你会在 MBean 树中看到其关联的属性和操作节点。

1.  展开属性节点。

    你会看到`QueueSample`属性出现在右侧窗格中，其值为`javax.management.openmbean.CompositeDataSupport`。

1.  双击`CompositeDataSupport`值。

    你会看到`QueueSample`值`date`、`head`和`size`，因为 MXBean 框架已将`QueueSample`实例转换为`CompositeData`。如果你将`QueueSampler`定义为标准 MBean 而不是 MXBean，JConsole 将找不到`QueueSample`类，因为它不在其类路径中。如果`QueueSampler`是标准 MBean，当检索`QueueSample`属性值时，你会收到`ClassNotFoundException`消息。JConsole 找到`QueueSampler`这一事实展示了在通过通用 JMX 客户端（如 JConsole）连接到 JMX 代理时使用 MXBeans 的实用性。

1.  展开操作节点。

    显示一个按钮来调用`clearQueue`操作。

1.  点击`clearQueue`按钮。

    显示成功调用方法的确认信息。

1.  再次展开属性节点，并双击`CompositeDataSupport`值。

    `head`和`size`值已重置。

1.  要关闭 JConsole，选择连接 -> 退出。
