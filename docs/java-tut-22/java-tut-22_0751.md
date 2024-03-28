# 创建自定义 JMX 客户端

> 原文：[`docs.oracle.com/javase/tutorial/jmx/remote/custom.html`](https://docs.oracle.com/javase/tutorial/jmx/remote/custom.html)

本教程中的前几课已经向您展示了如何创建 JMX 技术的 MBeans 和 MXBeans，并将它们注册到 JMX 代理中。然而，所有之前的示例都使用了现有的 JMX 客户端 JConsole。本课将演示如何创建您自己的自定义 JMX 客户端。

一个自定义 JMX 客户端的示例，`Client`，包含在 `jmx_examples.zip` 中。这个 JMX 客户端与之前课程中看到的相同的 MBean、MXBean 和 JMX 代理进行交互。由于 `Client` 类的大小，将在以下部分中逐块进行检查。

## 导入 JMX 远程 API 类

要能够从 JMX 客户端远程连接到运行的 JMX 代理，您需要使用 [`javax.management.remote`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/package-summary.html) 中的类。

```java
package com.example;
...

import javax.management.remote.JMXConnector;
import javax.management.remote.JMXConnectorFactory;
import javax.management.remote.JMXServiceURL;

public class Client {
...

```

`Client` 类将创建 [`JMXConnector`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnector.html) 实例，为此它将需要一个 [`JMXConnectorFactory`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnectorFactory.html) 和一个 [`JMXServiceURL`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXServiceURL.html)。

## 创建一个通知监听器

JMX 客户端需要一个通知处理程序，以便监听并处理可能由注册在 JMX 代理的 MBean 服务器中的 MBeans 发送的任何通知。JMX 客户端的通知处理程序是 [`NotificationListener`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationListener.html) 接口的一个实例，如下所示。

```java
... 

public static class ClientListener implements NotificationListener {

    public void handleNotification(Notification notification,
            Object handback) {
        echo("\nReceived notification:");
        echo("\tClassName: " + notification.getClass().getName());
        echo("\tSource: " + notification.getSource());
        echo("\tType: " + notification.getType());
        echo("\tMessage: " + notification.getMessage());
        if (notification instanceof AttributeChangeNotification) {
            AttributeChangeNotification acn =
                (AttributeChangeNotification) notification;
            echo("\tAttributeName: " + acn.getAttributeName());
            echo("\tAttributeType: " + acn.getAttributeType());
            echo("\tNewValue: " + acn.getNewValue());
            echo("\tOldValue: " + acn.getOldValue());
        }
    }
}    
...       

```

此通知监听器确定接收到的任何通知的来源，并检索通知中存储的信息。然后根据接收到的通知类型执行不同的操作。在这种情况下，当监听器接收到 [`AttributeChangeNotification`](https://docs.oracle.com/javase/8/docs/api/javax/management/AttributeChangeNotification.html) 类型的通知时，它将通过调用 `AttributeChangeNotification` 方法 `getAttributeName`、`getAttributeType`、`getNewValue` 和 `getOldValue` 获取已更改的 MBean 属性的名称和类型，以及其旧值和新值。

代码稍后将创建一个新的 `ClientListener` 实例。

```java

ClientListener listener = new ClientListener();

```

## 创建 RMI 连接器客户端

`Client` 类创建了一个 RMI 连接器客户端，配置为连接到在启动 JMX 代理 `Main` 时将要启动的 RMI 连接器服务器。这将允许 JMX 客户端与 JMX 代理进行交互，就好像它们在同一台机器上运行一样。

```java
...

public static void main(String[] args) throws Exception {

echo("\nCreate an RMI connector client and " +
    "connect it to the RMI connector server");
JMXServiceURL url = 
    new JMXServiceURL("service:jmx:rmi:///jndi/rmi://:9999/jmxrmi");
JMXConnector jmxc = JMXConnectorFactory.connect(url, null);
...        

```

正如你所看到的，`Client`定义了一个名为`url`的[`JMXServiceURL`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXServiceURL.html)，表示连接器客户端期望找到连接器服务器的位置。此 URL 允许连接器客户端从运行在本地主机端口 9999 上的 RMI 注册表中检索 RMI 连接器服务器存根`jmxrmi`，并连接到 RMI 连接器服务器。

识别了 RMI 注册表后，可以创建连接器客户端。连接器客户端`jmxc`是[`JMXConnector`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnector.html)接口的一个实例，通过[`JMXConnectorFactory`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnectorFactory.html)的`connect()`方法创建。在调用`connect()`方法时，传递了参数`url`和一个空的环境映射。

## 连接到远程 MBean 服务器

有了 RMI 连接，JMX 客户端必须连接到远程 MBean 服务器，以便可以通过远程 JMX 代理与其中注册的各种 MBeans 进行交互。

```java
...

MBeanServerConnection mbsc = 
    jmxc.getMBeanServerConnection();

...     

```

通过调用`JMXConnector`实例`jmxc`的`getMBeanServerConnection()`方法，创建了一个名为`MBeanServerConnection`的实例，命名为 mbsc。

现在，连接器客户端已连接到由 JMX 代理创建的 MBean 服务器，并且可以注册 MBeans 并对其执行操作，连接对双方完全透明。

要开始，客户端定义了一些简单的操作，以发现代理的 MBean 服务器中找到的信息。

```java
...

echo("\nDomains:");
String domains[] = mbsc.getDomains();
Arrays.sort(domains);
for (String domain : domains) {
    echo("\tDomain = " + domain);
}

...

echo("\nMBeanServer default domain = " + mbsc.getDefaultDomain());

echo("\nMBean count = " +  mbsc.getMBeanCount());
echo("\nQuery MBeanServer MBeans:");
Set<ObjectName> names = 
    new TreeSet<ObjectName>(mbsc.queryNames(null, null));
for (ObjectName name : names) {
    echo("\tObjectName = " + name);
}

...

```

客户端调用`MBeanServerConnection`的各种方法，以获取不同 MBeans 所在的域，MBean 服务器中注册的 MBeans 数量，以及它发现的每个 MBean 的对象名称。

## 通过代理执行远程 MBeans 上的操作

客户端通过创建一个 MBean **代理**，通过 MBean 服务器连接访问 MBean 服务器中的`Hello` MBean。这个 MBean 代理是客户端本地的，并模拟了远程 MBean。

```java

...

ObjectName mbeanName = new ObjectName("com.example:type=Hello");
HelloMBean mbeanProxy = JMX.newMBeanProxy(mbsc, mbeanName, 
                                          HelloMBean.class, true);

echo("\nAdd notification listener...");
mbsc.addNotificationListener(mbeanName, listener, null, null);

echo("\nCacheSize = " + mbeanProxy.getCacheSize());

mbeanProxy.setCacheSize(150);

echo("\nWaiting for notification...");
sleep(2000);
echo("\nCacheSize = " + mbeanProxy.getCacheSize());
echo("\nInvoke sayHello() in Hello MBean...");
mbeanProxy.sayHello();

echo("\nInvoke add(2, 3) in Hello MBean...");
echo("\nadd(2, 3) = " + mbeanProxy.add(2, 3));

waitForEnterPressed();

...

```

MBean 代理允许您通过 Java 接口访问 MBean，使您可以在代理上调用而不必编写冗长的代码来访问远程 MBean。在此处通过在[`javax.management.JMX`](https://docs.oracle.com/javase/8/docs/api/javax/management/JMX.html)类中调用`newMBeanProxy()`方法创建`Hello`的 MBean 代理，传递 MBean 的`MBeanServerConnection`、对象名称、MBean 接口的类名和`true`，表示代理必须表现为[`NotificationBroadcaster`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationBroadcaster.html)。JMX 客户端现在可以执行`Hello`定义的操作，就好像它们是本地注册的 MBean 的操作一样。JMX 客户端还添加了一个通知监听器，并更改了 MBean 的`CacheSize`属性，使其发送通知。

## 通过代理对远程 MXBeans 执行操作

您可以像创建 MBean 代理一样创建 MXBean 的代理。

```java
...

ObjectName mxbeanName = new ObjectName ("com.example:type=QueueSampler");
QueueSamplerMXBean mxbeanProxy = JMX.newMXBeanProxy(mbsc, 
    mxbeanName,  QueueSamplerMXBean.class);
QueueSample queue1 = mxbeanProxy.getQueueSample();
echo("\nQueueSample.Date = " + queue1.getDate());
echo("QueueSample.Head = " + queue1.getHead());
echo("QueueSample.Size = " + queue1.getSize());
echo("\nInvoke clearQueue() in QueueSampler MXBean...");
mxbeanProxy.clearQueue();

QueueSample queue2 = mxbeanProxy.getQueueSample();
echo("\nQueueSample.Date = " +  queue2.getDate());
echo("QueueSample.Head = " + queue2.getHead());
echo("QueueSample.Size = " + queue2.getSize());

...

```

如上所示，要为 MXBean 创建代理，您只需调用[`JMX.newMXBeanProxy`](https://docs.oracle.com/javase/8/docs/api/javax/management/JMX.html#newMXBeanProxy-javax.management.MBeanServerConnection-javax.management.ObjectName-java.lang.Class-)而不是`newMBeanProxy`。MXBean 代理`mxbeanProxy`允许客户端调用`QueueSample` MXBean 的操作，就好像它们是本地注册的 MXBean 的操作一样。

## 关闭连接

一旦 JMX 客户端获取了所需的所有信息，并在远程 JMX 代理的 MBean 服务器上执行了所有必要的操作，连接必须关闭。

```java

jmxc.close();

```

通过调用[`JMXConnector.close`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnector.html#close--)方法关闭连接。

### 运行自定义 JMX 客户端示例

此示例需要 Java SE 平台的 6 版本。要使用自定义 JMX 客户端`Client`远程监视`Main` JMX 代理，请按照以下步骤操作：

1.  如果尚未这样做，请将`jmx_examples.zip`保存到您的`work_dir`目录中。

1.  在终端窗口中使用以下命令解压示例类的捆绑包。

    ```java
    unzip jmx_examples.zip

    ```

1.  从`work_dir`目录中编译示例 Java 类。

    ```java
    javac com/example/*.java

    ```

1.  启动`Main`应用程序，指定暴露`Main`以进行远程管理的属性：

    ```java
    java -Dcom.sun.management.jmxremote.port=9999 \
         -Dcom.sun.management.jmxremote.authenticate=false \
         -Dcom.sun.management.jmxremote.ssl=false \ 
         com.example.Main

    ```

    生成一个确认，表明`Main`正在等待某些事件发生。

1.  在另一个终端窗口中启动`Client`应用程序：

    ```java
    java com.example.Client

    ```

    显示已获取`MBeanServerConnection`的确认。

1.  按 Enter 键。

    显示了由`Main`启动的 MBean 服务器中注册的所有 MBeans 所在的域。

1.  再次按 Enter 键。

    显示在 MBean 服务器中注册的 MBean 数量，以及所有这些 MBean 的对象名称。显示的 MBeans 包括在 Java VM 中运行的所有标准平台 MXBeans，以及由`Main`在 MBean 服务器中注册的`Hello` MBean 和`QueueSampler` MXBean。

1.  再次按 Enter 键。

    `Hello` MBean 的操作由`Client`调用，结果如下：

    +   向`Client`添加通知监听器，以侦听来自`Main`的通知。

    +   `CacheSize`属性的值从 200 更改为 150。

    +   在启动`Main`的终端窗口中，会显示`CacheSize`属性更改的确认信息。

    +   在启动`Client`的终端窗口中，显示来自`Main`的通知，通知`Client`有`CacheSize`属性更改。

    +   调用`Hello` MBean 的`sayHello`操作。

    +   在启动`Main`的终端窗口中，显示消息“Hello world”。

    +   调用`Hello` MBean 的`add`操作，参数为 2 和 3。结果由`Client`显示。

1.  再次按 Enter 键。

    `QueueSampler` MXBean 的操作由`Client`调用，结果如下：

    +   显示`QueueSample`值`date`、`head`和`size`。

    +   调用`clearQueue`操作。

1.  再次按 Enter 键。

    `Client`关闭与 MBean 服务器的连接，并显示确认信息。
