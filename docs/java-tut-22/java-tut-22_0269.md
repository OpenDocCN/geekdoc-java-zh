# 显示短状态字符串

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/showStatus.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/showStatus.html)

所有浏览器都允许 Java applets 显示一个简短的状态字符串。页面上的所有 Java applets 以及浏览器本身共享同一状态行。

永远不要将关键信息放在状态行中。如果许多用户可能需要该信息，请在 applet 区域内显示该信息。如果只有少数精通的用户可能需要该信息，请考虑将信息发送到标准输出（参见将诊断信息写入标准输出和错误流）。

状态行通常不太显眼，并且可能被其他 applets 或浏览器覆盖。因此，最好将其用于偶发的、短暂的信息。例如，一个加载多个图像文件的 applet 可能会显示当前正在加载的图像文件的名称。

Applets 使用[`showStatus`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html#showStatus-java.lang.String-)方法显示状态行，该方法从`Applet`类继承到`JApplet`类。

这里是其使用示例：

```java
showStatus("MyApplet: Loading image file " + file);

```

* * *

**注意：** 不要在状态行中放置滚动文本。浏览器用户会觉得这种行为非常恼人。

* * *
