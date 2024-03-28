# 监控和管理 Java 虚拟机

> 原文：[`docs.oracle.com/javase/tutorial/jmx/overview/javavm.html`](https://docs.oracle.com/javase/tutorial/jmx/overview/javavm.html)

JMX 技术也可以用于监控和管理 Java 虚拟机（Java VM）。

Java VM 具有内置的仪器，使您可以使用 JMX 技术监控和管理它。这些内置的管理实用程序通常被称为 Java VM 的*开箱即用*管理工具。为了监控和管理 Java VM 的不同方面，Java VM 包括一个平台 MBean 服务器和专门的 MXBeans，供符合 JMX 规范的管理应用程序使用。

## 平台 MXBeans 和平台 MBean 服务器

*平台 MXBeans*是 Java SE 平台提供的一组 MXBeans，用于监控和管理 Java VM 和 Java 运行时环境（JRE）的其他组件。每个平台 MXBean 封装了 Java VM 功能的一部分，例如类加载系统、即时（JIT）编译系统、垃圾收集器等。这些 MXBeans 可以通过符合 JMX 规范的监控和管理工具显示和交互，以便您监控和管理这些不同的 VM 功能。其中一种监控和管理工具是 Java SE 平台的 JConsole 图形用户界面（GUI）。

Java SE 平台提供了一个标准的*平台 MBean 服务器*，其中注册了这些平台 MXBeans。平台 MBean 服务器还可以注册您希望创建的任何其他 MBeans。

## JConsole

Java SE 平台包括符合 JMX 规范的 JConsole 监控和管理工具。JConsole 使用 Java VM 的广泛仪器（平台 MXBeans）提供关于在 Java 平台上运行的应用程序的性能和资源消耗的信息。

## 开箱即用的管理实用程序在操作中

因为标准的实现 JMX 技术的监控和管理实用程序内置于 Java SE 平台中，您可以在不编写一行 JMX API 代码的情况下看到开箱即用的 JMX 技术在操作中。您可以通过启动 Java 应用程序，然后使用 JConsole 监控它来实现。

## 使用 JConsole 监控应用程序

本过程展示了如何监控记事本 Java 应用程序。在 Java SE 平台版本 6 之前的版本中，您需要使用以下选项启动要使用 JConsole 监控的应用程序。

```java
-Dcom.sun.management.jmxremote

```

然而，Java SE 6 平台提供的 JConsole 版本可以连接到支持 Attach API 的任何本地应用程序。换句话说，任何在 Java SE 6 HotSpot VM 中启动的应用程序都会被 JConsole 自动检测到，不需要使用上述命令行选项启动。

1.  启动记事本 Java 应用程序，使用以下命令在终端窗口中：

    ```java
    java -jar 
        *jdk_home*/demo/jfc/Notepad/Notepad.jar

    ```

    其中`jdk_home`是安装 Java 开发工具包（JDK）的目录。如果您没有运行 Java SE 平台的 6 版本，您将需要使用以下命令：

    ```java
    java -Dcom.sun.management.jmxremote -jar 
          *jdk_home*/demo/jfc/Notepad/Notepad.jar

    ```

1.  一旦 Notepad 打开，在另一个终端窗口中，使用以下命令启动 JConsole：

    ```java
    jconsole

    ```

    会显示一个新的连接对话框。

1.  在新连接对话框中，从本地进程列表中选择`Notepad.jar`，然后点击连接按钮。

    JConsole 会打开并连接到`Notepad.jar`进程。当 JConsole 打开时，您将看到与 Notepad 相关的监控和管理信息概览。例如，您可以查看应用程序消耗的堆内存量，应用程序当前运行的线程数，以及应用程序消耗的中央处理单元（CPU）容量。

1.  点击不同的 JConsole 选项卡。

    每个选项卡提供了关于 Notepad 运行的 Java 虚拟机不同功能区域的更详细信息。所有呈现的信息都是从本教程中提到的各种 JMX 技术 MXBeans 获取的。所有平台 MXBeans 都可以在 MBeans 选项卡中显示。MBeans 选项卡将在本教程的下一部分中进行讨论。

1.  要关闭 JConsole，选择连接 -> 退出。
