# 检索资源

> [`docs.oracle.com/javase/tutorial/deployment/webstart/retrievingResources.html`](https://docs.oracle.com/javase/tutorial/deployment/webstart/retrievingResources.html)

使用`getResource`方法从 JAR 文件中读取资源。例如，以下代码从 JAR 文件中检索图像。

```java
// Get current classloader
ClassLoader cl = this.getClass().getClassLoader();
// Create icons
Icon saveIcon  = new ImageIcon(cl.getResource("images/save.gif"));
Icon cutIcon   = new ImageIcon(cl.getResource("images/cut.gif"));

```

该示例假设应用程序的 JAR 文件中存在以下条目：

+   images/save.gif

+   images/cut.gif
