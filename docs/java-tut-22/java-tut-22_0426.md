# 如何创建启动画面

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/misc/splashscreen.html`](https://docs.oracle.com/javase/tutorial/uiswing/misc/splashscreen.html)

几乎所有现代应用程序都有启动画面。通常启动画面用于以下目的：

+   广告产品

+   在长时间启动时向用户指示应用程序正在启动

+   提供每次访问仅需一次的信息

Java 基础类（包括 Swing 和抽象窗口工具包（AWT））使开发人员能够在 Java 技术应用程序中创建启动画面。然而，由于启动画面的主要目的是向用户提供关于应用程序启动的反馈，因此应用程序启动和启动画面弹出之间的延迟应尽量小。在启动画面弹出之前，应用程序必须加载和初始化 Java™虚拟机（JVM）、AWT、Swing，有时还包括应用程序相关的库。由此产生的几秒钟的延迟使得使用基于 Java™技术的启动画面变得不太理想。

幸运的是，Java™ SE 6 提供了一种解决方案，允许应用程序在虚拟机启动之前更早地显示启动画面。Java 应用程序启动器能够解码图像并在一个简单的非装饰窗口中显示它。

启动画面可以显示任何`gif`、`png`或`jpeg`图像，具有透明度、半透明度和动画效果。下图展示了作为动画`gif`文件开发的 Java 应用程序启动画面的示例。

![Java 应用程序的启动画面](img/e2d491d5f28ff0986ee26f1b2c2b1242.png)

[`SplashScreen`](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html)类用于关闭启动画面、更改启动画面图像、获取图像位置或大小以及在启动画面中绘制。应用程序不能创建此类的实例。只能存在一个在此类中创建的单个实例，并且可以使用`getSplashScreen()`静态方法获取此实例。如果应用程序在启动时没有通过命令行或清单文件选项创建启动画面，则`getSplashScreen`方法返回 null。

通常，开发人员希望保持启动画面图像在屏幕上并在图像上显示一些内容。启动画面窗口具有一个带有 alpha 通道的覆盖层，可以使用传统的`Graphics2D`接口访问此覆盖层。

以下代码片段显示了如何获取`SplashScreen`对象，然后如何使用`createGraphics()`方法创建图形上下文：

```java
...
        final SplashScreen splash = SplashScreen.getSplashScreen();
        if (splash == null) {
            System.out.println("SplashScreen.getSplashScreen() returned null");
            return;
        }
        Graphics2D g = splash.createGraphics();
        if (g == null) {
            System.out.println("g is null");
            return;
        }
...

```

在`SplashDemo.java`文件中找到演示的完整代码。

* * *

**注意：**

SplashDemo 应用程序使用固定坐标来显示叠加信息。这些坐标是依赖于图像的，并且针对每个启动画面单独计算。

* * *

本机启动画面可以通过以下方式显示：

+   命令行参数

+   使用指定清单选项的 Java™存档（JAR）文件

## 如何使用命令行参数显示启动画面

要从命令行显示启动画面，请使用`-splash:`命令行参数。此参数是一个 Java 应用程序启动器选项，用于显示启动画面：

```java
java -splash:*<file name> <class name>*

```

* * *

**试试这个：**

1.  将``SplashDemo.java``文件保存在名为`misc`的目录中。

1.  按照以下方式编译文件：

    ```java
    javac misc/SplashDemo.java

    ```

1.  将``splash.gif``图像保存在`images`目录中。

1.  通过以下参数从命令行运行应用程序：

    ```java
    java -splash:images/splash.gif misc.SplashDemo

    ```

1.  等待直到启动画面完全显示。

1.  应用程序窗口出现。要关闭窗口，请从弹出菜单中选择文件|退出，或单击 X。

1.  将启动画面名称更改为一个不存在的图像，例如`nnn.gif`。按以下方式运行应用程序：

    ```java
    java -splash:images/nnn.gif misc.SplashDemo

    ```

1.  您将看到以下输出字符串：

    ```java
    SplashScreen.getSplashScreen() returned null

    ```

* * *

## 如何使用 JAR 文件显示启动画面

如果您的应用程序打包在 JAR 文件中，可以在清单文件中使用`SplashScreen-Image`选项显示启动画面。将图像放在 JAR 文件中，并在选项中指定路径如下：

```java
Manifest-Version: 1.0
Main-Class: *<class name>*
SplashScreen-Image: *<image name>*

```

* * *

**试试这个：**

1.  将``SplashDemo.java``文件保存在名为`misc`的目录中。

1.  按照以下方式编译文件：

    ```java
    javac misc/SplashDemo.java

    ```

1.  将``splash.gif``图像保存在`images`目录中。

1.  准备`splashmanifest.mf`文件如下：

    ```java
    Manifest-Version: 1.0
    Main-Class: misc.SplashDemo
    SplashScreen-Image: images/splash.gif

    ```

1.  使用以下命令创建一个 JAR 文件：

    ```java
    jar cmf splashmanifest.mf splashDemo.jar misc/SplashDemo*.class images/splash.gif

    ```

    有关 JAR 文件的更多信息，请参阅在 JAR 文件中打包程序页面中的使用 JAR 文件。

1.  运行应用程序：

    ```java
    java -jar splashDemo.jar

    ```

1.  等待直到启动画面完全显示。

1.  应用程序窗口出现。要关闭窗口，请从弹出菜单中选择文件|退出，或单击 X。

* * *

## 启动画面 API

`SplashScreen`类不能用于创建启动画面。只能存在在此类中创建的单个实例。

| 方法 | 目的 |
| --- | --- |
| [getSplashScreen()](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html#getSplashScreen--) | 返回用于 Java 启动画面控制的`SplashScreen`对象。 |
| [createGraphics()](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html#createGraphics--) | 为启动画面叠加图像创建一个图形上下文（作为`Graphics2D`对象），允许您在启动画面上绘制。 |
| [getBounds()](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html#getBounds--) | 返回闪屏窗口的边界作为一个`Rectangle`。 |
| [close()](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html#close--) | 关闭闪屏并释放所有相关资源。 |

## 使用 SplashScreen API 的示例

以下表格列出了使用闪屏的示例。

| 示例 | 描述位置 | 备注 |
| --- | --- | --- |
| `SplashDemo` | 本节 | 在打开应用程序窗口之前显示闪屏。 |
