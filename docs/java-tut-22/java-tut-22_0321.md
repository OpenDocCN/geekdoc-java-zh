# 使用文件关联

> 原文：[`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/fileassociation.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/fileassociation.html)

为用户提供自包含应用程序的一个优势是能够设置文件关联。 可以基于 MIME 类型或文件扩展名将特定类型的文件与您的应用程序关联起来，以便您的应用程序用于打开关联的文件。 例如，如果您的应用程序编辑文本文件，则可以设置一个文件关联，当用户双击扩展名为`.txt`的文件时运行您的应用程序。

File Association Demo 读取 JavaScript 和 Groovy 代码。 使用 MIME 类型和文件扩展名，应用程序与 JavaScript 和 Groovy 文件关联。

您可以从 Self-Contained Application Examples 下载 File Association Demo 的源文件。

## 设置文件关联

用于生成自包含应用程序捆绑包的 Ant 任务位于 File Association Demo 的`build.xml`文件中。 `<fx:association>` Ant 元素用于将文件扩展名或 MIME 类型与您的应用程序关联起来。 Linux 捆绑程序需要 MIME 类型，Windows 捆绑程序需要文件扩展名，OS X 捆绑程序至少需要其中一个属性。 最佳实践是同时使用 MIME 类型和文件扩展名，使 MIME 类型和文件扩展名之间实现一对一映射，这样您可以在多个平台上使用相同的构建文件。 有关此元素的更多信息，请参见[<fx:association>](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html#JSDPG997)。

以下代码显示了在`fx:deploy`元素中需要包含的内容，以将应用程序与扩展名`.js`和`.groovy`以及 MIME 类型`text/javascript`和`text/x-groovy`关联起来。

```java
<fx:info title="File Association Demo"
         vendor="MySamples"
         description="A Demo of File Associations for Java Packager"
         category="Demos"
         license="3 Clause BSD">
    <fx:association extension="js" mimetype="text/javascript" description="JavaScript Source"/>
    <fx:association extension="groovy" mimetype="text/x-groovy" description="Groovy Source"/>
</fx:info>

```

如果捆绑程序不支持文件关联，则将忽略关联。 截至 JDK 的 8u40 版本，Windows EXE 和 MSI 捆绑程序，Linux DEB 和 RPM 捆绑程序以及 Mac .app 捆绑程序支持文件关联。 OS X PKG 和 DMG 捆绑程序通过使用 Mac .app 捆绑程序支持文件关联。

请查看`build.xml`以获取完整的构建代码。

要生成 File Association Demo 的可安装捆绑包，请参见 Converting an Existing Application 中的“生成捆绑包”部分。

## 从关联文件启动

安装程序在将自包含应用程序捆绑包安装到用户系统时设置文件关联。 安装应用程序后，打开与您的应用程序关联的文件会导致启动您的应用程序。 启动应用程序所采取的操作取决于其运行的平台。

### 在 Linux 和 Windows 上启动

在 Linux 和 Windows 上，当基于文件关联启动应用程序时，被打开的文件作为参数传递给主类，该主类覆盖了类的默认参数。对于文件关联演示，参数在启动应用程序的实例后传递给`loadscript`方法。每打开一个文件，都会启动一个不同的应用程序实例。

请查看`ScriptRunnerApplication.java`以获取 Linux 和 Windows 版本的代码。

### 在 OS X 上启动

在 OS X 上，只运行一个应用程序实例。当打开关联文件时，会向应用程序发送一个事件。应用程序必须注册一个事件监听器来处理该事件。

OS X 的文件关联演示有一个子类，其主方法与 Linux 和 Windows 版本不同。该主方法处理默认参数的方式与 Linux 和 Windows 版本的主方法相同，然后向 OS X 注册一个监听器以处理`FileOpenHandler`。当打开关联文件时，此监听器的事件方法被调用，并且文件名从`OpenFilesEvent`对象的`getFiles`方法中提取。

请查看`ScriptRunnerApplicationMac.java`以获取 OS X 版本的代码。

构建 OS X 版本的文件关联演示需要访问随 Oracle JDK 提供的 OS X 特定类。大多数`com.apple.eawt`类不包含在`javac`编译器使用的符号文件中。为了告诉编译器忽略符号文件，在构建文件中的`-pre-init` Ant 任务中向`javac`编译器传递`-XDignore.symbol.file=true`参数。请参见`build.xml`。

## 关于文件关联演示的更多信息

文件关联演示项目包含了应用程序的 Java 源文件，位于`/src/sample/fa`目录中。自定义图标位于`/src/package/`platform``目录中。要与应用程序打包的示例文件位于`/src`目录中。

为了处理 Groovy 代码，文件关联演示需要 Groovy 库。构建过程会将 Groovy 库下载到`/lib`目录。有关信息，请参见添加外部库。

JAR 文件生成后，构建过程将`/src`和`/lib`目录复制到`/dist`目录。然后，`/dist`目录中包含了应用程序的所有文件。

文件关联演示接受文件名作为参数。如果应用程序是通过打开关联文件启动的，那么关联文件的名称将被传递进来。如果应用程序是直接启动的，则会传递应用程序捆绑的示例文件 `sample.js`。请参阅提供默认参数以获取更多信息。

需要管理员权限才能设置文件关联。默认情况下，Windows 的 EXE 安装程序不会请求管理员权限。为了强制请求文件关联演示的管理员权限，捆绑参数 `win.exe.systemWide` 被设置为 `true`。这个设置表示执行系统范围的安装，需要管理员权限。

文件关联演示在 Linux、OS X 和 Windows 上运行。演示设置为使用一个包含所有平台信息的单个构建文件。请参阅为所有平台使用通用构建文件以获取更多信息。

## 附加资源

欲了解更多关于文件关联的信息，请参阅[将文件与独立应用程序关联](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html#JSDPG996)。

欲了解关于 JavaFX Ant 参数的更多信息，请参阅[JavaFX Ant 任务参考](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html)。
