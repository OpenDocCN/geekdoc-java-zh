# 使用多个入口点

> 原文：[`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/multiplelaunchers.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/multiplelaunchers.html)

当您有一组相关的应用程序希望用户部署时，自包含应用程序非常有用。自包含应用程序提供一个单独的可安装捆绑包，安装所有应用程序和运行它们所需的 JRE。

多个启动器演示包括转换现有应用程序中描述的动态树演示和使用文件关联中描述的文件关联演示。项目的`/src`目录包含两个应用程序的源文件。

您可以从自包含应用程序示例下载多个启动器演示的源文件。

自包含应用程序的主要入口点由`<fx:application>`元素的`mainClass`属性标识。在多个启动器演示中，主要入口点是文件关联演示。主类为 Linux 和 Windows 的`sample.fa.ScriptRunnerAppliation`，或者 OS X 的`sample.fa.ScriptRunnerApplicationMac`。有关在跨平台使用单个构建文件时确定要使用的类的信息，请参见为所有平台使用通用构建文件。

每个次要入口点由`<fx:secondaryLauncher>`元素的一个实例标识。有关此元素的信息，请参见[<fx:secondaryLauncher>](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html#JSDPG1003)。

在多个启动器演示中，次要入口点是动态树演示。`build.xml`文件中的以下代码显示了如何定义第二个入口点：

```java
<fx:secondaryLauncher name="Dynamic Tree Demo"
    mainClass="webstartComponentArch.DynamicTreeApplication"
    version="1.0"
    title="Dynamic Tree Demo"
    vendor="My Company"
    description="A Demo of Multiple Launchers for JavaPackager"
    copyright="(c) 2014 My Company"
     menu="true"
     shortcut="false"
     >
</fx:secondaryLauncher>

```

请查看`build.xml`以获取完整的构建代码。

要为多个启动器演示生成可安装的捆绑包，请参见转换现有应用程序中的“生成捆绑包”部分。

当您安装自包含应用程序时，将安装具有多个启动器入口点的文件关联演示和具有自己入口点的动态树演示。例如，在 Windows 上，开始菜单中的`Java Demos`文件夹包含两个条目：动态树演示和多个启动器演示。请注意，为多个启动器入口点设置了文件关联，因此打开 JavaScript 或 Groovy 文件会启动多个启动器。

## 附加资源

有关多个入口点的更多信息，请参见[支持多个入口点](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html#JSDPG1000)。

关于 JavaFX Ant 参数的更多信息，请参阅[JavaFX Ant 任务参考](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html)。
