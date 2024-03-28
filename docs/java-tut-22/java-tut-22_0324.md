# 使用一个通用的构建文件适用于所有平台

> 原文：[`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/commonbuild.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/commonbuild.html)

要为应用程序在每个平台上生成一个自包含的应用程序包，必须在每个平台上运行打包工具。您可以选择使用特定于平台的构建文件或设置一个可以在所有平台上运行的构建文件。特定于平台的文件可能更容易设置，但您必须维护多个文件。

使用文件关联中描述的文件关联演示使用一个可以在所有平台上运行的单个构建文件。

构建文件的以下元素支持其在所有平台上的使用：

+   应用程序的主类为`ScriptRunnerApplication.java`，用于 Linux 和 Windows，以及`ScriptRunnerApplicationMac.java`，用于 OS X。在`-pre-init`任务中的以下代码用于确定使用哪个类：

    ```java
    <condition property="main.class" 
               value="sample.fa.ScriptRunnerApplication"
               else="sample.fa.ScriptRunnerApplicationMac">
        <not><os family="mac"/></not>
    </condition>

    ```

+   在`-pre-init`任务中的以下代码用于防止在 Linux 或 Windows 上运行时编译 OS X 的主类：

    ```java
    <condition property="excludes" value="**/*Mac.java">
        <not><os family="mac"/></not>
    </condition>

    ```

+   `<fx:bundleArgument>` 元素用于向不同的打包程序传递参数。未被打包程序使用的参数将被忽略，因此构建文件可以包含所有平台所需的参数。以下代码定义了 Linux、OS X 和 Windows 的参数：

    ```java
    <fx:bundleArgument arg="classpath" value="FileAssociationsDemo.jar lib/groovy-all-2.3.8.jar"/>

    <fx:bundleArgument arg="win.exe.systemWide" value="true"/>

    <fx:bundleArgument arg="linux.bundleName" value="file-association-demo"/>
    <fx:bundleArgument arg="email" value="maintainer@example.com"/>
    <fx:bundleArgument arg="mac.CFBundleName" value="File Assoc Demo"/>
    <fx:bundleArgument arg="win.menuGroup" value="Java Demos"/>

    ```

请查看`build.xml`以获取完整的构建代码。

您可以从自包含应用程序示例下载文件关联演示的源文件。
