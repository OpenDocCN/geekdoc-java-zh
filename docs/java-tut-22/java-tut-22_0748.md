# 教训：通知

> 原文：[`docs.oracle.com/javase/tutorial/jmx/notifs/index.html`](https://docs.oracle.com/javase/tutorial/jmx/notifs/index.html)

JMX API 定义了一种机制，使 MBeans 能够生成通知，例如，用于表示状态变化、检测到的事件或问题。

要生成通知，MBean 必须实现接口[`NotificationEmitter`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationEmitter.html)或扩展[`NotificationBroadcasterSupport`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationBroadcasterSupport.html)。要发送通知，您需要构造类[`javax.management.Notification`](https://docs.oracle.com/javase/8/docs/api/javax/management/Notification.html)或其子类（例如[`AttributeChangedNotification`](https://docs.oracle.com/javase/8/docs/api/javax/management/AttributeChangeNotification.html)）的实例，并将实例传递给[`NotificationBroadcasterSupport.sendNotification`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationBroadcasterSupport.html#sendNotification-javax.management.Notification)。

每个通知都有一个来源。来源是生成通知的 MBean 的对象名称。

每个通知都有一个序列号。当顺序很重要且存在通知被错误处理的风险时，可以使用此编号来对来自同一来源的通知进行排序。序列号可以为零，但最好对于来自给定 MBean 的每个通知递增。

`Hello` MBean 在标准 MBeans 中实际上实现了通知机制。但是，出于简单起见，该课程中省略了此代码。`Hello`的完整代码如下：

```java
package com.example;

import javax.management.*;

public class Hello
        extends NotificationBroadcasterSupport
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
        int oldSize = this.cacheSize;
        this.cacheSize = size;

        System.out.println("Cache size now " + this.cacheSize);

        Notification n = new AttributeChangeNotification(this,
                                sequenceNumber++, System.currentTimeMillis(),
                                "CacheSize changed", "CacheSize", "int",
                                oldSize, this.cacheSize);

        sendNotification(n);
    }

    @Override
    public MBeanNotificationInfo[] getNotificationInfo() {
        String[] types = new String[]{
            AttributeChangeNotification.ATTRIBUTE_CHANGE
        };

        String name = AttributeChangeNotification.class.getName();
        String description = "An attribute of this MBean has changed";
        MBeanNotificationInfo info = 
                new MBeanNotificationInfo(types, name, description);
        return new MBeanNotificationInfo[]{info};
    }

    private final String name = "Reginald";
    private int cacheSize = DEFAULT_CACHE_SIZE;
    private static final int DEFAULT_CACHE_SIZE = 200;
    private long sequenceNumber = 1;
}

```

此`Hello` MBean 实现扩展了`NotificationBroadcasterSupport`类。`NotificationBroadcasterSupport`实现了`NotificationEmitter`接口。

操作和属性的设置方式与标准 MBean 示例中的方式相同，唯一的例外是`CacheSize`属性的 setter 方法现在定义了一个值`oldSize`。此值记录了在设置操作之前`CacheSize`属性的值。

通知是从 JMX 类`AttributeChangeNotification`的实例`n`构造的，该类扩展了`javax.management.Notification`。通知是在`setCacheSize()`方法的定义中从以下信息构造的。这些信息作为参数传递给`AttributeChangeNotification`。

+   通知来源的对象名称，即`Hello` MBean，由`this`表示

+   一个序列号，即`sequenceNumber`，设置为 1，并逐渐增加

+   时间戳

+   通知消息的内容

+   已更改的属性名称，本例中为`CacheSize`

+   已更改的属性类型

+   旧属性值，在本例中为`oldSize`

+   新属性值，在本例中为`this.cacheSize`

然后，通知`n`被传递给`NotificationBroadcasterSupport.sendNotification()`方法。

最后，[`MBeanNotificationInfo`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanNotificationInfo.html)实例被定义，以描述 MBean 为给定类型的通知生成的不同通知实例的特征。在本例中，发送的通知类型是`AttributeChangeNotification`通知。

## 运行 MBean 通知示例

再次使用 JConsole 与`Hello` MBean 交互，这次是为了发送和接收通知。此示例需要 Java SE 平台的第 6 版。

1.  如果尚未这样做，请将`jmx_examples.zip`保存到您的`work_dir`目录中。

1.  在终端窗口中使用以下命令解压示例类的捆绑包。

    ```java
    unzip jmx_examples.zip

    ```

1.  从`work_dir`目录中编译示例 Java 类。

    ```java
    javac com/example/*.java

    ```

1.  启动`Main`应用程序。

    ```java
    java com.example.Main

    ```

    生成一个确认，表示`Main`正在等待某些事件发生。

1.  在同一台机器的不同终端窗口中启动 JConsole。

    ```java
    jconsole

    ```

    显示新连接对话框，呈现可以连接的正在运行的 JMX 代理列表。

1.  在新连接对话框中，从列表中选择`com.example.Main`并点击连接。

    显示您平台当前活动的摘要。

1.  点击 MBeans 选项卡。

    此面板显示当前在 MBean 服务器中注册的所有 MBeans。

1.  在左侧框架中，展开 MBean 树中的`com.example`节点。

    您会看到由`Hello`创建和注册的示例 MBean`Hello`。如果点击`Hello`，您会看到其在 MBean 树中的通知节点。

1.  展开 MBean 树中`Hello` MBean 的通知节点。

    请注意，面板为空白。

1.  点击订阅按钮。

    当前接收的通知数量（0）显示在通知节点标签中。

1.  展开 MBean 树中`Hello` MBean 的属性节点，并将`CacheSize`属性的值更改为 150。

    在启动`Main`的终端窗口中，会显示对此属性更改的确认。请注意，显示在通知节点中的接收通知数量已更改为 1。

1.  再次展开 MBean 树中`Hello` MBean 的通知节点。

    通知的详细信息将被显示。

1.  要关闭 JConsole，请选择连接 -> 退出。
