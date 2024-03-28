# 与服务器端应用程序一起工作

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/server.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/server.html)

Java 小程序与其他 Java 程序一样，可以使用`java.net`包中定义的 API 在网络上进行通信。Java 小程序可以与在与小程序相同主机上运行的服务器应用程序进行通信。这种通信不需要服务器上的任何特殊设置。

* * *

**注意：**根据小程序加载到的网络环境以及运行小程序的浏览器，小程序可能无法与其来源主机通信。例如，在防火墙内部主机上运行的浏览器通常无法获取有关防火墙外部世界的许多信息。因此，一些浏览器可能不允许小程序与防火墙外部主机通信。

* * *

当小程序部署到 Web 服务器时，请使用`Applet`的`getCodeBase`方法和`java.net.URL`的`getHost`方法来确定小程序来自哪个主机，如下所示：

```java
String host = getCodeBase().getHost();

```

如果小程序部署在本地，`getCodeBase`方法将返回 null。建议使用 Web 服务器。

在获得正确的主机名后，您可以使用自定义网络教程中记录的所有网络代码。

* * *

**注意：**并非所有浏览器都完美支持所有网络代码。例如，一个与 Java 技术兼容的广泛使用的浏览器不支持向 URL 发布内容。

* * *

有关实现作为网络客户端的小程序的示例，请参见网络客户端小程序示例。
