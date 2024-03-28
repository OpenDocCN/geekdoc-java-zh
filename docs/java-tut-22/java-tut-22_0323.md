# 提供默认参数

> 原文：[`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/defaultarg.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/defaultarg.html)

当启动应用程序时，参数会传递给 Java 应用程序。自包含应用程序可以设置一个默认参数，当未指定参数时使用。使用`<fx:argument>`元素来定义参数。可以通过为每个参数添加一个`<fx:argument>`元素来传递多个参数。有关此元素的信息，请参阅[<fx:argument>](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html#JSDPG528)。

在使用文件关联中描述的文件关联演示设置为使用打包在应用程序中的示例文件之一的名称作为默认参数。

`build.xml`文件中`<fx:deploy>`任务中的以下代码显示了如何定义默认参数：

```java
<fx:application id="fileassociationdemo"
                name="File Association Demo"
                mainClass="${main.class}"
                version="1.0">
    <fx:argument>sample.js</fx:argument>
</fx:application>

```

查看`build.xml`以获取完整的构建代码。

您可以从自包含应用程序示例下载文件关联演示的源文件。

## 其他资源

有关默认参数的更多信息，请参阅[向自包含应用程序传递参数](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html#JSDPG995)。

有关 JavaFX Ant 参数的更多信息，请参阅[JavaFX Ant 任务参考](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html)。
