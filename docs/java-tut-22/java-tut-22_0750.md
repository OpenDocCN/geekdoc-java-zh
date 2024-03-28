# 通过 JConsole 将资源暴露给远程管理

> 原文：[`docs.oracle.com/javase/tutorial/jmx/remote/jconsole.html`](https://docs.oracle.com/javase/tutorial/jmx/remote/jconsole.html)

使用 JMX API 将您的 Java 应用程序暴露给远程管理可以非常简单，如果您使用开箱即用的远程管理代理和现有的监控和管理工具，如 JConsole。

要将您的应用程序暴露给远程管理，您需要使用正确的属性启动它。此示例展示了如何将`Main` JMX 代理暴露给远程管理。

* * *

**安全注意事项：**

为简单起见，此示例中禁用了身份验证和加密安全机制。但是，在实际环境中实现远程管理时，您应该实现这些安全机制。下一步是什么？ 提供了指向其他 JMX 技术文档的指针，显示如何激活安全性。

* * *

此示例需要 Java SE 平台的 6 版本。要远程监视`Main` JMX 代理，请按照以下步骤操作：

1.  如果尚未这样做，请将`jmx_examples.zip`保存到您的`work_dir`目录中。

1.  在终端窗口中使用以下命令解压示例类的捆绑包。

    ```java
    unzip jmx_examples.zip

    ```

1.  从`work_dir`目录中编译示例 Java 类。

    ```java
    javac com/example/*.java

    ```

1.  启动`Main`应用程序，指定暴露`Main`进行远程管理的属性。（对于 Windows，请使用插入符（`^`）而不是反斜杠（`\`）来将长命令分成多行）：

    ```java
    java -Dcom.sun.management.jmxremote.port=9999 \
         -Dcom.sun.management.jmxremote.authenticate=false \
         -Dcom.sun.management.jmxremote.ssl=false \
         com.example.Main

    ```

    生成一个等待某些事件发生的`Main`的确认。

1.  在**不同的机器**上的不同终端窗口中启动 JConsole：

    ```java
    jconsole

    ```

    显示新连接对话框，显示可以在本地连接的正在运行的 JMX 代理列表。

1.  选择远程进程，并在远程进程字段中输入以下内容：

    ```java
    *hostname*:9999

    ```

    在此地址中，`hostname`是运行`Main`应用程序的远程机器的名称，9999 是将连接到开箱即用的 JMX 连接器的端口号。

1.  点击连接。

    显示运行`Main`的 Java 虚拟机（Java VM）的当前活动摘要。

1.  点击 MBeans 选项卡。

    此面板显示当前在远程 MBean 服务器中注册的所有 MBean。

1.  在左侧框架中，在 MBean 树中展开`com.example`节点。

    您会看到由`Main`创建和注册的示例 MBean `Hello`。如果您点击`Hello`，即使它在不同的机器上运行，您也会看到其关联的属性和操作节点在 MBean 树中。

1.  要关闭 JConsole，请选择连接 -> 退出。
